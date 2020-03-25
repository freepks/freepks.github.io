gitHub 테스트 페이지입니다.

-- 자주 사용하는 명령

## Github Pages Repository Clone

git bash 및 terminal 프로그램을 실행하고, 아래와 같은 command를 입력해주세요.

```bash
$ git clone https://github.com/engineering-skcc/engineering-skcc.github.io.git
```

## Git Repository 변경사항 가져오기

다른 사람이 포스팅을 한 후 또는 설정 파일 변경 등 Repository의 파일이 수정된 후에는, 기존의 내 컴퓨터 내부에 로컬 Repository와 원격 Repository를 동기화 시켜줘야합니다.
이때 `pull`을 통해 동기화 시킬 수 있어요. 

Open terminal → 아래 command 입력

```bash
$ git pull origin master
```

Fork repository 를 사용할 때 original 과 local 을 동기화시키려면
```bash
$ git pull upstream master
```

## Git에 Commit / Push

포스팅 또는 페이지 작성, 기타 파일 수정 후에 Repository에 올리는 방법입니다. 

```bash
$ git add .
   
$ git commit -m "commit message"
  
$ git push origin [git username]
```

[포스트 작성연습-1](2020-03-17-포스트-작성연습-1.md)<br/>
[스베덴보리-01](스베덴보리-01.md)<br/>
[direct-path-read-01](2020-03-17-adaptive-direct-path-load.md)<br/>
[2020-03-18-전문순서역전현상_XA_RAC](2020-03-18-전문순서역전현상_XA_RAC.md)<br/>
[2020-03-19-성테-네트웍-병목사례](2020-03-19-성테-네트웍-병목사례.md)<br/>
[2020-03-20-direct_path_insert_온라인영향](2020-03-20-direct_path_insert_온라인영향.md)<br/>
[Fork를 이용한 Github 공동작업-0322.docx](Fork를 이용한 Github 공동작업-0322.docx)<br/>