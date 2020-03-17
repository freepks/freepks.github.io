---
title: "Agile 프로젝트 Starting 퀵가이드(8)-스크럼 환경 구성"
last_modified_at: 2020-03-06T16:00:00-30:00
classes: single
categories:
  - agile-quickguide
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
tags:
  - agile
  - scrum
  - confluence
  - jira
#	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
author: magaretjo
excerpt: "릴리즈 계획은 무엇이며, 무엇을 릴리즈할 것인가?"
toc: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_sticky: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_label: "List" #(toc 사용시-변경x, 사용하지 않을 시 삭제)
---
<br>
## <span class="mg_title_1">스크럼 환경의 범위
  
스크럼 환경은 좁게는 앞에서 이야기한 의사소통 및 위험/이슈 관리를 위한 도구를 말하며, 넓게는 조직/프로젝트의 성숙도에 따라 테스트 자동화 및 CI/CD를 위한 빌드ㆍ배포 Pipeline 등의 DevOps Toolchain 연동 환경으로도 볼 수 있다. 
![](/assets/images/agile/agile-devops-toolchain.png) 


최근 트렌드는 DevOps를 모두 아우르는 양상을 보이고 있기는 하지만, 아직 많은 프로젝트 현장에서 빌드ㆍ배포 Pipeline을 포함한 DevOps Toolchain 구성은 어려워 보인다. 또한 프로젝트를 위한 빌드ㆍ배포 환경 구성은 기존의 운영 시스템 담당자와 사전에 협의할 사항이 많은 관계로 프로젝트 초반보다는 진행 과정 중 운영팀과 충분히 논의하여 커스터마이징하는 것이 좋다.
따라서, 애자일 프로젝트의 빠른 적용을 위한 내용을 소개할 목적으로 작성한 본 퀵가이드에서는 DevOps Toolchain은 제외하며, 다양한 이해관계자간 협업 및 정보 공유를 위한 목적에서의 의사소통, 위험/이슈 도구 활용 수준의 가이드만 제공하고자 한다. 

자, 이제 컨플루언스 / JIRA를 활용하여 스크럼 프로젝트를 위한 환경을 구성해보도록 하자.
먼저 해야 할 것은 프로젝트 공간을 할당받는 것이다. 아래 내용을 참고하여 컨플루언스 / JIRA 공간을 확보하시라.
<br>

## <span class="mg_title_1">컨플루언스/JIRA 공간 할당 

고객사의 컨플루언스/JIRA 담당자에게 요청하여 프로젝트용 공간을 할당받는 것을 추천한다. 프로젝트와 관계된 모든 이해관계자가 공유하는 공간이 되기 위해서는 각 1인당 고유 계정이 있어야 하고 제품 책임자를 포함하여 현업 담당자 및 프로젝트에 참여하는 외부 협력사 모두 고객사의 비밀유지 책임이 있으므로, 공개된 외부 사이트나 고객사와 관계없는 컨플루언스/JIRA 공간을 사용할 경우 내용 작성에 제약을 받을 것이다. 
또한 Network 보안 정책상 접근이 허용되지 않아 무용지물이 될 수 있다.

고객사에 컨플루언스/JIRA 시스템이 없고 관계사 프로젝트를 수행하는 경우에 한하여, 당사에서는 그룹 공용 Network 상에서 접근 가능한 컨플루언스/JIRA 서비스를 제공하고 있다. 

<https://group.skdevops.com> 포털에 접속하여 컨플루언스/JIRA 공간을 할당받기 바란다. 주관부서는 “Cloud그룹” (2020기준)이며, 신청방법은 포털에 안내되어 있다.

또한 프로젝트 공간과 함께 사용할 devops 계정도 신청해야 한다.
단, <https://group.skdevops.com>는 당사 직원과 당사 MIS에 등록된 파트너 직원까지만 사용 가능하며, 고객용 계정은 등록이 불가하다. 보다 상세한 정책은 관리자 계정(devops4u@sk.com)으로 문의하면 된다.


- **컨플루언스 / JIRA만 사용 가능한가?**
  -	스크럼 프로젝트를 위한 도구로써, Atlassian에서 나온 컨플루언스 / JIRA 제품이 외에도 Microsoft의  Teams, Slack, Redmine, Trello 및 아사나 등 다양한 의사소통 지원 및 협업 도구가 있다. 
  -	그러나 Atlassian 제품이 시중에서 가장 많이 사용되며, 실제로도 가장 사용하기 편하고 스크럼에 꼭 필요한 기능 및 효율적인 메타정보 관리로 사용자 취향의 프로젝트 관리 도구로써 커스터마이징이 원활하다. 
  -	다만 사용자 수에 따라 License 비용이 크게 증가하므로, 조직/프로젝트의 규모와 비용, 물리적인 소통 공간 등 현장 상황에 맞는 제품을 선택하도록 한다.
{: .notice--memo} 
<br>


## <span class="mg_title_1">컨플루언스/JIRA 기본 사용법
할당받은 컨플루언스/JIRA 공간을 자신의 프로젝트에 맞게 커스터마이징 하기 전에 먼저 기본적인 컨플루언스/JIRA사용법을 알아야 한다. 다음 링크에서 기본적인 사용법을 익히도록 하자. 


- 컨플루언스 구조의 이해, 컨플루언스 페이지 작성, 컨플루언스 주요 매크로 활용 방법 (링크, @, 체크박스) 
- <https://engineering-skcc.github.io/devops-tools/Jirasetting/>
- <https://engineering-skcc.github.io/devops-tools/jiraworkflow/>
- <https://engineering-skcc.github.io/devops-tools/jirascrumboard/>



## <span class="mg_title_1">컨플루언스 공간 구성
컨플루언스는 WYSIWYG 방식으로 사용법이 매우 직관적이며 편리하고, 쉽게 구성할 수 있다
다음 그림과 같이 왼쪽 공간을 활용하여 프로젝트용 폴더를 구성할 수 있다.
폴더의 구조는 프로젝트 규모/조직/마일스톤 또는 중요 정보에 따라 구성하되, 맨 상단에 공지사항이 오도록 하고, 맨 아래에는 기타 or 개인용 Working 폴더를 만들어 누구나 편하게 자료를 공유하도록 한다. 
**【 컨플루언스 활용 예시 1】**
 ![](/assets/images/agile/agile-confluence-1.png) 

프로젝트 공간의 홈페이지에 전체 또는 팀별 공지사항, Calendar를 통하여 일정 공유 뿐 아니라 회의록 및 위험/이슈 관리 등 프로젝트 구성원간 주고 받는 내용을 웹 페이지 형식으로 포스팅할 수 있다. 
**【 컨플루언스 활용 예시 2】**
 ![](/assets/images/agile/agile-confluence-2.png) 
 

또한 기존 파일서버에서 관리하던 PPT, Word와 같은 보고서 및 각종 파일에 대해서도 관련 설명과 함께 파일 첨부가 가능하며, 권한에 따라 페이지별로 접근제어도 가능하다.

컨플루언스는 쉽게 사용할 수 있으므로, 자세한 사용법은 여기서는 생략하겠다.

## <span class="mg_title_1">지라(JIRA) 공간 구성
Atlassian의 지라(JIRA)는 애자일 프로젝트에서 가장 선호하는 도구로 직관적이며 효율적인 일감 관리 기능을 제공한다. 프로젝트 전체의 일감 계획 및 진행상황 추적 및 팀 성능 분석 뿐 아니라 컨플루언스, Bamboo, BitBucket, Jenkins 등의 DevOps 도구와의 연계를 통한 통합적 관리가 가능하다.
다음은 지라를 사용한 일감관리 화면 예시이다.

**【 지라를 이용한 일감관리 예시 】**
  ![](/assets/images/agile/agile-jira-sample.png) 


위와 같이 스프린트별 일감 현황을 공유할 수 있는 화면을 “스크럼 보드”라고 하는데, 위와 같은 보드 화면을 갖기 위해서 대략 다음의 순서에 따라 작업을 하도록 한다.

-	JIRA 기본 이해하기
  1.	[JIRA시스템 용어](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#jira시스템-용어)
  2.	[JIRA시스템의 일감구조](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#jira시스템의-일감구조)
  3.	[JIRA 일감 유형](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#jira-일감-유형)
-	스크럼보드 만들기
  1.	[워크플로우 정의](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#워크플로우-정의)
  2.	[보드 만들기](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#보드-만들기)
-	JIRA 프로젝트의 일감 정의하기
  1.	[프로젝트 “구성요소” 정의](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#프로젝트-구성요소-정의)
  2.	[프로젝트 “일감” 등록](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#프로젝트-일감-등록)
  3.	[프로젝트 “스프린트" 생성](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#스프린트-생성) 
  4.	[스프린트별 일감 할당](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#스프린트별-일감-할당)
-	대시보드 만들기 
  1.	[데이터 필터 정의](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#데이터-필터-정의)
  2.	[가젯 구성](/agile-quickguide/Agile-QuickGuide-Appendix-JIRA/#가젯-구성)


***

<div class="mg_subject_1"><b>Related Posts : </b></div> 
<div class="mg_content_1">
<a href="/agile-quickguide/Agile-QuickGuide07-소통&품질/">QuickGuide 07</a> > 의사소통 및 품질 계획 
</div>
<br>

<div class="container">
    <div>
        <h1>생활코딩</h1>
    </div>
    <section class="content">
        <span class="mg_nav">
            <ul>
                <li>html</li>
                <li>css</li>
                <li>javascript</li>
            </ul>
        </span>
        <span class="mg_main">
            Eveniet doloremque animi maxime aliquid rem fugit dolor dignissimos! Quo, ut quod ab.
        </span>
        <span class="mg_etc">
            AD
        </span>
    </section>
    <div>
        <a href="https://opentutorials.org/course/1">홈페이지</a>
    </div>
</div>

< EOF >