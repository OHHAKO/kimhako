---
layout: post
title: "[NextJS] Next.js 10 RELEASE NOTE"
excerpt: "Next.js 10 버전으로 업데이트 되며 새로 생긴 기능을 요약정리 했다."
categories: [NextJS]
comments: true
tags: [NextJS]
credit: NextJS
creditlink: https://github.com/OHHAKO
---

## 뭐? Nextjs 버전이 벌써 10이야?

27일에 munchen에서 `NextJS conference`가 있었다. 난 급한 약속이 생겨 라이브를 챙겨보진 못했으나 트위터에서 #nextjsconf 해시태그와 함께 Image Optimization, Image cache, CDN edge 등의 단어를 보았다. </br>
그리고 오늘 드디어 NextJS v.10 릴리즈 노트가 공식 홈페이지에 올라왔다. 사무실에서 Nextjs 프로덕트를 개발하면서 내가 사용하는 프레임워크의 최신 버전 정도는 파악해야 한다고 생각했다. 공식문서 읽는 습관을 들이는것도 중요하기 때문에 이 포스팅은 나를 위한 글이라고 생각하고 있다.

버전 10에서 출시된 기능, 주요기능을 요약해보았다.

## Next.js 10 주요 기능

- built-in 이미지 컴포넌트와 이미지 자동 최적화: 새로운 `next/image` 컴포넌트를 이용하여 이미지를 자동으로 최적화 한다.

- 국제화 된 라우팅: 내장된 기본요소로 Next.js 애플리케이션을 국제화 시킨다.

- Next.js 애널리틱스: 실제 유저 퍼포먼스를 측정하고 조치한다.

- Next.js 커머스: 인터넷 상업 사이트의 고성능을 위한 올인원 스타터 키트

- React 17 지원: 최근 React가 17버전을 출시했는데 Next.js는 이 버전에 대해 완전히 호환된다. 프로젝트에서 React 17버전을 사용하고 있는 경우 버전 10 업데이트를 적극 고려하면 좋을 것 같다.

- `getStaticProps`,`getServerSideProps`의 빠른 새로고침: 파일 수정시 '빠른 새로고침'이 일어나는건 Next.js가 가진 큰 장점이다. 컴포넌트가 마운트 되기 전에 서버에서 데이터를 fetch하는게 두 메서드의 목적이다. 이제 두 메서드를 수정할때 properties를 자동으로 다시 불러오게 된다.

- MDX를 위한 빠른 새로고침: `@next/mdx`를 사용할때 전체 페이지의 reload 없이도 변화를 적용하기 위해 빠른 새로고침을 활용한다. 참고로 MDX는 마크다운 독스에 JSX 문법을 사용할 수 있도록 지원하는 포맷이다.

- Third Party React Component에서 CSS 임포트 하기: npm에서 컴포넌트를 위해 CSS를 임포트 하는 기능이 이제 지원된다.

- 자동으로 `href`를 해결함: `next/link`를 사용할때 `as` 프로퍼티는 더이상 필요하지 않다. href 프로퍼티에 준 경로와 동일하게 주어야 하는 불편함을 덜게 되었다.

- `@next/codemod` CLI: 모든 Next.js codemods에 쉽게 접근할 수 있다. 참고로 Codmod는 페북에서 개발한 거대한 코드베이스 리팩토링을 도와주는 툴이다.

- `getStaticPaths`의 Fallback 블로킹: 새로운 static 페이지를 생성할때 static fallback 페이지를 제공하는 대신 pre-rendering을 기다린다. fallback 페이지란 랜딩 페이지가 오프라인 상태거나 찾을 수 없는 경우 유저가 보는 페이지를 말한다.
