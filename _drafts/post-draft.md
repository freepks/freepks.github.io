---
title: "제목을 입력하세요"
last_modified_at: 2020-xx-xxT00:00:00-00:00
classes: wide
categories:
  - 카테고리를 입력하세요
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
  # Agile Categories : agile-overview, scrum-quickguide, agile-reference, agile-practices, agile-thingy
  # SWAT Categories: PerformanceTest, LoadRunner, SilkPerformer, k8s, Docker, kubernetes, devops, kafka, tomcat, apache, spring
tags:
  - 태그를 입력하세요
  - 태그2
  - 태그3
#	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
#   Agile: agile, scrum, lean, jira, confluence, release plan, sprint plan, backlog, review, retrospective, scrum master, product owner, scrum team, dev team,
author: 작성자를 입력하세요.
excerpt: "포스팅에 관한 간단한 소개 메세지를 입력하세요"
toc: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_sticky: true #(toc를 사용하지 않을 경우에만 false로 변경)
toc_label: "List" #(toc 사용시-변경x, 사용하지 않을 시 삭제)
---
<!--title H1 // #의 개수로 H2, H3 ''' H6-->
# H1 
## H2
### H3
#### H4
##### H5
###### H6

text contents

<!-- 이미지 삽입: 클립보드에 복사된 이미지를 Ctrl+Alt+V로 삽입이 가능하며 Path 및 Naming 또한 자동으로 설정됩니다.
(MacOS : command+ option+ V)

또는 assets/images/ 폴더에 직접 이미지 파일 삽입 후, 해당 이미지 주소를 입력하세요
직접 삽입시 이미지 주소 : https://www.engineering-skcc.github.io/assets/images/filename
-->
<!-- 이미지 사이즈 조정하기-->
<!--

<img src="https://engineering-skcc.github.io/assets/images/filename" width="500">

-> width 를 바꿔서 사이즈 조정

<!-- 사이즈 조정한 사진 가운데 정렬

<center><img src="https://engineering-skcc.github.io/assets/images/filename" width="500"></center>

-->

![이미지 출처 링크](https://www.engineering-skcc.github.io/assets/images/filename을 입력하세요) 

<!-- 텍스트에 링크 삽입 -->

<!-- Bold Text : 아래 예시처럼 **text** 또는 텍스트 블럭 처리 후, Ctrl + B -->
**Exception Sample 예제** 

<!--취소줄 : 아래 예시처럼 텍스트를 ~~ 로 감싸주세요. -->
~~Client가 "localhost:8083/test/xxx" 요청 ( RequestMapping("/test"), GetMapping({id})인 경우)~~ 

<!-- Italic Text 아래 예시처럼 텍스트를 _ 로 감싸주세요. 또는 텍스트 블럭 처리 후, Ctrl + i -->
_This is italic text_

<!-- 두꺼운 Italic Text 아래 예시처럼 텍스트를 ***로 감싸주세요. -->
***This is Bold italic text*** 

→ 화살표는 복사해서 사용하세요 <!-- 화살표 -->

<!--숫자형 리스트 -->
1. 1번 리스트
: 내용
2. 2번 리스트
: 내용
3. 3번 리스트
: 내용
4. 4번 리스트
   1. 4-1번 리스트 (Tab하여 생성!)
   2. 4-2번 리스트
  <!--코드 블럭 시작 ```후 프로그래밍 언어를 입력하면 (java, c, sql 등 ) 코드블럭이 생성됩니다. 코드 블럭 끝에도 반드시 ```를 입력하세요!-->
   ```java 
    package com.skcc.demo.exceptionsample.context.exceptionhandle.apierror;
    import java.time.LocalDateTime;
    import lombok.Data;
    @Data
    public class ApiErrorDetail {

    private HttpStatus status;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy hh:mm:ss", timezone="Asia/Seoul")
    private LocalDateTime timestamp;
    private String message;
    private String debugMessage;
    private List<ApiSubError> subErrors;
    private ApiErrorDetail() {
        timestamp = LocalDateTime.now();
    }
   
   ```


<!--코드 블럭 끝 -->

**Tips** 팁박스입니다.
{: .notice--danger} <!--팁 박스 빨간색-->

**ProTip:** Be sure to remove `/docs` and `/test` if you forked Minimal Mistakes. These folders contain documentation and test pages for the theme and you probably don't want them littering up your repo.
{: .notice--info}<!--팁 박스 파란색-->
 
 
**Note:** The theme uses the [jekyll-include-cache](https://github.com/benbalter/jekyll-include-cache) plugin which will need to be installed in your `Gemfile` and added to the `plugins` array of `_config.yml`. Otherwise you'll throw `Unknown tag 'include_cached'` errors at build.
{: .notice--warning} <!--팁 박스 노란색-->

<!-- 리스트 -->
- CustomExceptionHandler처럼 Entity나 Business Logic의 특정 Exception 처리가 아닌, Validation/Binding 등 전역에서 발생할 수 있는 에러를 한 곳에 모아 같은 Format으로 처리.
- View와 연결 시키는 경우 return type을 ModelAndView로 설정
- Error View의 경우, Spring에서는 다음과 같은 Page가 Default로 설정되어 있음.

<!--구분점 쓰기 -->
* 구분 1
* 구분 2
  * 구분 1
  * 구분 2

<!-- 가로선 긋기-->
---

<!-- 표만들기 사이트 -->
<!-- 한 셀에 여러 줄 입력 시 Line breaks as <br> 반드시 체크!! -->

http://www.tablesgenerator.com/markdown_tables

<!--글씨 색 변경 -->
<span style="color:blue"> **내용 내용 내용** </span> 

<!--인용구 : 참고한 자료 및 사이트는 반드시 아래처럼 작성해주세요. -->



>Exception 처리 가이드 참고 자료:  
<https://www.mkyong.com/spring-boot/spring-rest-error-handling-example/> <!--링크 참조 기본 -->
<https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc>
<https://github.com/paulc4/mvc-exceptions.git>
  
>Logging 처리 참고 자료:  
<https://www.sangkon.com/hands-on-springboot-logging/>
<https://meetup.toast.com/posts/149>
