---
layout: post
title: "[github] Fork한 저장소를 최신으로 동기화 시키는 방법"
excerpt: "GitBash를 이용해 fork 해온 저장소를 최신으로 동기화 시킬 수 있다"
categories: [github]
comments: true
---

<!-- ![img load fail](/img/fork_repository_update.png) -->
<img src='{{ "/img/fork_repository_update.png" | relative_url }}' alt="fork-repo-local-update" width="75%" height="40%">

### 작업 설명
Original 저장소로부터 최신 정보를 가져와 fork해온 저장소를 **업데이트 시키는** 작업이다.<br>
동기화 작업이 완료되면 로컬 저장소, 원격 저장소가 모두 Original 저장소의 **최신 상태**에 동기화된다.

#### 요약 설명
1. <code> $git remote add upstream 원본저장소URL </code>
    - 로컬에 original 원격 저장소를 upstream으로 추가해 original 원격 저장소와 연결
2. <code> $git remote -v </code>
    - 원격 저장소 목록 확인
3. <code> $git fetch upstream </code>
    - original 저장소로부터 최신 상태정보 가져오기
4. <code> $git checkout master </code>
    - master 브랜치로 전환
5. <code> $git merge upstream/master </code>
    - 가져온 최신 정보가 upstrea/master 브랜치에 있으므로 master브랜치와 통합해 적용
6. <code> $git push origin master </code>
    - 원격 fork 저장소에 반영
