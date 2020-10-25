---
layout: post
title: "[Angular] 앵귤러 이해하기"
excerpt: "프론트 프레임워크인 앵귤러에 대해 간략히 알아 보겠습니다."
categories: [angular]
comments: true
tags: [angular, typescript]
---

## Angular는 프레임워크 이다

Anuglar는 DOM을 조작하기 쉽게 도와주는 **프론트 프레임워크(front-end framework)** 입니다. <br>
언어는 타입스크립트(Typescript), 자바스크립트(Javascript)를 기반으로 하며 <br>
컴포넌트 단위로 뷰를 구성하는 특징이 있습니다. <br>
<br>

## Single Page Application (SPA) 이다.

인터넷 기술이 발달하는 동안에 서버 단에서 html을 만들어 클라이언트에게 전달하는 것 보다 **(SSR)** <br>
클라이언트 단에서 html을 만들어 내는 것이 유리해졌습니다. **(CSR)** <br>
전체 html을 서버단에서 생성 할 필요 없이 필요한 부분만 선택해 데이터, 뷰를 그릴 수 있게 되었는데 <br>
이러한 방식을 SPA(Single Page Application ) 라고 부릅니다. <br>
앵귤러는 SPA를 채택한 대표적인 프레임워크 입니다.

> Single Page Application (SPA) <br>
> 뼈대만 남은 html + 유저 동작 javascript + 분리된 환경의 API
> <br><br>
> Server Side Rendering (SSR) <br>
> 서버 단에서 html를 렌더링
> <br><br>
> Client Side Rendering (CSR) <br>
> 클라이언트 단에서 html를 렌더링 (부분 렌더링 가능)
> <br><br>
> Search Engine Optimazation (SEO) <br>
> 검색 엔진 최적화

## Angular 기반 웹의 단점

Angular로 만들어진 웹은 검색엔진이 유용한 데이터를 검색하지 못하는 단점이 있습니다. (**SEO**) <br>
이유인 즉슨 SPA의 특성상 브라우저가 Html을 모두 그리고 나서야 모든 컨텐츠가 생성되기 때문입니다. <br>

이 문제를 해결하기 위해 컨텐츠를 서버에서 만들어 주는 **Anuglar Universal**이 나왔습니다. <br>
기존 Anuglar는 동적 서버가 필요없지만 Angular Univ 을 이용하면 Nodejs 서버를 통해 <br>
Angular 코드가 실행되고 서버 단에서 페이지를 만들어 클라이언트에게 제공합니다. <br>
