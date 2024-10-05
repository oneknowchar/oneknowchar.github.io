---
title: '원격 저장소란? 그리고 자주쓰는 명령어들'
excerpt: 'github remote repository를 활용하여 소스를 백업할 수 있습니다.'

published: true

categories: git&github
tags: git&github

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

# Remote repository란?

원격에 있는 저정소를 의미하며,  
작업물들을 사용자의 로컬 컴퓨터가 아닌, 원격 저장소에 `백업(Back up)`할 수 있습니다.  
원격에 저장소를 두어 여러 개발자가 접근하여 `협업(Cooperation)`을 할 수 있습니다.

# Github?

로컬에 깃을 사용하듯, 인터넷 상에서 깃을 사용하는 프로그램입니다.  
깃허브에는 유료와 무료계정이 존재하며,  
무료계정의 경우 3명까지 협업할 수 있습니다.

# 로컬 저장소를 원격 저장소에 연결

## 테스트 준비

로컬 저장소를 만들고 테스트를 위해 파일을 생성하겠습니다.

```
$ mkdir git-my-remote   //원격저장소와 연결할 로컬 저장소 생성
$ cd git-my-remote      //로컬 저장소 접근
$ git init              //깃 초기화
$ vim test.txt          //원격 저장소 업로드를 위한 테스트 파일 생성
```

깃허브에 "git-remote" 이름의 원격 저장소를 만들었습니다

![](/images/2024-05-27/2024-05-27-13-03-24.png)  
_git-remote 원격 저장소를 생성합니다._

지역 저장소와 원격 저장소를 연결하겠습니다.

## Git remote add origin

그 다음 "git-remote" 저장소 화면에서 알려준  
명령어를 사용하여 로컬과 원격 저장소를 연결할 수 있습니다.

```
// push an existing repository from the command line
// 커맨드라인에서 기존 저장소를 푸시한다. (로컬->원격)
// 깃에 원격 저장소를 붙히는데 이름은 origin이고 주소는 ~~~ 이다.
$ git remote add origin https://github.com/oneknowchar/git-remote.git
```

여기서 origin은 원격저장소를 의미합니다.(깃허브 주소)  
깃에서 기본 브랜치를 _master_ 라고 하듯 깃허브 원격 저장소는 *origin*을 사용합니다.

![](/images/2024-05-27/2024-05-27-13-26-10.png)  
_원격 저장소를 연결하는 명령어 사용 후 오류가 안 뜬다면 성공입니다._

## Git remote -v

원격 저장소와 잘 연결되었는 지 확인할 수 있는 옵션입니다.

![](/images/2024-05-27/2024-05-27-13-28-17.png)  
_위와 같이 fetch와 push 주소가 떴다면 성공입니다._

# 원격 저장소에 올리기

## git push

방금 막 remote add origin {깃허브 주소}로 원격 브랜치를 받아왔다면 ...

```
// -u 명령어는 로컬 저장소를 원격 저장소 업스트림(추적) 참조한다. 즉 연결해준다는 뜻입니다.
$ git push -u origin {브랜치명}
```

![](/images/2024-06-15/2024-06-15-21-37-08.png)

그 다음부턴 작업 커밋 후
"git push" 명령어로 쉽게 원격저장소에 푸시할 수 있습니다.  
-u 명령어로 업스트림 연결을 하지 않았을 경우 매번 아래와 같이 푸시해야합니다.

```
$ git push origin {브랜치명}
```

# 원격 저장소에서 내려 받기

## git fetch

git fetch 명령어는,
원격 브랜치의 작업 내역을 가져오는 명령어 입니다.  
신규 브랜치나 커밋 내역등을 가져올 때 사용합니다.  
git merge를 해야 작업물들을 로컬로 가져올 수 있으며 이는 git pull과 동일한 역할을 하게 됩니다.

[![](/images/2024-06-15/2024-06-15-22-22-03.png)](https://www.google.com/search?sca_esv=b39c937a77fb6461&sca_upv=1&sxsrf=ADLYWIK03Q2wtj7JX5q7FETneslourZVIQ:1718456780382&q=git+stage&udm=2&fbs=AEQNm0DmKhoYsBCHazhZSCWuALW8l8eUs1i3TeMYPF4tXSfZ98Z_XVxzNb13fp2atSe3aTZxh00P4RN46vKaeCU6lCwgokKSAjPDH2MlsSy-8gaDDUQaKgJ6WGNN_FX8vJhn-4awYk5Gk7BAEdii98OBeClKK9JjFHF7liK4K3tHwCkeIdyWNQJGJEedbu5ErfKv3dVRvt_95CfohgWcKP7_C0Z4J1v1EA&sa=X&ved=2ahUKEwjnmNOv1t2GAxUes1YBHcNkICwQtKgLegQIDxAB&biw=1920&bih=929&dpr=1#vhid=uE1Vzd8nswyAqM&vssid=mosaic)

_이미지를 클릭하면 구글 검색으로 이동됩니다._

> git fetch + git merge = git pull

제 경험에서, 한 가지 다른 점이 있다면  
git pull 사용시 충돌이 없다면, 머지할 필요가 없어서 커밋 내역이 생기지 않으며,  
git fetch를 한 경우엔 꼭 merge를해야 하므로 커밋 내역이 생긴다는 차이가 있습니다.

## git pull

git pull 명령어는 원격 저장소의 작업을 가져오는 명령어 입니다.  
원격 브랜치 로컬 저장소를 연결한 후,  
로컬 브랜치에 작업물을 추가, 변경, 삭제 했더라도  
원격 브랜치에 변경 사항이 없는 경우 아무런 일이 이러나지 않습니다.

원격 브랜치에 변경 사항이 생겨 커밋이 일어난 경우에만  
변화가 생기며 같은 라인에 변경 내역이 있다면, 충돌이 생길 수 도 있습니다.

## git clone

깃 클론은 아래의 명령어와 동일한 기능을합니다.

```
// 원격의 저장소를 로컬로 가져온다.
$ git remote add {remote repo Url} {branchName}

// 원격의 저장소를 로컬로 가져온다.
$ git clone {remote repo Url} {branchName}
```

서로 다른 점은??  
remote add 명령어는 `.git` 폴더가 존재하는 위치에서 사용할 수 있습니다.  
따라서 .git 파일이 없는 위치에서 해당 명령어를 사용한다면 에러 문구가 나타납니다.

clone 명령어는 원격 저장소를 가져오면서 .git 폴더도 같이 생성합니다.

![](/images/2024-06-16/2024-06-16-19-50-10.png)

# git 원격 저장소 주소 변경

```
git remote set-url origin https://github.com/oneknowchar/study-jpa.git
```

# git push 간편하게 브랜치명(main) 생략

```
git push --set-upstream origin main
```

# 로컬에서 커밋 취소하며 스테이징에 남기기

```
git reset --soft HEAD^
```

---

여기까지 깃& 깃허브의 Remote repository를 알아보았습니다.
