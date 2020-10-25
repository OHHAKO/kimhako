---
layout: post
title: "[React] React 처음 시작하기 - 1. 공식문서를 읽어보자"
excerpt: "나처럼 React를 다짜고짜 개발부터 하는 사람들을 위해 포스팅을 남긴다."
categories: [React]
comments: true
---

## Ajax, Angular 경험 회고

지난 두개의 프로젝트를 진행하는 동안에 웹 포지션을 담당한 나는
**"어떤 방식으로 뷰를 출력하나"**가 큰 관심사 였다. <br>

첫번째 프로젝트 떄 기술스택은 jsp, html, javascript 였는데 <br>
실시간으로 가져온 데이터를 뷰에 출력할때 하나의 페이지 전체 로딩없이 `필요한 부분`만 동적 출력하는데 'Ajax'를 사용했다.

두번째 프로젝트는 hyperledger 모듈을 이용해야 했기 때문에 docker와 호환성이 뛰어난 <br>
컴포넌트 중심의 프론트엔드 프레임워크 'Angular'를 다루게 되었다. <br>
뷰의 변화단위가 '컴포넌트'인 앵귤러는 html/typescript/css(옵션) 를 하나의 컴포넌트 구성요소로 한다. 그래서 필연적으로 리소스의 수가 많고 구조가 복잡해지는데 이 컴포넌트 간의 이동 설정을 한번에 할 수 있는 기능이 있다. 바로 routing 이다. 컴포넌트 간의 관계가 parent인지 child인지 등록하고 어떤 호출이 올때 어떤 컴포넌트를 호출 할 것인지 등록할 수 있다. 앵귤러가 갖는 `최고의 매력`이 아닐까 싶다. <br>

**그렇다면 react는 어떤 방식으로 렌더할까?** <br>

## React의 특징

일단 React을 이해하고 간단한 코드 테스팅을 위해 React 공식 홈페이지를 방문했다.

- React official document : [https://ko.reactjs.org/](https://ko.reactjs.org/)

React의 동작 방식을 간단하게 요약하자면 **view, state, render** 이 세 가지가 주요 핵심이다. 컴포넌트의 state가 변화하면 render되어 view를 그린다. 정도로 플로우를 이해하자. view는 html 태그로 DOM을 구성하는 **뼈대**이고 state는 그 뼈대에 넣을 **데이터**이다. 그 **뼈대에 데이터를 넣는 작업**을 render가 해주는 것으로 쉽게 예측할 수 있다. <br>

홈페이지에서 소개하는 React를 개발할때 주목할만한 특징을 캐치해 보았다.

#### 선언형

상태에 대한 뷰만 선언하면 상태 데이터 변경에 따라 적절한 컴포넌트 갱신. <br>
상호작용 많은 UI에 적절하다.

#### 컴포넌트 기반

js로 로직을 작성하면 컴포넌트가 스스로 상태를 관리한다.

#### 컴포넌트 자체관리

DOM과 별개로 컴포넌트의 상태를 관리할 수 있다. 그 말인즉슨, button같은 요소에 뷰에 변화를 주는 이벤트를 작성할 때 getElementById처럼 DOM 객체에 직접 접근하지 않고도 페이지 변화를 줄 수 있다는 말이다. 개발자는 컴포넌트 작성에 온전히 신경 쓸 수 있다.

이제 React에서 컴포넌트가 어떤 역할을 하는지 대략적으로 이해했으니
코드를 통해 컴포넌트와 동작을 살펴본다.

#### 컴포넌트 동작 예시코드

예시 코드는 XML과 유사한 문법인 `JSX`로 작성되었다. 여기서 확인해야 하는 것은 역시 **view, state, render** 이다. <br>

- 이 **`HelloMessage`** 컴포넌트는 **`state`**로 message를 갖고있다.
- this.setState 메서드에서 **`state`**가 변경되도록 했다. 이제 message가 "hello" 값을 갖게 되었다.
- **`state`**가 변경되었으니 **`render`** 메서드가 호출되어 컴포넌트를 출력한다.

```jsx
class HelloMessage extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: "" };
  }

  componentDidMount() {
    this.setState((satate) => ({
      message: "hello",
    }));
  }

  render() {
    return <div>message: {this.props.name}</div>;
  }
}
```

React를 본격적으로 다뤄보기 전에 동작을 간단하게 이해하여 보았다. <br>
다음 포스트에서는 컴포넌트를 이루는 요소들에 대해 정리해보기로 한다.
