---
title:  "깃이란? 깃의 기본 명령어들"
excerpt: "깃 설치부터 버전관리 그리고 버전 삭제까지 알아보겠습니다."

published: true 

categories: git&github
tags: git&github

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

# Git  이란? 
하나의 온라인 저장소를 기준으로 여러 사람이 파일을 추가, 수정, 삭제하면서  
`버전관리`, `백업`, `협업`을 도와주는  프로그램입니다.  

깃을 다루는 프로그램의 종류로 GitHub Desktop, Tortoise, SourceTree, CLI(명령 프롬프트) 등이 있습니다.  
깃은 리눅스 명령어를 사용합니다.  
[깃을 설치](https://git-scm.com/downloads)하게 되면 Git bash라는 프로그램이 함께 설치되어 여기서 리눅스 명령어를 사용할 수 있습니다.  


# Git 세팅
## Git 설치  
[https://git-scm.com/downloads](https://git-scm.com/downloads)

![](/images/2024-05-23/2024-05-23-14-18-10.png)  

*사용자의 OS에 맞는 Git을 설치합니다.*  

![](/images/2024-05-23/2024-05-23-14-20-29.png)  
*Git Bash 설치 완료*  

![](/images/2024-05-23/2024-05-23-14-06-47.png)  

*windows OS 환경에서 Linux CLI를 사용할 수 있습니다.*  


# Git Command Line

## 사용자 정보 입력  
깃은 버전을 저장할 때마다 사용자 정보를 같이 저장합니다.  
누가, 어떤 버전을, 언제, 왜 만들었는지 파악할 수 있습니다.  따라서 Git은 `버전관리`, `백업`, `협업`에 많은 도움이 됩니다.

```
// $ 표시는 타이핑하지 않습니다.
// global옵션은 사용자 정보를 전역으로 설정합니다.

$ git config --global user.name {사용자 이름 또는 깃허브 아이디}  
$ git config --global user.email {사용자 이메일 또는 깃허브 이메일}
```  

![](/images/2024-05-23/2024-05-23-14-36-54.png)  

*사용자의 이름과 이메일도 확인 할 수 있습니다.*


# 버전 관리
## git init  
깃을 사용해서 버전관리를 하려면, 깃을 위한 디렉토리를 설정해야합니다.  
이미 존재하는 폴더로 접근하던가, 새로운 폴더를 만들어 봅니다.  
```
$ mkdir git-study
$ cd git-study

$ git init
```
![](/images/2024-05-23/2024-05-23-16-37-02.png)  

*(master)라는 문구가 생겼습니다.*  

![](/images/2024-05-23/2024-05-23-16-39-11.png)  

*숨겨진 .git 폴더와 함께 자동으로 몇개의 파일들이 생성되었습니다.*  

## .git 이란?  
.git 는 내부에 stage와 repository를 저장하는 공간 입니다.  
깃에는 3가지의 영역 개념이 존재 합니다.  

1. 작업트리(working tree) : 파일 생성, 수정, 삭제 등 실제 업무가 일어나는 디렉토리  
2. 스테이지(stage) : 작업트리와 저장소 중간 영역으로 깃의 버전으로 만들 파일이 대기 하는 곳  
3. 저장소(repository) : 스테이지에서 대기하던 파일들을 버전을 만들고 저장하는 곳  


## git add && git status  
![](/images/2024-05-23/2024-05-23-16-52-48.png)

새로 만든 git-study 폴더에,  test.txt 파일을 만들고
test.txt 파일을 생성 후 "안녕하세요... " 문구를 추가했습니다.  


```
$ git status //깃 상태 조회 명령어
$ git add {파일명}  //수정한 파일을 스테이지에 추가
```
![](/images/2024-05-23/2024-05-23-16-54-22.png)  

1. git status명령어로 .git 폴더와 git-study 폴더를 비교합니다.  
2. stage 영역에 없고, 한 번도 버전 관리하지 않은 `Untracked`파일인 test.txt 파일이 발견되었습니다.
3. git add test.txt 명령어로 test.txt 파일을 stage에 담았습니다.
4. git status 깃 상태 조회를 하니 `new file : test.txt` 정상적으로 새로운 작업물이 stage에 담겼습니다.
5. git add 시에 `warning` 문구는 다른 OS 간에 문자 개행시 발생하는 오류입니다. 어떠한 조치를 해야 하는 것은 아니니 무시하셔도 됩니다.  

## git commit
### git commit -m
git add를 했다면, 이제 버전별로 관리할 준비가 되었다는 뜻 입니다.  


```
// -m :커밋시 메시지를 남기는 옵션입니다.
$ git commit -m "save test file into my repository"
```

![](/images/2024-05-24/2024-05-24-08-41-58.png)  

*스테이지에 있던 1개의 test.txt 파일이 repository에 추가되었습니다.*

### git commit -am  

한 번이라도 커밋한 내역이 있는 파일들은 매번 수정 후 git add할 필요 없이
-am 옵션으로 스테이징후 커밋까지를 한꺼번에 처리할 수 있습니다.  
다시 말해, 신규 파일은 항상 `git add`를 하고 커밋해야 한다는 뜻입니다.  

> 고민할것 없이 커밋을 하려면 git add > git commit -m "msg" 로 하면 됩니다.  


## git log  
```
git log --stat 
```
![](/images/2024-05-24/2024-05-24-08-45-12.png)  
해당 커밋의 고유 아이디와, 남긴 메시지를 확인할 수 있습니다.  
`(head->master)`는 사용자의 호스트 저장소의 최종 커밋을 의미합니다.  
나중에 나올 `(origin/master)`는 원격 저장소의 최종 커밋을 의미합니다.
 
--stat 로그 상세 옵션입니다. 

커밋시점에 변경된 파일명과 라인수 를 알수 있습니다.  
![](/images/2024-05-24/2024-05-24-09-38-27.png)

## git diff  
```
$ git diff
```
스테이지에 있는 수정된 파일과, 저장소에 있는 파일을 비교하여 다른점을 알려주는 명령어 입니다.  


# 작업 되돌리기  
작업 내용을 스테이징, 커밋 까지 했다면, 스테이징에서 내리거나 커밋을 취소할 수 도 있습니다.  

## git checkout  
> git checkout 의 기능이 너무 많기 때문에 `switch`나 `restore` 가 도입되었습니다.  
[참고 블로그](https://blog.outsider.ne.kr/1505)  

checkout: Switch branches or restore working tree files  
switch: Switch branches  
restore: Restore working tree files  

실제 깃 bash를 사용하더라도   
![](images/2024-05-28/2024-05-28-10-02-45.png)  
*restore 하라는 명령어만 존재할 뿐입니다.*  

>**더 이상 checkout 명령어를 직접적으로 사용할 필요가 없어졌습니다.**

원래  checkout은  작업을 수행했습니다.  

Git 에서 가장 다양하고 유용한 명령어 입니다. 그렇기 때문에 대신할 명령어가 도입 되었습니다.  

```
1. 브랜치간 이동  // git checkout {브랜치명} >> git switch {브랜치명}
2. 커밋 간 이동   // git checkout {커밋명} (계속해서 checkout 사용)
3. 파일 복원      // git checkout -- {파일명} >> git restore {파일명}
4. 새 브랜치 생성 // git checkout -b {새 브랜치명} >> git branch {브랜치명}
```  

git checkout -- {파일명} 명령어로  


## git reset 
깃에서 reset은 두 가지 용도,  스테이징 취소와 커밋 삭제를 할 수 있습니다.  

```
$ git reset -- {파일명}
```

## git reset HEAD^
git reset은 커밋 내역을 취소하는 명령어 입니다.  
default option 은 --mixed 입니다.
HEAD^ 는 최신 버전의 커밋을 의미하며, ~n 는 최신 버전(1)로부터 n개를 의미합니다.  
생략시 가장 최근의 커밋을 가리킵니다.  
`**HEAD의 위치를 바꿔버리며  저장소의 가장 최신 시점이 됩니다.**`

```
// 시점 선택 방법
$ git reset HEAD^      // git reset HEAD^~1 두 명령어는 기능적으로 동일 
$ git reset HEAD^~2    // 최근 커밋 2개를 취소
$ git reset            // HEAD^ 생략가능, 기본적으로 가장 최근의 커밋이 대상이 됨
$ git reset {커밋해시}  // 특정 커밋 해시 시점으로 reset할 수 있습니다.

// reset 옵션
$ git reset --soft  // 커밋 삭제, 변경 내역 유지, stage 에 올라가 있음
$ git reset --mixed // 커밋 삭제, 변경 내역 유지, stage 에 올라가 있지 않음.
$ git reset --hard  // 커밋 삭제, 변경 내역 삭제, 즉 마지막 커밋 시점 전으로 완전 초기화
```

하지만, git reset은  `사용을 멀리`해야 하는 이유가 있습니다.  

1.  커밋 히스토리가 변경되어 `혼란`을 야기할 수 있다.
2.  작업 디렉토리를 조작할 수 있으므로 `중요한 데이터를 삭제`할 수 있다.
3.  원격 저장소에 push한 경우, reset은 사용불가합니다. 로컬과 원격의 commit history가 달라져 충돌이 일어나며 commi이 불가능 합니다!!

따라서 reset으로 이전 상태로 되돌리기 보다는, 상태를 보존하고 새로운 커밋을 추가하여 업데이트 하는 쪽이 더 좋은 대안 입니다.

## git revert  
커밋 내역을 삭제하지 않고 지우고자 하는 커밋의 변경 내역을 삭제하여 revert 커밋을 새로 만듭니다.  
```
$ git revert {커밋해시}   // 설정한 커밋해시의 변경 내역을 없애며 시점 이동, 커밋 내역을 남김
$ git revert --no-commit {커밋해시}  // 설정한 커밋해시의 변경 내역을 없애며 시점 이동, 커밋 내역을 남기지 않음
```
----------


여기까지  기본적인 깃의 기능들을 다뤄봤습니다.  