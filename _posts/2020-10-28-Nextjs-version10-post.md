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

# 뭐? Nextjs 버전이 벌써 10이야?

27일에 munchen에서 `NextJS conference`가 있었다. 난 급한 약속이 생겨 라이브를 챙겨보진 못했으나 트위터에서 #nextjsconf 해시태그와 함께 Image Optimization, Image cache, CDN edge 등의 단어를 보았다. </br>
그리고 오늘 드디어 NextJS v.10 릴리즈 노트가 공식 홈페이지에 올라왔다. 사무실에서 Nextjs 프로덕트를 개발하면서 내가 사용하는 프레임워크의 최신 버전 정도는 파악해야 한다고 생각했다. 공식문서 읽는 습관을 들이는것도 중요하기 때문에 이 포스팅은 나를 위한 글이라고 생각한다.

버전 10에서 출시된 기능, 주요기능을 요약해보았다.

# **Next.js 10 주요 기능**

- **built-in 이미지 컴포넌트와 이미지 자동 최적화**: 새로운 `next/image` 컴포넌트를 이용하여 이미지를 자동으로 최적화 한다.
- **국제화 된 라우팅**: 내장된 기본요소로 Next.js 애플리케이션을 국제화 시킨다.
- **Next.js 애널리틱스**: 실제 유저 퍼포먼스를 측정하고 조치한다.
- **Next.js 커머스**: 인터넷 상업 사이트의 고 성능을 위한 올인원 스타터 키트
- **React 17 지원**: 최근 React가 17버전을 출시했는데 Next.js는 이 버전에 대해 완전히 호환된다. 프로젝트에서 React 17버전을 사용하고 있는 경우 버전 10 업데이트를 적극 고려하면 좋을 것 같다.
- **`getStaticProps`,`getServerSideProps`의 빠른 새로고침**: 파일 수정시 '빠른 새로고침'이 일어나는건 Next.js가 가진 큰 장점이다. 컴포넌트가 마운트 되기 전에 서버에서 데이터를 fetch하는게 두 메서드의 목적이다. 이제 두 메서드를 수정할때 properties를 자동으로 다시 불러온다.
- **MDX를 위한 빠른 새로고침**: `@next/mdx`를 사용할때 전체 페이지의 reload 없이도 변화를 적용하기 위해 빠른 새로고침을 활용한다. 참고로 MDX는 마크다운 독스에 JSX 문법을 사용할 수 있도록 지원하는 포맷이다.
- **Third Party React Component에서 CSS 임포트 하기**: npm에서 컴포넌트를 위해 CSS를 임포트 하는 기능이 이제 지원된다.
- **자동으로 `href`를 해결함**: `next/link`를 사용할때 `as` 프로퍼티는 더이상 필요하지 않다. href 프로퍼티에 준 경로와 동일하게 주어야 하는 불편함을 덜게 되었다.
- **`@next/codemod` CLI**: 모든 Next.js codemods에 쉽게 접근할 수 있다. 참고로 Codmod는 페북에서 개발한 거대한 코드베이스 리팩토링을 도와주는 툴이다.
- **`getStaticPaths`의 Fallback 블로킹**: 새로운 static 페이지를 생성할때 static fallback 페이지를 제공하는 대신 pre-rendering을 기다린다. fallback 페이지란 랜딩 페이지가 오프라인 상태거나 찾을 수 없는 경우 유저가 보는 페이지를 말한다.

[hako]: 사실 제가 알고있는 bg가 많이 없기 때문에 새 기능 중 가장 기대했던 항목은 `이미지 컴포넌트` , `국제화 라우팅`, `애널리틱스`, `getStaticProps, getServerSideProps의 빠른 새로고침` 이정도인 것 같습니다. 다른 분들도 소감이 있다면 댓글로 얘기해주세요:)

## **Built-in Image Component and Automatic Image Optimization**

Vercel의 Next.js에 대한 목표는 `개발자 경험`, `유저 경험` 이 두 가지에 대한 개선이다.

올해 Next.js 애플리케이션에 대한 개발자 경험과 성능 개선 이 두 가지를 면밀히 조사했다. 우리는 브라우저가 로드해야 하는 자바스크립트의 양을 줄이는 데에 초점을 두었다.

우리는 성능과 개발자 경험을 개선하는 20개 이상의 기능을 발표했다. 동시에 Next.js 코어의 자바스크립트가 16% 가량 줄었다.

1월에 우리는 구글 크롬팀과 협력하여 `best-in-class`라는 자바스크립트 code-splitting의 새로운 전략을 소개했다.

예를 들어 Barnebys는 애플리케이션 크기가 23% 줄어드는 것을 보았고 몇몇은 거대한 자바스크립트 번들의 크기가 70% 가량 줄었다. 이러한 개선은 그들의 Next.js 애플리케이션 코드에서 어떠한 변경 없이 이루어졌다.

회사들은 Next.js의 최근 버전을 간단히 업그레이드 시킴으로서 자동적으로 이러한 새로운 전략을 채택했다.

[hako]: 이 부분을 읽고 프로젝트의 version upgrade는 필요할때 도입하면 되는걸까? 반드시 필요하진 않더라도, 개선시킬 수 있는 기능이나 사용하면 좋을만한 기능이 있다면 도입을 고려하지 않을까! 하는 생각이 있었습니다. 여러분은 웹 프레임워크의 최신 버전이 나왔을때 어떤 점을 고려하여 업데이트와 시기를 결정하는지 여쭙고 싶네요! 글을 모두 읽고 댓글로 달아주십쇼 😇

## **Images on the Web**

브라우저가 로드하는 자바스크립트의 양을 줄이는데 초점을 맞춘 반면, 웹은 사실 자바스크립트 뿐만 아니라 마크업과 이미지도 갖고있다.

**이미지는 전체 웹페이지 bytes 중 50%를 차지한다.**

페이지가 로드될때 가장 크게 보이는 요소인만큼 이미지는 거대한 컨텐츠를 그리는데 큰 영향을 미친다. 거대한 contentful 페인트는 Core Web Vital이다.

모든 이미지 중 절반은 사이즈가 1MB를 넘어가는데 이는 웹상에 표시되기 위해 최적화 되지 않았다는 의미이다. 요즘 유저는들은 핸드폰, 타블렛, 노트북을 이용해 웹을 브라우징 하는데 이때 이미지는 여전히 한 사이즈로 들어맞는다. 예를 들어 사이트는 2000X2000 픽셀 이미지를 로드하지만 폰은 100X100 픽셀만 표시한다.

게다가 웹 페이지에서 첫 뷰포트의 바깥에는 30%의 이미지들이 있는데, 이는 유저가 스크롤을 내리지 않는 이상 보이지 않는 이미지들을 브라우저가 로드한다는 의미이다.

이미지는 종종 'width'나 'height' 프로퍼티를 갖고있지 않은데, 이는 페이지가 로드될때 사진이 날뛰는 현상을 불러일으킬 수 있다.

이는 Cumulative 레이아웃이 Core Web Vital을 떠나도록 상해를 입힌다. (의미 파악 필요..)

웹상 이미지의 99.7%는 WebP 처럼 최신 이미지 포맷을 사용하지 않는다.

웹 페이지 상에서 성능성 이미지를 사용하기 위해 고려되어야 하는 몇 가지 측면이 있다.

**`사이즈`, `무게`, `lazy loading` 그리고 `최신 이미지 포맷`**이다.

개발자는 이미지 최적화를 하기위해 복잡한 빌드 툴링을 set up 해야 한다. 그러나 이러한 도구들은 보통 모든 이미지 최적화가 불가능한 외부 데이터 소스 이미지(예: 유저가 제출한 이미지)를 덮어씌우지 않는다.

이런 불가능한 개발 과업은 끔찍한 엔드 유저 경험을 불가피하게 이끌어 낸다.

## **Next.js Image Component**

웹에서 이미지를 연출할 수 있는 솔루션을 발표하게 되어 기쁘다. 이번은 Next.js **이미지 컴포넌트**와 **자동 이미지 최적화**에 대한 내용이다. html의 img 태그를 Next.js 이미지 컴포넌트로 교체하는 것이 가장 기본이다.

```jsx
<img src="/profile-picture.jpg" width="400" height="500">
```

```jsx
import Image from 'next/image'

<Image src="/profile-picture.jpg" width="400" height="400">
```

`next/image` 컴포넌트를 사용할때 이미지들은 자동으로 `lazy-load` 되는데 이는 유저가 이미지를 보기 위해 가까이 갈때 이미지들이 렌더 된다는 의미이다. 이는 초기 뷰포트 바깥에 있는 30% 이미지 로딩을 방지한다.

이미지 치수가 강제로 적용되어, 브라우저가 로드될때 이미지를 점프하는 대신 이미지에 필요한 공간을 즉시 렌더링 해서 레이아웃 변경을 방지한다.

Html의 img 요소에서 width와 height는 반응형 레이아웃 이슈를 일으킬 수 있는데, `next/image`를 사용할 때는 해당되지 않는다. `next/image`를 사용하면 이미지는 제공된 width, height의 비율에 기반하여 자동으로 반응형으로 만들어진다.

개발자는 뷰포트의 이미지를 표시하여 Next.js가 자동으로 이미지들을 preload 하도록 할 수 있다. 초기 뷰포트의 Preloading 이미지는 거대한 Contentful 출력에서 50%까지 개선을 보여줄 수 있다.

## **Automatic Image Optimization**

이러한 개선이 html의 img 요소와 비교되어도, 주요 문제가 남아있다. 2000x2000 픽셀 이미지가 작은 이미지를 렌더하는 폰에 전송된다.

Next.js 10에서 선보이는 `next/image` 컴포넌트는 내장된 이미지 최적화를 통해 자동으로 작은 사이즈를 생성한다. 내장된 이미지 최적화는 브라우저가 지원하면 이미지를 JPEG보다 30%가량 작은 `WebP` 같은 최신 이미지 포맷을 제공한다. Next.js가 미래 이미지 포맷을 자동으로 채택하고 그러한 포맷을 지원하는 브라우저에게 이미지를 제공하게 된다.

이미지 최적화는 **어떠한 이미지 소스에도 동작한다.** CMS 처럼 외부 소스에서 오는 이미지도 최적화 된다.

빌드 타임에 이미지를 최적화하는 대신, Next.js 10은 유저가 요청하는 대로 on-demand에 이미지를 최적화한다. 정적 사이트 제너레이터와 static-only 솔루션과 달리 10개 이미지를 전송하든 1,000만개 이미지를 전송하든 빌드 타임은 증가하지 않는다.

## **Conclusion**

새로운 `next/image` 컴포넌트와 자동 이미지 최적화는 유저 경험을 막대하게 개선시킬 강력하고 새로운 기본 기능이다.

`next/image` 컴포넌트는 자동 lazy-loading, critical 이미지들에 대한 사전 로딩, 기기에 상관없이 정확한 크기 조절 그리고 최신 포맷을 자동으로 지원한다. 이러한 기능은 어떤 소스의 이미지에서도 동작한다.

팀은 이러한 새로운 기본 기능들로 인해 유저 경험이 얼마나 빨라지는지 지켜 볼 생각이다.

상세 설명: [Image Component and Image Optimization](https://nextjs.org/docs/basic-features/image-optimization)

## **Internationalized Routing**

올해 몇 비즈니스와 커뮤니티 회원들은 우리 팀이 국제화가 얼마나 중요한지 이해는데 도움을 주었다.

예를들어 사이트가 번역되면 소비자 중 72%는 사이트에 더 오래 머문다는 걸 알게 됐으며 55%의 소비자는 자국 언어로 된 커머셜 사이트에서만 구매를 한다고 얘기했다.

**만약 다른 나라의 시장에 진출할 예정이라면 성공을 위해 프로젝트를 국제화 시키는 것은 중요하다.**

프로젝트 국제화는 두 가지의 주축이 있다. ⇒ **번역과 라우팅**

많은 React 라이브러리는 애플리케이션이 번역되도록 준비하고 당신이 라우팅을 직접 처리하기를 기대한다. 그리고 일반적으로 렌더링 하나 전략(one-rendering strategy)에만 동작한다.

Next.js 10의 일부에서 우리가 내장되어 있는 '국제화 된 라우팅'과 '언어 감지 지원'을 출시한 한 이유가 바로 이것이다.

이 내장된 국제적 라우팅 지원은 Next.js의 하이브리드 전략을 지원하므로 페이지 단위로 정적 생성과 서버 렌더링 사이에서 하나를 선택할 수 있다.

Next.js 10은 두개의 흔한 라우팅 전략을 지원한다. : **subpath routing과 domain routing**

두 전략을 위해 Next.js 설정에 있는 locales를 설정하여 시작할 수 있다.

```jsx
// next.config.js
module.exports = {
  i18n: {
    locales: ["en", "nl"],
    defaultLocale: "en",
  },
};
```

Locales은 UTS 인데 locales를 정의하기 위해 표준화 된 포맷이다.

일반적으로 Locale 식별자는 언어, 지역 그리고 dash로 세분된 스크립트이다. (language-region-script) 지역과 스크립트는 옵션이다.

- 'en-US' - English as spoken in the United States
- 'nl-NL' - Dutch as spoken in the Netherlands
- 'nl' - Dutch, no specific region

locales가 한번 설정되면 subpath와 domain 라우팅 중 선택할 수 있다.

### **Subpath routing**

subpath 라우팅은 url에 locale를 넣는다.

이는 단일 도메인에 모든 언어가 들어갈 수 있도록 허락한다. 예를 들어 '/nl-nl/blog' 나 '/en/blog' 처럼 말이다.

### **Domain routing**

도메인 라우팅은 locale을 최상위 도메인과 매핑할 수 있도록 한다. 예를 들어 아래처럼 'example.nl'은 'nl' 지역과 매핑될 수 있고 'example.com'는 'en' 지역과 매핑될 수 있다.

```jsx
// next.config.js
module.exports = {
  i18n: {
    locales: ["en", "nl"],
    domains: [
      {
        domain: "example.com",
        defaultLocale: "en",
      },
      {
        domain: "example.nl",
        defaultLocale: "nl",
      },
    ],
  },
};
```

### **Language Detection**

Next.js 10은 모든 최신 브라우저가 지원하는 **'Accept-Language'** 헤더를 기반으로 **'/'** 라우트에 언어 감지가 내장되어 있다. 설정된 지역이 'Accept-Language' 헤더와 일치된 다음 설정된 전략에 따라서 리디렉션 된다.

### **Search Engine Optimization (SEO)**

Next.js가 유저에 의해 방문된 페이지의 언어를 알고난 이후 html 태그에 lang 속성을 자동으로 추가한다.

Next.js는 페이지 변형에 대해 모르므로 'next/head'를 이용해 'hreflang' 메타태그를 추가하는 것은 개발자에게 달렸다.

'hreflang'에 대한 상세 설명: [ Google Webmasters documentation](https://support.google.com/webmasters/answer/189077)

### **The future of internationalization in Next.js**

Internationalized Routing은 프로젝트를 쉽게 국제화 하고 지역화 할 수 있도록 만드는 시리즈 기능의 첫번째 이다. Internationalized Routing는 React 국제화 라이브러리의 주요 기능들을 통합하도록 한다.

Next.js 국제화된 라우팅 상세 설명:[Internationalized Routing](https://nextjs.org/docs/advanced-features/i18n-routing)
