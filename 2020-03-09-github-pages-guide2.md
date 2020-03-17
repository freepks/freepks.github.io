---
title: "Github Pages 시작하기 2편"
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
excerpt: "Github Pages 초기 사용자를 위한 가이드 2편입니다. (포스팅에 필요한 Git command 알아보기)"
toc: true 
toc_sticky: true 
toc_label: "List" 
---

이번에는 Github Pages 에 포스팅 할 때에 필요한 Command를 알아볼게요.

Git Command는 훨씬 다양하지만, 이번에는 우선 Posting할 때 반드시 써야하고 가장 기본적인 Command 부터 알려드릴게요.

# Git에 Commit / Push

포스팅 또는 페이지 작성, 기타 파일 수정 후에 Repository에 올리는 방법입니다. 

```bash
$ git add .
   
$ git commit -m "commit message"
  
$ git push origin [git username]
```

# Git Repository 변경사항 가져오기

다른 사람이 포스팅을 한 후 또는 설정 파일 변경 등 Repository의 파일이 수정된 후에는, 기존의 내 컴퓨터 내부에 로컬 Repository와 원격 Repository를 동기화 시켜줘야합니다.
이때 `pull`을 통해 동기화 시킬 수 있어요. 

Open terminal → 아래 command 입력

```bash
$ git pull origin master
```

# git pages 업그레이드 (config.yml 수정 후 바로 적용 안 될때 사용)

아래 Command는 config.yml파일 수정 후 바로 적용이 안될 때 사용하는데요, 반드시 **관리자**만 사용하도록 합니다.
Command 문구에서 추측할 수 있듯이, **강제 Build** 이기 때문에 자칫하면 사이트 자체가 오류나는 최악의 경우가 생길 수도 있기 때문이에요.

```bash
$ git commit --allow-empty -m "Force Rebuild"
$ git push origin master
```