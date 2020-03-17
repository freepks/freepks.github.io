---
title: "DBCP Connection Pool 연결 대기 지연 현상"
last_modified_at: 2020-03-15T00:00:00-00:00
classes: single
categories:
  - Cloud
  - Tomcat
  - Apache
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
  # Agile Categories : agile-overview, scrum-quickguide, agile-reference, agile-practices, agile-thingy
  # SWAT Categories: PerformanceTest, LoadRunner, SilkPerformer, k8s, Docker, kubernetes, devops, kafka, tomcat, apache, spring
tags:
  - 모바일 쇼핑몰 이벤트
  - Cloud
  - Tomcat
  - DBCP
  - Jennifer
  #	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
#   Agile: agile, scrum, lean, jira, confluence, release plan, sprint plan, backlog, review, retrospective, scrum master, product owner, scrum team, dev team,
author: hyeonwoo
excerpt: "DBCP DB Connection Pool에서 발생할 수 있는 연결 대기 지연 현상에 대해 알아보자."
toc: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_sticky: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_label: "List" #(toc 사용시-변경x, 사용하지 않을 시 삭제)
---
# DBCP Connection Pool 연결 대기 지연 현상
> 모바일 한정판 굿즈 판매 이벤트.<br/>대학교 수강신청.<br/>설, 추석 귀성 KTX 예매.<br/>온라인몰 일회용 마스크 판매.<br/>위와 같이 한정된 수량의 물품을 선착순으로 판매하거나 경품을 제공하기 위해 수행되는 이벤트가 있다.<br/>
이러한 이벤트는 동시에 많은 사용자가 순간적으로 몰리는 현상이 발생하기 때문에 아무리 여유있는 자원을 가진 시스템이라도 문제가 발생할 가능성이 높아진다.<br/>
특히 DB에서 데이터를 조회,수정, 생성하기 위해 사용하는 Connection Pool이 부족해지면 다른 자원이 아무리 여유가 있어도 장애가 발생하곤 한다.<br/>
하지만 경우에 따라서는 Pool이 부족한 것이 아니라 설정이 잘못되어 대기 현상이 발생하는 경우가 있다.<br/>
DBCP에서 설정 문제로 인한 응답지연 및 에러가 발생하는 경우에 대해 알아보기로 하자.<br/>

## 1.DB Connection Pool이 모자라!

 가장 흔하게 볼 수 있는 모습이다. 아끼면 똥된다는 말이 있다.
 시스템에서는 DB Connection Pool을 아낀다면 똥이 아니라 장애가 된다. 그래서 Pool은 모자라느니 남아 도는 게 낫다.
 
![DB Connection Pool이 부족해 응답시간이 튀는 모습(jennifer X-View)](https://engineering-skcc.github.io/assets/images/Pool1.png)

DB Pool이 모자랄 경우 위 그림 처럼 응답시간이 튈 수 있다.
새로운 DB 연결을 만드는 과정은 많은 시간을 필요로 한다. 따라서 처음에 여유있게 만들어서 유지하는 것이 정신 건강에 이롭다.

**DB Connection Pool 대기 중인 Thred의 Stack 정보** 
```
java.lang.Object.wait(Native Method)
org.apache.tomcat.dbcp.pool.impl.GenericObjectPool.borrowObject(GenericObjectPool.java:1123)
org.apache.tomcat.dbcp.dbcp.AbandonedObjectPool.borrowObject(AbandonedObjectPool.java:79)
org.apache.tomcat.dbcp.dbcp.PoolingDataSource.getConnection(PoolingDataSource.java:106)
org.apache.tomcat.dbcp.dbcp.BasicDataSource.getConnection(BasicDataSource.java:1044)
aries.runtime.tracer.a.a(SourceFile:121)
aries.runtime.tracer.b.t.getConnection(SourceFile:357)
aries.base.profile.ProfileSQL.getConnection(ProfileSQL.java:318)
aries.base.jdk.DataSource.getConnection(DataSource.java:75)
org.springframework.jdbc.datasource.DataSourceUtils.doGetConnection(DataSourceUtils.java:111)
org.springframework.jdbc.datasource.DataSourceUtils.getConnection(DataSourceUtils.java:77)
org.mybatis.spring.transaction.SpringManagedTransaction.openConnection(SpringManagedTransaction.java:81)
org.mybatis.spring.transaction.SpringManagedTransaction.getConnection(SpringManagedTransaction.java:67)
org.apache.ibatis.executor.BaseExecutor.getConnection(BaseExecutor.java:279)
org.apache.ibatis.executor.SimpleExecutor.prepareStatement(SimpleExecutor.java:72)
org.apache.ibatis.executor.SimpleExecutor.doQuery(SimpleExecutor.java:59)
org.apache.ibatis.executor.BaseExecutor.queryFromDatabase(BaseExecutor.java:267)
org.apache.ibatis.executor.BaseExecutor.query(BaseExecutor.java:137)
org.apache.ibatis.executor.CachingExecutor.query(CachingExecutor.java:96)
org.apache.ibatis.executor.CachingExecutor.query(CachingExecutor.java:77)
sun.reflect.GeneratedMethodAccessor82.invoke(Unknown Source)
sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
java.lang.reflect.Method.invoke(Method.java:606)
org.apache.ibatis.plugin.Invocation.proceed(Invocation.java:49)
com.cware.framework.core.dataaccess.plugins.StatmentPlugin.intercept(StatmentPlugin.java:74)
org.apache.ibatis.plugin.Plugin.invoke(Plugin.java:60)
com.sun.proxy.$Proxy21.query(Unknown Source)
org.apache.ibatis.session.defaults.DefaultSqlSession.selectList(DefaultSqlSession.java:108)
org.apache.ibatis.session.defaults.DefaultSqlSession.selectList(DefaultSqlSession.java:102)
org.apache.ibatis.session.defaults.DefaultSqlSession.selectOne(DefaultSqlSession.java:66)
sun.reflect.GeneratedMethodAccessor260.invoke(Unknown Source)
sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
java.lang.reflect.Method.invoke(Method.java:606)
org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:358)
com.sun.proxy.$Proxy18.selectOne(Unknown Source)
org.mybatis.spring.SqlSessionTemplate.selectOne(SqlSessionTemplate.java:163)
```

#### DBCP 주요설정(1.x 기준) 
***

|**설정**|**설명**|
|:---|:---|
|**initialSize**|BasicDataSource Class 생성 후 최초로 getConnection() 메서드를 호출할 때 Connection Pool에 채워 넣을 Connection의 개수|
|**maxActive**|동시에 사용할 수 있는 최대 Connection 개수(기본값:8). maxTotal(2.x)|
|**minIdle**|최소한으로 유지할 Connection의 개수(기본값:0)|
|**maxIdle**|Connection Pool에 반납할 때 최대로 유지될 수 있는 Connection의 개수(기본값:8)|

이벤트와 같이 사용자가 몰릴 것이 예상된다면 initialSize와 maxActive, maxIdle, minIdle 항목을 동일한 값으로 설정하는 것을 권장한다.

최대 Connection의 개수에 대한 설정이 적정한지에 대한 검증은 실제 뚜껑을 열어보기 전까지는 아무도 알 수 없다.
하지만 이에 대한 검증을 하지 못해 불안하다면 걱정 마시라!
우리팀에는 이러한 성능 검증을 완벽하게 해줄 수 있는 성능 테스트 전문가들이 있으니 주저하지 말고 연락하시면 된다.


## 2.기다릴 것인가? 말 것인가?

DB Connection Pool이 부족한 상황이 발생할 수 있다. 그렇다면 기다려야 할 것인가? 말 것인가? 그 것이 문제다.

### Pool이 할당될 때까지 기다리기
***
누군가 사용하고 난 뒤에는 DB Pool을 반드시 반납해야 한다. 그렇지 않으면 이 것도 장애로 이어진다. 예전에는 이런 문제가 참 많았었다.
정상적이라면 기다리면 Pool이 반납되고 줄을 서서 기다리고 있던 다음 사용자의 요청에 Connection을 주게 되어 DB 처리를 수행하게 된다.
그렇다면 얼마나 기다려야 하는 걸까? 5초? 10초? 60초?

|**설정**|**설명**|
|:---|:---|
|**maxWait**|60000(DB Connection이 부족할 경우 대기하는 시간:ms)|

사용자의 인내심은 그리 길지 않다. 그리고 대기한다고 해서 반드시 DB Connection을 할당 받는다는 보장은 없다.
위와 같이 60초 대기를 하도록 설정되어 있다면 전체 Thread들이 모두 대기하는 현상이 발생할 수 있다.
매진된 가게 앞에서 기다린다고 없던 물건이 생기진 않으니 말이다.
인터넷이나 모바일 사용자들은 요청이 지연되면 새로고침이나 브라우저를 닫고 새로 열어 새로운 요청을 보내는 경향이 있다.
이럴 경우 DB Pool을 대기하던 이전 요청은 사라지지 않고 시스템에 남아 있고 새로운 요청이 추가되게 된다. 
따라서 서버 입장에서는 최악이다. 기존 요청을 강제로 없앨 방법도 없다. 이는 웹요청의 특성 때문에 생기는 문제이다.
따라서 DB Pool을 길게 대기하도록 설정하는 것은 시스템을 장애로 인도하는 방법이 될 수 있다.

![DB Pool대기가 발생한 액티브 스택트레이스(Jennifer X-View)](https://engineering-skcc.github.io/assets/images/Pool2.png)
20초 이상 DB Pool을 대기(60초 대기 설정됨)

### 예외로 처리하기
***
Pool이 부족할 경우 기다리지 않고 바로 예외처리하는 경우다.
사용자 입장에선 날벼락일 수 있다. 기껏 입력하고 주문 버튼을 눌렀는데 에러 페이지가 뜬다면 육두 문자가 나올 수도 있는 상황이다.
하지만 운영자에게는 즉시 DB Connection Pool의 설정을 늘려서 장애를 사전에 막을 수 있는 시그널이 될 수 있다.
Pool을 대기하도록 설정했다면 APM 대시보드에서 빨갛게 차곡차곡 쌓여가는 액티브 서비스를 보게 될 것이다.


## 3.DB Pool을 너무 아끼지 말자!
예전에는 DB Connection Pool이 누수되는 경우가 종종 있어서 장애의 원인이 되곤 했다.
하지만 요즘은 개발자가 DB Pool의 처리에 개입하는 경우가 거의 없다. 그말은 결국 누수가 발생하는 경우가 거의 없다는 것이다.
그럼에도 불구하고 DB Pool을 애지중지하며 빌려준 Pool을 강제로 가져오려고 하는 분(?)들이 있다.

다음과 같은 설정이 있다.

|**설정**|**설명**|
|:---|:---|
|**removeAbandonedTimeout**|기본값 60초|
|**removeAbandoned**|DB Connectio이 열려만 있고 Connection.close() 메서드가 호출되지 않는 DB Connection을 임의로 닫는 기능.(기본값 false)|

60초 이상 놀고 있는 DB Connection에 대해 강제로 회수하겠다는 무서운 엄포(?)를 내리고 있다.
위 설정을 사용할 경우, 우리의 기대와는 다르게 다음과 같이 예상치 못한 문제가 발생할 수 있다.

#### 강제 반납대기 
***
**반납대기 현상이 발생한 Thread Stack 정보**
```
oracle.jdbc.driver.OraclePreparedStatement.clearParameters(OraclePreparedStatement.java:9167)
oracle.jdbc.driver.OraclePreparedStatementWrapper.clearParameters(OraclePreparedStatementWrapper.java:1366)
org.apache.tomcat.dbcp.dbcp.DelegatingPreparedStatement.clearParameters(DelegatingPreparedStatement.java:160)
org.apache.tomcat.dbcp.dbcp.PoolingConnection.passivateObject(PoolingConnection.java:349)
org.apache.tomcat.dbcp.pool.impl.GenericKeyedObjectPool.addObjectToPool(GenericKeyedObjectPool.java:1626)
org.apache.tomcat.dbcp.pool.impl.GenericKeyedObjectPool.returnObject(GenericKeyedObjectPool.java:1576)
org.apache.tomcat.dbcp.dbcp.PoolablePreparedStatement.close(PoolablePreparedStatement.java:96)
org.apache.tomcat.dbcp.dbcp.DelegatingStatement.close(DelegatingStatement.java:168)
org.apache.tomcat.dbcp.dbcp.DelegatingConnection.passivate(DelegatingConnection.java:426)
org.apache.tomcat.dbcp.dbcp.DelegatingConnection.close(DelegatingConnection.java:246)
org.apache.tomcat.dbcp.dbcp.PoolableConnection.reallyClose(PoolableConnection.java:122)
org.apache.tomcat.dbcp.dbcp.PoolableConnectionFactory.destroyObject(PoolableConnectionFactory.java:628)
org.apache.tomcat.dbcp.pool.impl.GenericObjectPool.invalidateObject(GenericObjectPool.java:1286)
org.apache.tomcat.dbcp.dbcp.AbandonedObjectPool.invalidateObject(AbandonedObjectPool.java:125)
org.apache.tomcat.dbcp.dbcp.AbandonedObjectPool.removeAbandoned(AbandonedObjectPool.java:158)
```
위와 같이 Thread Stack상에 강제 반납을 진행하려고 대기 중인 것을 볼 수 있다.
만약 사용자 요청이 몰리는 경우라면 DB Connection 요청을 먼저 처리하는 경우 수 십초 씩 대기하는 Threa들이 발생할 수 있다.

#### Connection Close 에러

*removeAbandoned의 대상이 되는 경우는 사용하지 않는 액티브 Connection이다. 그런데 DBCP에서는 ResultSet에서 데이터를 가져오는 과정은 Connection을 사용하는 것으로 Count하지 않는다.
따라서 조회 조건이 없거나 대량 데이터 조회하는 경우에는 SQL 수행시간과 관계없이 ResultSet에서 데이터를 가져오는 시간(Fetch Time)이 60초 이상인 경우라면 removeAbandoned의 대상이 되므로 해당 Connection은 강제로 Close되게 되는 것이다.*

해당 Connection이 강제로 Close될 경우 다음과 같은 에러가 발생한다.

**예외발생 내용**

**java.SQLRecoverablEexception : closed connection : next**
{: .notice--danger}

말 그대로 resultset.next() 메서드를 수행 중이던 Connection이 Closed 되었다는 것이다.
대량 조회를 처리하는 중에 Fetch 작업을 Connection 미사용으로 간주하여 removeAbandonedTimeout만큼 미사용인 경우 강제로 Close해버리는 것이다.
따라서 DB Connection 좀 아낄려고 하다간 초가삼간을 다 태울 수도 있다는 말이다.

위와 같이 SQL 수행시간이 너무 긴 경우 사용 제한을 걸고 싶다면 위 설정을 사용하지 말고 queryTimeout을 통해 제한을 거는 방법을 권장한다.


## 4.정리하며...
이상으로 DBCP Pool을 사용하면서 발생할 수 있는 응답 대기 지연 현상을 실제 사례와 연계하여 설명하였다.
DB Pool 설정은 성능에 직접적으로 영향을 주는 중요한 요인이므로 최적화된 설정 검증을 위해서는 반드시 성능 테스트를 수행할 것을 권장한다.
그리고 동일한 Pool이라고 하더라도 버전에 따라서 다른 현상이 나타날 수 있으므로 사용 환경에 따른 특성을 고려하여야 한다.
