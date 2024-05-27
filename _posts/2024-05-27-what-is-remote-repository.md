---
title:  "원격 저장소란?(remote repository)"
excerpt: "github remote repository를 활용하여 소스를 백업할 수 있습니다."

published: true 

categories: git&github
tags: git&github

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
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
*git-remote 원격 저장소를 생성합니다.*  

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
깃에서 기본 브랜치를 *master* 라고 하듯 깃허브 원격 저장소는 *origin*을 사용합니다.  

![](/images/2024-05-27/2024-05-27-13-26-10.png)  
*원격 저장소를 연결하는 명령어 사용 후 오류가 안 뜬다면 성공입니다.*  


## Git remote -v  

![](/images/2024-05-27/2024-05-27-13-28-17.png)  
*위와 같이 fetch와 push 주소가 떴다면 성공입니다.*  

# 원격 저장소에 올리기, 커밋


----------


여기까지 깃& 깃허브의 Remote repository를 알아보았습니다.  