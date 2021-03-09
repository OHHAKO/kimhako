---
layout: post
title: "[Trouble Shooting] eclipse error : java was started but returned exit code=13 이클립스 열때 에러"
excerpt: "java 업데이트와 관련되어 Eclipse 개발도구가 열리지 않는 문제를 해결합니다."
categories: [Trouble Shooting]
comments: true
---

## 이클립스 실행시 Error 메세지 발생

어느날 이클립스를 실행시켰더니 프로그램은 동작하지 않고 <br>
에러 메세지만 홀연히 남겼습니다..

<img src='{{ "/img/eclipse_error_13.png" | relative_url }}' alt="eclipse-error-message" width="490px" height="550px">

가장 위에 보이는 이 기록이 에러 이름인듯 합니다.

```
eclipse error : java was started but returned exit code=13
```

## 이클립스 상세한 에러메세지 확인

에러를 더 자세하게 확인하기 위해 로그 파일을 열어봅시다.

- `Eclipse` 폴더 > `configuration` 폴더 > `.log` 더블클릭

로그파일을 열면 아래와 같은 메세지가 적혀있습니다.

```
java.lang.UnsatisfiedLinkError: Cannot load 64-bit SWT libraries on 32-bit JVM
```

이는 이클립스 개발도구와 자동으로 업데이트 된 자바의 비트수가 안맞아서 발생하는 에러 메세지 입니다. <br>
노트북에 깔려있던 java는 64bit 였는데 버전이 자동 업데이트 될때 32bit로 바뀐 것 같습니다.

## 해결과정

Oracle 사이트에 들어가서 자바를 삭제하고 다시 깔겠습니다. <br>
그렇다면 환경변수도 새로 설정을 해주어야겠죠?

1. 제어판 > 프로그램 삭제 > 프로그램 목록에서 java, java update plattform 삭제
2. [자바 공식사이트](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)에 들어가서 windows x64용 java 다운받기. 버전과 관련있는 노란색 표시를 잘 확인해주세요.
   <img src='{{ "/img/javawindow64.png" | relative_url }}' width="400px" height="400px">

3. 다운받은 설치파일 실행시켜서 자바 인스톨 시작. 위치는 default로 설정

4. `시스템` > `고급 시스템 설정` or `시스템 속성` > `환경변수` 에서 - 아랫단(시스템 변수(s))에 있던 기존의 JAVA_HOME 변수 값을 본인이 다운받은 JDK 경로로 변경
   (예를들어 저같은 경우 C:\Program Files\Java\jdk1.8.0_201 )

5. 기존의 Path 변수에 `%JAVA_HOME%\bin` 추가
   <img src='{{ "/img/javahome.png" | relative_url }}' width="400px" height="500px">

6. cmd 창을 열어 `javac -version` 명령어를 입력하면 나오는 새 버전 확인

이제 Eclipse를 열면 에러창 없이 깔끔하게 실행 된답니다.
