---
layout: post
title: "[github] 원격/로컬 브랜치 삭제하기"
excerpt: "GitBash를 이용해 remote 브랜치와/ local 브랜치를 삭제하는 방법"
categories: [github]
comments: true
---

## Remote 브랜치 삭제하기

```
$ git checkout master //1
$ git branch -r //2
$ git branch -a //3
$ git branch -d your-branch //4
$ git push origin :your-branch //5
```

1. master 브랜치로 전환
2. `Remote` 브랜치 목록 확인
3. `Remote + local` 브랜치 목록 확인
4. 로컬에서 your-branch 삭제
5. 삭제한 상태를 Remote 저장소에 반영

<br>

## Local에 Remote 브랜치가 없는 상태라면?<br>
- 2번 수행 전에 먼저 로컬로 `브랜치`를 가져온다.

```
$ git checkout -t origin/your-branch
```

## 브랜치 삭제 이후 브랜치 목록
위와 같이 브랜치 삭제 작업이 완료된 이후에는 로컬에서 삭제된 브랜치로 전환하는 작업이 안되고 remote에서 브랜치 목록에도 찾아볼 수 없다. 그래도 `gitbash`에서 `git branch -r`로 브랜치 목록 확인시에 삭제된 브랜치가 나오는 경우가 있다. 이는 해당 브랜치 작업이 **master에 반영된 증거**이며 실제로는 삭제된 것이 맞으니 안심하자.

---

### 이 글은 아래 사이트를 참고해 작성되었습니다.

- https://blog.naver.com/dpfdkdlqmdl/221749773809
- https://wonjerry.tistory.com/6
- https://cjh5414.github.io/get-git-remote-branch/
- https://mu-storage.tistory.com/33
