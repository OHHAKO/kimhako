---
layout: post
title: "[Trouble Shooting] CORS 정책 서버측에서 해결하기"
excerpt: "Nodejs 기반 서버에서 요청/응답 이용해 CORS 정책 에러를 해결하기"
categories: [Trouble Shooting, Nodejs]
comments: true
---

# 📋 에러 레포트

### 에러 메세지

크롬 콘솔창에서 확인한 에러 메세지 입니다.

<img src='{{ "/img/error/cors.JPG" | relative_url }}' alt="cors-error">
<br>

```
Access tp XMLHttpRequest at 'localhost:3000/user/ron12' from origin http://localhost:4200'has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
```

### 에러 설명

Angular 컴포넌트에서 localhost:3000에서 실행중인 REST서버에 자원을 요청했는데 에러가 발생했다. 꽤나 자주 보이던 익숙한 에러 메세지. CORS policy에 의해 자원요청이 **blocked** 되었단다. 이 에러는 몇년 전 구글 확장 프로그램 'Allow CORS:Access-Control-Allow-Origin'을 설치해서 편리하게 해결했던 기억이 난다. 지금은 앱스토어에서 그 프로그램이 삭제된 것 같다. 그럼 어떻게 해결해야 할까? 여러가지 방법이 있지만 나는 **'Request With Credentia'**라고 하는 `인증 방식`을 선택했다. 이 인증 방식은 `클라이언트 사이드`와 `서버 사이드` 양측 모두 작업이 필요하다. 그 전에 **CORS의 정체가 무엇인지** 아래에 간단히 요약한다.

# CORS란 대체 무엇일까?

CORS는 **Cross Origin Resource Sharing**의 줄임말이다. **교차 출처 리소스 공유**라는 뜻으로 다른 출처에서 자원을 공유하는 상황을 의미한다. CORS의 상황에서는 동일 출처 정책(Same-Origin Policy)로 인해서 브라우저에서 보안 목적으로 **자원 접근을 차단**시킨다. 내 상황의 경우 포트4200 Angular에서 포트3000 REST API에 자원을 요청하여 에러가 발생했다. 요청한 출처와 요청된 출처의 **도메인이나 포트번호**가 상이한 경우 서로 다른 출처라고 인식한다. 이처럼 CORS는 자기 로컬에서 개발할때 서버에서 로컬에 있는 자원에 접근하거나 서버에 자원을 요청하는 매커니즘에서 종종 나타난다.

## 구체적으로 서로 다른 출처란 무엇일까?

<img src='{{ "/img/URI구조.png" | relative_url }}' alt="uri-structure">

브라우저는 요청한 출처와 요청된 출처의 **도메인이나 포트번호**가 상이한 경우 **서로 다른 출처**라고 인식한다. URI와 URL이 가리키는 자원의 범위는 다르니 제대로 구분하여 넘어가자. CORS가 주의하는건 `호스트와 도메인` 이다. 내 상황의 경우 동일한 local host 였지만 port가 달라서 '서로 다른 애플리케이션'의 자원이므로 CORS가 제한하는 대상이다. <br/>
그럼 보통 사이트 주소는 어떨까? <br/>
URI가 `https://www.naver.com/abc`인 경우 naver.com이 도메인이고 /abc 는 path를 가리킨다. 따라서 도메인이 naver.com/ 로 들어오는 호출은 **서로 같은 출처**로 인식되어 CORS 제한 대상이 아니다.

다음은 CORS를 해결하기 위해 구체적으로 클라이언트와 서버 앱에서 어떤 작업이 필요한지 코드로 알아보자.

## Angular (클라이언트 사이드) 작업

지금부터 하는 작업은 클라이언트와 서버가 서로 합의하에 이루어지는 자원 공유임을 알리기 위해 `토큰`을 주고받는 코드를 구현한다. 먼저 클라이언트가 서버에게 보내는 `토큰`으로서 **자격 인증(withCredentials)**을 전송(reqeust)에 포함시키자. 이미 HTTP 통신 메서드가 작성된 상태이므로 request에 **자격 인증**만 추가로 설정하면 된다. API를 호출하는 컴포넌트 혹은 서비스마다 **자격 인증** 코드를 추가할 경우 동일한 코드를 여러번 작성해야 한다. 이는 Angular의 공통 관심사 이므로 `Service`로 분리해 애플리케이션 전역에서 사용하도록 한다. `Service`를 생성하고 클라이언트의 **서버 요청을 가로채는** 동작을 수행하는 **`HttpInterceptor`**를 작성하자. **`HttpInterceptor`**는 http 모듈에서 제공한다.

<br>

HttpConfigInterceptorService 이름으로 Service 생성한다.

```
$ npm g s HttpConfigInterceptorService
```

- http 관련 모듈 임포트
- HttpInterceptor 인터페이스 구현
- intercept 메서드 작성
  - request에 `자격 인증` 추가

```typescript
import { Injectable } from "@angular/core";
import {
  HttpInterceptor,
  HttpRequest,
  HttpHandler,
  HttpEvent,
} from "@angular/common/http";
import { Observable } from "rxjs";

@Injectable({
  providedIn: "root",
})
export class HttpConfigInterceptorService implements HttpInterceptor {
  constructor() {}

  intercept(
    request: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    request = request.clone({
      withCredentials: true,
    });

    return next.handle(request);
  }
}
```

## REST (서버 사이드) 작업

이번에는 자원을 응답하는 REST 서버 사이드 작업을 해보자. 클라이언트가 인증을 포함한 요청을 보내올때 이를 확인하고, 확인 되었음을 알리기 위해 해당 정보를 **응답 객체(response)**에 포함시킨다. <br>

- 먼저 응답객체에 cors 옵션을 사용하기 위해서는 먼저 터미널을 열어 `cors 미들웨어` 설치

```
$npm install --save cors
```

<br>

다음은 서버파일 'index.js' 에 코드를 삽입해보자.

- cors 변수에 cors 미들웨어 모듈을 호출해 저장
- corsOption에는 `허용 URL`과 `자격 인증` 두 가지를 설정 (Credentials default false)
- `자격 인증`은 CORS 정책을 해결하는 핵심
- 미들웨어 함수 로드하는 `app.use()` 호출
  - 요청한 클라이언트에게 응답하는 header에 **Access-Control-Allow** config 추가
  - 설정한 corsOption 사용 등록

```javascript
//..

const cors = require("cors");
const corsOption = {
  origin: "http://localhost:4200",
  Credentials: true,
};

app.use(function (req, res, next) {
  res.header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE");
  res.header("Access-Control-Allow-Headers", "content-type");
  next();
});
app.use(cors(corsOption));

//..
```

### Response Header 의 CORS 연관 메서드

- Access-Control-Allow-Origin: 요청 도메인 URI 지정. 인증방식에서는 와일드카드 사용 불가능
- Access-Control-Expose-Headers: 브라우저에서 접근할 수 있도록 노출시키는 `response header`를 지정
- Access-Control-Allow-Methods: 서버 자원에 접근 가능한 HTTP 메소드(GET, POST, PUT, DELETE) 지정
- Access-Control-Allow-Headers: 실제 요청에서 사용하도록 HTTP Header를 가리키는 header 목록 저장
