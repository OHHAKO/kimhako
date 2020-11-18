---
layout: post
title: "서버의 개념"
excerpt: "포괄적인 서버의 개념을 분명하게 이해하자"
categories: [기타글]
comments: true
---

<img src="/img/server.jpg" width="500px" height="300px"/>

## 서버 개념 요약정리

### 목차

- [서버란?](#서버란?)
- [서버 하드웨어](#서버-하드웨어)
- [서버 운영체제](#서버-운영체제)
- [서버 종류](#서버-종류)
- [웹서버 종류](#웹서버-종류)

### 서버란?

서버는 클라이언트에게 정보, 서비스를 제공하는 컴퓨터 혹은 프로그램 입니다. <br>
서버의 실체는 하드웨어일수도 있고 소프트웨어일 수도 있습니다.

### 서버 하드웨어

서버 하드웨어는 서버로 사용하는 컴퓨터 물리장치를 의미합니다.

### 서버 운영체제

말 그대로 서버를 운영하는 운영체제 입니다. <br>
서버를 지향하는 운영 체제는 서버 환경에 적합하게 설계되어 있습니다. <br>
운영체제를 직접 사용하는 것 보단 주로 외부에 서비스를 제공하기 때문에 <br>
일반용 운영체제와 차이가 있습니다. <br>

**운영체제 종류**

- linux
  - 가장 많은 서버들이 사용하는 OS
  - 라이센스 비용이 없는 오픈소스
  - 필요한 부분만 골라 가볍게 OS를 구성할 수 있습니다.
- window
  - linux 다음으로 수요가 높은 MS사의 OS
  - 윈도우 닷넷 기반 프로그램과 호환이 잘되며 구동 가능성이 뛰어납니다.
  - MS사의 지원으로 인해 엔터프라이즈 서버로 많이 이용됩니다.
- solaris
  - Unix를 기반하는 서버 중 하나입니다.
  - 인수한 오라클이 현재 지원과 개발을 맡고 있습니다.

### 서버 종류

- FTP 서버: 대용량 파일 전송을 위한 **프로토콜**
- 웹 서버: 웹 서비스 제공하기 위한 서버. 대표적으로 아파치 서버가 있습니다.
- 데이터베이스 서버: 거대한 양의 데이터를 저장하고 관리하는 데이터베이스를 구동하는 서버
  - 대표적으로 MySQL서버, Oracle DB 서버, MS Sql 서버

### 웹서버 종류

<!-- <img src="/img/enterpriseLogo/apacheWebServer.jpg" width="400px" height="300px"/> -->

### 1. 아파치 웹서버 (Apache web server - the Http web server)

가장 유명하고 널리 쓰이는 웹 서버 입니다. <br>
프리 오픈소스 소프트웨어로 대부분의 운영체제에서 사용이 가능합니다. <br>
아파치 톰캣과 별개의 제품입니다. <br><br><br>

<!-- <img src="/img/enterpriseLogo/apacheTomcat.png" width="300px" height="300px"/> -->

### 2. 아파치 톰캣(Apache Tomcat)

아파치 웹서버와 다른 **웹 어플리케이션 서버(WAS)** 입니다. <br>
동적 서버 컨텐츠를 수행한다는 점에서 웹서버와 구분됩니다. <br>
servlet, jsp를 지원하도록 개발되었습니다.<br>
아파치 톰캣은 국내에서 개인, 교육용으로 많이 쓰이는 제품이라 그런지 관련 자료가 많습니다. <br>
학부시절 Java기반 웹서버를 구축할때마다 톰캣서버를 매년 사용했던 기억이 있네요.<br>
단독적으로 서버 이용이 가능하나 보통 Apache http 웹서버나 다른 웹서버와 함께 사용되고 있습니다. <br>
HTTP를 통해 애플리케이션을 동작시키는 미들웨어 라고 이해하면 됩니다.<br><br><br>

<!-- <img src="/img/enterpriseLogo/nginX.png" width="300px" height="300px"/> -->

### 3. NginX Http server

비동기 이벤트 주도 아키텍처로 좋은 성능과 효율을 자랑합니다. <br>
확장성, 헤비유저 부하량을 처리하는데 최소한의 자원을 이용합니다. <br>
트래픽이 많은 사이트에서 가장 많이 채택한 엔진이며 2020년 기준 시장 점유율이 높은 서버입니다.<br>
이용자 통계상 초당 요청 처리량이 아파치의 4배가까이 되네요. <br>
[출처](https://www.slideshare.net/sjjang61/nginx-testing-innaver-16742438) <br><br><br>

<!-- <img src="/img/enterpriseLogo/lighttpd.png" width="300px" height="300px"/> -->

### 4. 라이티(Lighttpd)

고성능의 속도 환경에 맞춰 최적화된 경량 웹 서버입니다. <br>
무슨 언어의 웹앱이든 전부 지원하며 메모리 풋프린트(memory footprint), cpu부하가 비교적 적고 <br>
여러 고급 기능을 갖추고 있습니다. <br><br><br>

<!-- <img src="/img/enterpriseLogo/iisServer.JPG" width="400px" height="200px"/> -->

### 5. IIS Windows Server (Internet Information Services)

MS사가 서비스하는 웹서버입니다. 시장 점유율이 떨어지는 추세인데요. <br>
시장점유율 통계에서 활성사이트냐 아니냐의 차이로 아파치랑 선두를 다투던 때가 있었지만 <br>
몇 년 전부터 눈에 띄게 점유율이 떨어졌습니다.<br>
