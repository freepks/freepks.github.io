---
title: "parallel direct path insert 배치가 온라인 서비스 성능에 영향을 준 사례"
last_modified_at: 2020-03-20T00:00:00-00:00
classes: single
categories:
  - oracle tuning
  - oracle trouble-shooting
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
  # Agile Categories : agile-overview, scrum-quickguide, agile-reference, agile-practices, agile-thingy
tags:
  - parallel direct path insert
  - oracle tuning
  - oracle trouble-shooting
  - performance
#	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
#   Agile: agile, scrum, lean, jira, confluence, release plan, sprint plan, backlog, review, retrospective, scrum master, product owner, scrum team, dev team,
author: freepks
excerpt: "parallel direct path insert 배치가 온라인 서비스 성능에 영향을 준 사례"
toc: false 
toc_sticky: false
---

## parallel direct path insert 배치가 온라인 서비스 성능에 영향을 준 사례

장애 당시 특정 배치업무에서 parallel direct path insert SQL 이 수행되었다.
해당 세션은 parallel dml 모드였고 target table 은 partitioning 되어 있었고 index 들이 있었다.
세션모드와 힌트에 따라 parallel direct path 모드로 수행되었으며, 해당 SQL 의 plan 을 보면 아래 테스트 예와 같이 인덱스 갱신(INDEX MAINTENANCE) step 이 있었다. 


[테스트용 예제]

--parallel dml 모드 on
alter session enable parallel dml;

--parallel query and parallel insert 수행
insert /*+parallel(t 8) */ into dir_test t
select /*+parallel(s 8) */* from dir_src s;

commit;


Execution Plan
<img src="https://freepks.github.io/images/index_maintenance_1.PNG" width="600">

direct path insert 에서는 테이블 세그먼트에 대한 적재작업이 먼저 수행되고 난 후에 인덱스 갱신(index maintenance) 작업이 일괄적으로 수행되게 되는데, 이때 인덱스 Leaf node split 이 발생되게 되며(Deacon 해당지표: LFsplt) 바로 이 때문에 redo log 발생량(Deacon 해당지표: Rdo[m])이 급격하게 많아지는 것이다.(parallel degree 가 높을수록 redo log 발생량은 많아짐.)<br/>
장애당시 디콘(Deacon) Instance Monitor 에서 초당 redo log 양이 200 mb 까지 높아지자 서비스 품질이 나빠지기 시작했다.(서버 IO 성능에 따라 다름. IO 성능이 안좋다면 초당 50 mb 만 되어도 장애가능)
<br/>

<img src="https://freepks.github.io/images/index_maintenance_2.PNG" width="400">

인덱스 갱신작업에 의해 redo log 발생 - 적색표시부분

<br/>
장애원인을 정리해 보면,<br/>
parallel direct path insert 작업중 index maintenance 단계에서 초당 redo log 양이 많아지자 부하가 몰린 Log Writer(LGWR) 에 병목이 발생되었고, 이로 인해 RAC 노드간 block 을 전송하거나 commit 을 수행할 때 log buffer 를 redo log file 로 flush 해야 하는 온라인 트랜잭션들의 응답시간도 따라서 길어지게 된 것으로 분석되었다.

발생된 대기 event 들은 아래와 같음.

             gcs log flush sync
             log file sync
             gc cr block busy
             gc current block busy
             gc buffer busy acquire

문제 재발방지를 위해 아래와 같은 대책들을 생각해 볼 수 있겠다.

1. parallel direct path insert 작업 전에 인덱스들을 drop 했다가 작업 후에 recreate 하는 방안

   - index 생성시 redo log 발생량은 미량이므로, 특히 NOLOGGING 옵션주면 거의 발생 않음
   - 속도 측면에서도 많은 이점이 있음

2. parallel degree 를 낮추거나 serial direct path(APPEND 힌트) 로 수행하는 방안

   - degree 를 낮추면 redo log 발생량이 낮아지게 되어 LGWR 부담이 감소됨
   - 전체 수행시간은 좀 길어지는 단점이 있음

3. conventional insert 로 수행하는 방안.

   - 테이블과 인덱스가 함께 insert 되므로 redo log 발생량이 급격히 증가되지 않고 일정수준을 유지함
   - 세션의 parallel dml mode 를 disable 시키고 APPEND 힌트를 빼면 conventional insert 가 됨
   - 전체 수행시간이 많이 길어지는 단점이 있음
   
4. one-SQL 로 처리하지 않고 looping SQL 로 처리하는 방안

   - redo log 발생량이 급격히 증가되지 않고 일정수준을 유지함
   - 수행시간이 길어지는 단점이 있음 

5. 배치수행시간 조정방안(최선)

   - 온라인 TR 들에게 영향이 적은 시간대로 이동

## 결론

direct path insert 는 테이블을 먼저 적재한 후에 일괄적으로 인덱스 갱신작업을 수행한다. 이때 redo log 발생량이 급격히 많아진다.(nologging 옵션을 주어도 인덱스 갱신때는 redo 발생을 피할 수 없음)
그런데 이걸 parallel 로 수행하면 redo log 발생량은 parallel degree 에 비례해서 더 많아지게 되고 IO 에는 부담이 가중되게 된다.<br/>
redo log 발생량이 급격히 많아지면 Lgwr(log writer process) 에 병목이 생기게 되어 다른 세션들까지도 영향을 받게 된다. 만일 RAC 환경이라면 LMSn(global cache service process) 에게까지 부담이 가게 되고 그 영향은 인스턴스 레벨까지도 확대될 수 있다.(**DBMS hang!**)<br/>
**기본은 온라인 서비스 시간에는 이런 direct path insert 방식의 배치작업은 수행하지 않는 것이다.** 시간조정이 어렵다면 배치 수행방식을 위에 제시한 방법들을 참고해서 변경할 필요가 있다.
direct path insert. 빠른만큼 단점도 있으니 잘 알고 사용해야 한다.

