gitHub 테스트 페이지입니다.

-- 자주 사용하는 명령

# Git Repository 변경사항 가져오기

다른 사람이 포스팅을 한 후 또는 설정 파일 변경 등 Repository의 파일이 수정된 후에는, 기존의 내 컴퓨터 내부에 로컬 Repository와 원격 Repository를 동기화 시켜줘야합니다.
이때 `pull`을 통해 동기화 시킬 수 있어요. 

Open terminal → 아래 command 입력

```bash
$ git pull origin master
```


# Git에 Commit / Push

포스팅 또는 페이지 작성, 기타 파일 수정 후에 Repository에 올리는 방법입니다. 

```bash
$ git add .
   
$ git commit -m "commit message"
  
$ git push origin [git username]
```

[포스트 작성연습-1](2020-03-17-포스트-작성연습-1.md)<br/>
[2020-03-15-DB-Pool-For-Event](2020-03-15-DB-Pool-For-Event.md)<br/>