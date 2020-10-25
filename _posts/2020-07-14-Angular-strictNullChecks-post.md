---
layout: post
title: "[TypeScript] strictNullChecks과 Type Guards를 이용한 안전한 코드"
excerpt: "JavaScript가 갖는 동적타입을 포기한 TypeScript는 어떤 장점이 있을까. TS 코드를 짜다가 발생한 strickNullChecks로부터 그 장점을 공부하게 되었다."
categories: [Typescript]
comments: true
tags: [typescript]
credit: typescript
creditlink: https://github.com/OHHAKO

---



# 경험회고

최근 Angular 프로젝트에서 TypeScript를 작성하는데 배열 객체에 chaining으로 접근하니 빨간 줄이 표시되었다. `strictNullChecks` 라는 키워드를 확인했는데 안전이 보장되지 않는 데이터에 접근해서 일어난 에러임을 알 수 있었다. 요소가 null 혹은 undefined일 경우 예외 처리를 했지만 아예 쓰지 못하는 상황이 발생하니 당황스러웠다. TypeScript는 나에게 어떤 메세지를 주고 싶었던 걸까. 컴파일러에게 의도적 사용을 알릴 순  없을까? 반드시 안전한 코드를 작성해야 할까? 

<br/>

> 이 글은 Ben에게 허락을 구하고 번역한 내용입니다. <br/> [Ben's post about safer code with typescript strictNullChecks & type guards](https://shinesolutions.com/2017/01/06/writing-safer-code-with-typescript-strict-null-checks-type-guards/)



<br/>

`strictNullChecks`라고 부르는 특징을 활용해 안전한 TypeScript를 짜는 방법을 소개한다. 기본적으로 **비활성 된 기능**이라 간과하기 쉬우나 더 튼튼한 코드를 생산하는데 도움이 된다. TypeScript의 언어적 특징인 `type guards` 에 대해서도 얘기할 것이다.

 몇몇 자바 풀스택 개발자들은 정적으로 타입된 자바스크립트를 원했다. 지난번 GWT로 새 프로젝트를 시작했을 때  자바를 양 사이드에서 사용할 수 있는 가능성에 대해서 꽤 놀랐다. 많은 언어들이 js를 대체하기 위해 노력하는 경향이 있는 것 같다. TypeScript가 그들 중 하나인데 나같은 경우 Angular2 로 처음 경험했다. JS에서 정적 타입을 추가하는 개념이 꽤 좋았다. 그러나 개발자들은 늘 JS로부터 자유롭고 싶어하는 것 같다. **정적 타입**과 **동적 타입**을 어떻게 적절하게 섞을지 고민한다면 TypeScript가 그런 개발자에게 도움을 줄 수 있을지도 모른다.

 (Ben은 두 **tradeOffs**를 경험한 바로는 전 새로운 `React/Redux` 프로젝트에 TypeScript를 추가하기로 결정했다고 한다. 글쓴이는 정적 데이터를 출력하는 `React/Gastby` 프로젝트에 참여했을때 TypeScript를 적용시킨 코드를 봤다)

# 문제 개요

보통 JS에서 개발자가 고통받는 포인트는 null과 undefined 값이다. 빈번하게 논의되는 주제이므로 자세한 설명 생략한다. null 혹은 undefined로 정의된 객체에 접근해본 개발자라면 누구나 이 메세지를 본 적 있을 것이다.

```
Uncaught ReferenceError: foo is not defined
Uncaught TypeError: window.foo is not a function
```

이 에러는 코드가 이미 배포되어 런타임에서 발생하는 에러다. 개발자는 `코드 에러 예방`을 실행할 때가 아니라 **코드를 작성할 때** 하고싶다. 그게 바로 `strictNullChecks`가 존재하는 이유이다.

# Strict null checks

`strictNullChecks`는 개발자가 null 혹은 undefined 값을 참조하는 것을 방지한다. 컴파일러 명령에서 **플래그 옵션**을 추가하거나 **tsconfig.json** 파일에 추가하면 `strictNullChecks`를 사용할 수 있다. TS 개발자들은 실제로 이 옵션을 활성시켜 사용하는 것을 권장한다. 아래는 기술서 발췌 내용이다.

```
참고: 가능하면 -strickNullChecks를 사용하는 것을 권장드립니다. 
그러나 기술서의 목적을 위해 우리는 해당 기능을 꺼둔 것으로 가정하겠습니다.
```

프로젝트를 처음 시작하면서 `-strickNullChecks`을 사용하는 건 지혜로운 생각이다. 그런데 이 기능을 켜놓으면 수백개의 컴파일 에러를 맛볼 수 있다. 개인적으로 모든 에러 결과를 수정하는데 3시간이 걸렸기 때문이다.



### strickNullChecks 플래그를 켜두었을때 컴파일 에러가 발생하는 예제

```typescript
function mapStateToProps(state: State, ownProps?: TApiKeyProps): TApiKeyProps {
  return {
    apiKeys: state.apiKey.data.apiKeys,
    //ERROR: TS2532: Object is possibly 'undefined'.
    onAddApiKey: ownProps.onAddApiKey,
    //ERROR: TS2532: Object is possibly 'undefined'.
    onDeleteApiKey: ownProps.onDeleteApiKey,
  } as TApiKeyProps;
}
```

컴파일러가 에러를 던지는데 그 이유는 옵션인자 'ownProps'가 정의되지 않았을 가능성이 있기 때문이다. 그래서 이 타이밍에 컴파일러는 개발자에게 '야! undefined를 조심해!' 라는 신호를 보낸다. 이 문제를 해결하려면 우리는 TypeScript 2.1에서 구현되는 `객체 스프레드(object spread)` (곧 출시될 EXMAScript 기능)을 사용할 수 있다. 참고로TypeScript의 스프레드 연산자는 배열이나 객체의 값을 다른 객체,배열을 초기화 시킬때 사용된다. 연산자는  `...` 로 표현한다.

```typescript
function mapStateToProps(state: State, ownProps?: TApiKeyProps): TApiKeyProps {
  return {
    //Copies all fields from ownProps and overwrite with fields defined below (apiKeys)
    ...ownProps,
    apiKeys: state.apiKey.data.apiKeys,
  } as TApiKeyProps;
}
```

스프레드 연산자를 사용한 위 코드에서 만약 'ownProps' 변수가 `undefined` 되면 컴파일러는 이를 가볍게 무시한다. undefined 될 수 있는 객체에 접근하는 것으로부터 예방될 수 있는 장점이다. 대체적으로 우린 `객체 스프레딩` 구문 없이도 'ownProps'가 정의됨을 명시적으로 확인한다.
<br/>
다음 예제는 얼마나 strictNullChecks가 코드를 안전하게 만들어 주는지 뿐만 아니라 표준문안(boilerplate)을 줄일 수 있는 유용함을 보여준다.

**boilerplate code란?**  반복되지만 자주 쓰이는 형태를 자동화 하는 행위

- 최소한의 변경으로 재사용할 수 있는 것
- 적은 수정만으로 여러 곳에 활용 가능한 코드, 문구
- 각종 문서에서 반복적으로 인용되는 문서의 한 부분

```typescript
export function apiKey(state: ApiKeyState, action: BaseAction): ApiKeyState {
  let newState: ApiKeyState;

  switch (action.type) {
    case "ADD_API_KEY":
      newState = Object.assign({}, state);
      newState.data.apiKeys = [
        ...state.data.apiKeys,
        action.payload.Key,
      ] as string[];
      break;
    //More actions handling here
    //...
  }

  //ERROR: TS2454:Variable 'newState' is used before being assigned.
  if (newState) {
    return newState;
  } else {
    return state ? state : API_KEY_INITIAL_STATE;
  }
}
```

위 코드에서 컴파일러는 if문까지 내려가서 'newState' 변수 사용을 확인한다. 그러나 그 변수는 아마 할당될 일이 없을 것이다. 이 에러를 수정하기 위해서는 아래 코드처럼 넘어온 인자에서 'newState'를 'state'에 할당하고 if조건문을 완전히 지워야 한다. 

```typescript
export function apiKey(state: ApiKeyState, action: BaseAction): ApiKeyState {
  //Assigning variable here.
  let newState: ApiKeyState = state;

  switch (action.type) {
    case "ADD_API_KEY":
      newState = Object.assign({}, state);
      newState.data.apiKeys = [
        ...state.data.apiKeys,
        action.payload.Key,
      ] as string[];
      break;
    //More actions handling here
    //...
  }

  //Using only newState in return.
  return newState ? newState : API_KEY_INITIAL_STATE;
}
```

<br/>

### null 혹은 undefined를 사용해야 할 때

아래처럼 개발자가 가끔 `null` 혹은 `undefined` 를 사용하는 상황이 종종 있다. 

```typescript
private usedMb(): number {
    if (this.props.usedBytes != null) {
        return (this.props.usedBytes / (1024 * 1024));
    }

    //ERROR: TS2322:Type undefined is not assignable to type number.
    return undefined;
}
```

이런 경우 `undefined`를 반환하는 건 함수 입장에서 타당한 행동이다. **그러나 컴파일러는 에러를 발생시킨다.** 이를 피하려면 우리는 **함수 선언부**에 명시적으로 `undefined`를 추가해야 한다. 컴파일러에게 이 행위가 의도적이며 타당한 행위라는 것을 알리기 위함이다. (컴파일러가 오해하지 않게 일러줘야 함)

```typescript
private usedMb(): number | undefined {
    if (this.props.usedBytes != null) {
        return (this.props.usedBytes / (1024 * 1024));
    }

    return undefined;
}
```

이제 결과 확인 없이 함수를 사용하려 한다면, 우리는 추가적인 에러를 갖게 된다.

```typescript
private formatUsedMb(): string {
    //ERROR: TS2531: Object is possibly undefined
    return this.usedMb().toFixed(0).toString();
}
```

이러한 경우 우리는 반환값이 정의되어 있는지 확인해야 한다.

```typescript
private formatUsed(): string {
    let usedMb = this.usedMb();

    return usedMb ? usedMb.toFixed(0).toString() : '';
}
```



추가: `null` 대신 `undefined`를 사용하는게 더 편하다는것을 알아냈다. 왜냐하면 Typescript는 **optional interface의 필드**와 **함수 인자**에서 `undefined`를 사용하기 때문이다.

# Type guards

타입가드는 TypeScript 기능 중 하나로, 개발자가 type을 확인하도록 허락하여 TypeScript가 자동적으로 해결하도록 유도한다. 바이트를 메가바이트로 변환하는 간단한 예제를 통해 알아보자.

```typescript
private usedMb(): number | undefined {
    if (this.props.usedBytes) {
        return (this.props.usedBytes / (1024 * 1024));
    }

    return undefined;
}
```

만약 'this.props.usedBytes' 변수가 정의되어있지 않으면 usedMb()는 `undefined`를 반환할 것이다. 그러나 'this.props.usedBytes'에 값이 0으로 할당되면 실제 원하는 값이 0일 때 반환 값으로 정의되지 않기 때문에 현재 구현은 기대만큼 작동하지 않을 것이다.
<br/>

타입 가드의 장점은 property가 숫자인지 문자열인지 개발자가 감당해야 할 타입 확인을 컴파일러에게 맡길 수 있다는 것이다. 운이 좋게도 TypeScript는 그걸 하기에 아주 좋은 기능을 제공한다. 그것을 **type gurad** 라고 부른다. 이 경우를 간단한 함수로 보인다.

```typescript
export function isNumber(n: any): n is number {
  return typeof n === "number";
}
```

`is` 형태와 같은 시그니처를 반환함으로서 개발자는 컴파일러에게 타입 가드가 된 함수를 고려하도록 한다. 그럼 우리는 타입 가드를 사용하여 기존의 usedMb()를 다시 작성할 수 있다.

```typescript
private usedMb(): number | undefined {
    //Using type guard isNumber()
    return isNumber(this.props.usedBytes) ?
        (this.props.usedBytes / (1024 * 1024)) :
        undefined;
}
```

타입 가드 함수를 위해 `any`를 타입 인자로 사용하는 것은 실제 흔하지 않다. 타입가드는 개발자가 넘겨주는 타입의 범위를 한정할 수 있을때 더 파워풀하다. 쓰이는 동안에 컴파일러는 이러한 타입들을 구분할 수 있게 된다. 아래에서 예제를 확인한다.

```typescript
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}

...
// Both calls to 'swim' and 'fly' are now okay.
if (isFish(pet)) {
    pet.swim();
}
else {
    pet.fly();
}
```

보다시피 컴파일러는 pet이 Bird 타입이라는 것을 알고있음을 확인할 수 있다.

# 결론

TypeScript는 개발자 코드를 안전하게 만들어주는 편리한 메커니즘을 제공한다.

특히 `–strictNullChecks` 옵션은 매우 유용하다. 그것은 어떠한 잡음도 만들지 않고 개발자가 작성한 코드의 실질적인 문제들을 조명시킨다. 또한 그것을 사용하는 건 개발자의 코드를 안전하게 만들어주는 것 뿐만 아니라 작은 규모로, 똑똑하게 만드는 경향이 있다.

이러한 기능을 사용하는 것은 모든 가능한 시나리오를 커버함으로써 개발자가 유닛 테스트를 사용하는 부담을 덜어준다. 유닛 테스트를 대체한다는 말을 의미하진 않는다. 다만 대형 리팩터링 같은 특별한 작업을 수행할때 보완적이라고 할 수 있을 것 같다. 다음은 TDD에 대한 공부를 해보아도 좋을 것 같다.

