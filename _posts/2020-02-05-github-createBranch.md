---
layout: post
title: "[github] 원격저장소 브랜치를 생성하는 방법"
excerpt: "GitBash를 이용해 원격 저장소의 브랜치를 생성할 수 있다"
categories: [github]
comments: true
---
### 작업 설명
원격저장소의 브랜치를 이용하면 팀원들과의 공동 작업물과 개인의 작업물을 효율적으로 병합시킬 수 있습니다. 팀원들이 각자의 브랜치를 이용하려면 먼저 원격 저장소의 브랜치를 생성해야 합니다. <br>
브랜치를 생성하는 방법은 두 가지 입니다.
- **깃허브 웹**을 이용하는 방법
- **GitBash**를 이용하는 방법 두가지가 있으며 


## 깃허브 웹에서 브랜치(branch) 만드는 방법
![img load fail](/img/createBranch1.JPG)  <br>
1. 원하는 저장소에 들어가기 
<br>

2. 메뉴 왼쪽 하단에 보면 셀렉박스로 branch master라고 써있는 버튼 확인
 <br>

3. 버튼 클릭 시 현재 원격저장소가 갖고있는 브랜치 목록이 뜬다. 
 <br>

4. 텍스트박스를 클릭해 브랜치이름을 입력한다. 
 <br>

5. Enter 치면 branch 생성 완료
 <br>

6. 위 5개의 메뉴에서 commits 오른쪽에 있는 branches를 클릭하면 생성된 브랜치를 확인할 수 있다.


## GitBash 이용해 브랜치(branch) 만드는 방법
1. 작업폴더에서 GitBash 오픈 (Windows경우 파란색 글씨의 master표시 확인)
2. <code><strong>$ git branch 브랜치이름 </strong></code>
    - 브랜치 생성
3. <code><strong>$ git checkout 브랜치이름 </strong></code>
    - 생성한 브랜치로 전환
    - 2번의 브랜치이름과 동일해야 한다.

4. <code><strong>$ git push origin 브랜치이름 </strong></code>
    - 원격 저장소에 반영하기
    - 2,3의 브랜치이름과 동일해야 한다.

5. <code><strong>$ git branch -r </strong><code>
    - 생성된 브랜치를 로컬에서 확인한다


* 참고로 1,2번의 작업을 동시에 진행하는 명령어는 아래와 같다. <br>
<code><strong> git checkout -b 브랜치이름 </strong></code>  

* 생성된 브랜치를 원격 저장소에서 확인하려면 위의 사진처럼 깃허브 웹 저장소에 접속하고 commit버튼의 아래에 위치한 branch:master를 누르면 생성된 브랜치 목록이 나온다. 로컬에서 git push를 하지 않았으면 안뜰 수 있으니 주의하도록 하자.

---

### 다른 깃허브 사용 방법
- [원격 저장소 브랜치 로컬로 가져오는 방법](https://ohhako.github.io/articles/2020-02/github-bringRemoteBranch)
- [GitBash 명령어 사용 방법](https://github.com/TheCopiens/algorithm-study#gitbash-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95)