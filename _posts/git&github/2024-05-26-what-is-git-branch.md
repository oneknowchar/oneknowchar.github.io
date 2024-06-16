---
title:  "git branch란?"
excerpt: "git 브랜치(branch)를 활용하여 버전을 관리할 수 있습니다."

published: true 

categories: git&github
tags: git&github

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

# Git branch란? 
branch는 나뭇가지를 의미합니다.  
단어 의미 그대로, 원래의 소스(*master*)는 유지하면서  

![](/images/2024-05-26/2024-05-26-19-14-55.png)  

새로운 길을 내어 새로운 작업을 할때 사용합니다.  
브랜치를 통해 `버전관리`, `협업`을 할 수 있습니다.  

## test 준비  
테스트를 위한 준비를 합니다.  
```
$ mkdir git-manual
$ cd git-manual
$ vim work.txt
```
work.txt 파일을 만든 뒤  
"content1" 작성 "work 1"커밋,  
"content2" 작성 "work 2"커밋,  
"content3" 작성 "work 3"커밋 하였습니다.  

![](/images/2024-05-26/2024-05-26-19-31-39.png)  *HEAD 와  (master) 모두 가장 최근 업데이트 "work 3" 을 가리킵니다.*  

## HEAD와 master  
HEAD : 현재 작업중인 최신 버전 커밋을 의미합니다. (수정 불가!)  
master : 우리 깃 프로젝트에서 기준이 되는 메인 브랜치명 입니다. (수정 가능)
 
# Git branch  
버전 관리를 위해서 브랜치를 조회하고, 만들어 보겠습니다.
```
$ git branch      //조회
$ git branch aaa  // "aaa" 브랜치 생성
$ git branch bbb  // "bbb" 브랜치 생성
$ git branch ccc  // "ccc" 브랜치 생성

$ git branch
```  
![](/images/2024-05-26/2024-05-26-20-04-22.png)  
*aaa, bbb, ccc 브랜치를 생성고 master 브랜치를 조회중입니다.*  

## Git log --oneline  
master 브랜치의 work.txt 파일에 임의의 내용을 추가하고 커밋하였습니다.

![](/images/2024-05-26/2024-05-26-20-26-34.png)  
*git log `--oneline` 옵션으로 커밋 내역을 한 줄로 요약할 수 있습니다.* 


(master) 브랜치의 "master 4 update" 버전을 HEAD(조회)하고 있습니다.

## Git checkout  
```
$ git checkout {브랜치명}   //브랜치 간 이동 명령어
```  

ccc 브랜치로 체크아웃 하여 로그 조회를 하였더니  
master 브랜치와 별개로 work 3 버전이 최신 버전임을 확인할 수 있었습니다. 

![](/images/2024-05-26/2024-05-26-20-31-22.png)  
*브랜치간에 업데이트는 별개로 관리됨을 알 수 있었습니다.*


## Git log --oneline --branches   
`--branches` 옵션으로 모든 브랜치간에 커밋내역도 함께 확인할 수 있습니다. 

![](/images/2024-05-26/2024-05-26-20-46-51.png)  
*테스트를 위해 aaa 브랜치 커밋 내역 추가를 한 뒤*  

![](/images/2024-05-26/2024-05-26-20-48-40.png)  
*--branches 옵션으로 모든 커밋내역 확인을 했습니다.*


## Git log --graph  
점점 옵션이 추가되며 길어지고 있지만...  
로그 내역을 시각화 할 수 있는 명령어가 있습니다.

```
$ git log --oneline --branches --graph
```
![](/images/2024-05-26/2024-05-26-20-54-45.png)  
*`--graph` 옵션으로 로그 내역을 그래프화 할 수 있습니다.*  

## Git log master..aaa  
브랜치간 비교하는 명령어 입니다.
```
$ git log aaa..master   // 왼쪽을 기준으로 오른쪽 브랜치와 비교, 오른쪽 브랜치 내용을 보여줍니다.
```  

## Git merge {branch}  
두 브랜치를 하나로 합치는 명령어 입니다.  
우선 합병의 기준이 되는 브랜치로 체크아웃을 해야 합니다.

```
$ git checkout         // 현재 브랜치는 aaa 입니다.
$ vim aaa.txt          // merge 테스트를 위해 더미 파일을 생성합니다.

$ git checkout master  // master 브랜치로 이동...

$ git merge aaa        // master 브랜치에 aaa 브랜치를 합칩니다.  

하지만...
```  

![](/images/2024-05-26/2024-05-26-21-17-42.png)  
*Merge conflit in work.txt 머지 실패!!*  

aaa와 master 브랜치의 work.txt 파일에서 같은 라인 수정이 일어났기 때문에 머지를 실패 했습니다.  
work.txt 파일을 확인해 보겠습니다!  


```
$ vim work.txt
```

![](/images/2024-05-26/2024-05-26-21-19-46.png)  
*의미 심장한 못 보던 기호들이 있습니다...*  
중심 선(=) 기준으로 HEAD(현재 브랜치)와 aaa브랜치 사이에 라인 다른 내용을 보여 줍니다.  

중심 선을 기준으로,  
유지하고싶은 하나의 소스를 제외한 다른 소스와 <<<HEAD,  >>> aaa 주석을 지워 충돌을 해결한 뒤,  
`커밋` 합니다.  

![](/images/2024-05-26/2024-05-26-21-47-02.png)  
*aaa 브랜치를 master 에 합병했습니다.*  

 
![](/images/2024-05-26/2024-05-26-21-50-18.png)  

*work.txt 파일에 제가 선택한 "apple content 4"가 남겨져 있습니다!*  
*그리고...*  
*aaa.txt 파일도 master 브랜치에 잘 합쳐진것을 확인할 수 있었습니다.*   

 

![](/images/2024-05-26/2024-05-26-22-23-34.png)  
*그래프를 통해 두 브랜치의 머지를 확인할 수 있었습니다.*

## Git branch -D
옵션에서 -d 또는 -D 의 차이점은...  
삭제 하려는 브랜치가 체크아웃된 브랜치가 아니고 병합된 브랜치인지  
또는 조건없이 강제 삭제인지의 차이입니다.  

![](/images/2024-05-26/2024-05-26-22-39-35.png)  
*aaa 브랜치를 머지한 뒤 제거를 하였습니다.*  

놀라운 사실중 하나는, 삭제한 같은 브랜치의 이름으로 다시 만들면 작업했던 내용이 그대로 나타납니다.  
이건 깃 자체에서 완전히 삭제하는게 아닌 깃의 흐름속에서 감추는것이라고 합니다...  
이해하고 넘어갑니다.

----------


여기까지 깃의 branch를 알아보았습니다.  