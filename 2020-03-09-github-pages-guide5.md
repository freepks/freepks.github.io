---
title: "Github Pages 시작하기 5편"
last_modified_at: 2020-03-09T00:00:00-00:00
classes: single
categories:
  - github pages
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
  # Agile Categories : agile-overview, scrum-quickguide, agile-reference, agile-practices, agile-thingy
tags:
  - git
  - github
  - github pages
#	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
#   Agile: agile, scrum, lean, jira, confluence, release plan, sprint plan, backlog, review, retrospective, scrum master, product owner, scrum team, dev team,
author: Judy
excerpt: "Github Pages 초기 사용자를 위한 가이드 5편입니다. (포스팅 작성하고 업로드 하기)"
toc: false
---

이제 실제로 포스팅을 작성하고 업로드 해보는 방법을 배워보겠습니다.

먼저, Author을 등록하지 않으신 분은 반드시 Author를 등록 후, 이 가이드를 수행해주세요.

-> [Author등록 바로가기](https://engineering-skcc.github.io/github%20pages/github-pages-guide3/)

이미 등록되어있는 분들은 재등록 하지 않으셔도 됩니다.

1. Master branch 내용 가져오기
   Posting은 각각 개인의 brach에 업로드 한 후, 매일 운영자가 Master branch로 pull request하여 포탈에 반영하게됩니다.
   따라서, 업데이트 된 Master Branch를 개인 로컬 저장소로 가져와야 새로운 포스팅작성이 가능하겠죠?
   포스팅 작성 전, 반드시 Master Branch에서 업데이트된 내용을 가져와주세요!

```bash
$ git pull origin master
```

2. _draft 폴더 안에 post-draft.md 열기
3. post-draft.md 의 상단 내용 복사
   ![](https://engineering-skcc.github.io/assets/images/2020-03-09-17-55-36.png)
4. _post폴더 아래, Markdown 파일 생성
   _posts 폴더를 클릭하고, 상단 이미지의 파일 모양 아이콘을 눌러 파일을 생성하거나, 마우스 우클릭으로 파일 생성

    → **파일 이름은 반드시 YYYY-MM-DD-file-name.md 형식으로 생성**

    → **파일 이름 작성 시, 공백은 ' - ' 로 처리**

    ![](https://engineering-skcc.github.io/assets/images/2020-03-09-17-56-44.png)

5. 복사한 내용작성
   
   생성한 파일을 열어 draft에서 복사한 내용을 붙여넣기합니다.
   
   아래 코드 블럭 내용을 채워줍니다. (아래 항목은 모두 필수 입력 사항입니다.)

   " ← 와 같이 큰따옴표가 있는 경우, **큰따옴표는 절대 삭제하지 마세요.**
   
   categories와 tag는 여러 개 등록이 가능합니다. 
   
   입력 후 ex)의 내용은 반드시 삭제 해주세요.
   
   categories와 tag를 여러 개 등록할 경우, **tap이 아닌 space bar 2번으로 ' - ' 를 구분** 해주세요.

```
---
title: "Github Pages 시작하기 5편"
last_modified_at: 2020-03-09T00:00:00-00:00
classes: single
categories:
  - github pages
  # 카테고리는 반드시 아래 예시 중에 골라 입력해주세요.
  # 예시에 원하는 카테고리가 없을 시, 예시에 추가해주시고 이 문서를 commit&push 해주세요.
  # ex)Cloud, AI, Big Data, Culture, Spring, MSA
  # Agile Categories : agile-overview, scrum-quickguide, agile-reference, agile-practices, agile-thingy
tags:
  - git
  - github
  - github pages
#	태그는 자유롭게 개수 제한 없이 입력할 수 있습니다. 아래는 예시입니다.
# ex)Cloud: k8s, Docker, CloudZ, Azure, AWS, Google Cloud
#	  AI: Abril, Tensor Flow
#   Big Data: accuinsight+, QUTA
#   Agile: agile, scrum, lean, jira, confluence, release plan, sprint plan, backlog, review, retrospective, scrum master, product owner, scrum team, dev team,
author: Judy
excerpt: "Github Pages 초기 사용자를 위한 가이드 5편입니다. (포스팅 작성하고 업로드 하기)"
toc: true 
toc_sticky: true 
toc_label: "List" 
---
```

6. Contents작성
   [Markdown 문법정리](https://engineering-skcc.github.io/github%20pages/github-pages-guide4/)를 참고하여 내용을 작성해주세요.
   내용 작성 중 내가 작성한 내용이 어떻게 보일지 너무너무 궁금하시다면!
   아래 이미지의 빨간 동그라미 버튼을 눌러주세요!
   ![](https://engineering-skcc.github.io/assets/images/2020-03-09-18-00-33.png)

7. Git Repository commit & Push
   작성한 내용을 이제 원격 저장소에 업로드 해보겠습니다. 

```bash
$ git add .
   
$ git commit -m "commit message"
  
$ git push origin [git username]
```

*Master Branch에 Push하지 않았기 때문에 페이지에 바로 반영되지 않습니다. 

예)

![](https://engineering-skcc.github.io/assets/images/2020-03-09-18-01-37.png)

페이지에 반영시키기 위해선 master branch로 merge해야합니다.

branch가 뭔지.. master가 뭔지.. merge는 또 뭐고 commit은 뭐고 너무 어렵다구요?!

곧 git 관련 포스팅 2편을 작성할 예정이니, 조금만 기다려주세요.

그럼 멋진 포스팅 작성을 기대하며 마치겠습니다.

감사합니다~~

