---
layout: post
title: "[Angular] 입력 프로퍼티와 ngOnChanges"
excerpt: "컴포넌트에 입력 프로퍼티가 존재할때 ngOnChanges는 언제 동작할까?"
categories: [angular]
comments: true
tags: [angular, typescript]
---

타입스크립트로 컴포넌트를 구현하다 보면 부모 컴포넌트와 자식 컴포넌트 간에 **데이터를 주고받는 경우**가 있다.
양쪽 컴포넌트에서 이 데이터의 변화를 감지해 어떤 동작을 수행하도록 지시할 수 있다. <br/>
**생명주기(LifeCycle)**를 이용해서 말이다.

# 생명주기와 ngOnChanges 메서드

Angular에서 컴포넌트와 디렉티브는 **생명주기**를 갖는다. <br/>
생명주기란 둘을 생성하고 소멸되기 까지 어떤 주기들이 존재함을 의미한다. 추상메서드인 **'생명주기 훅 메서드'**를 이용하면 특정 주기에 동작하는 메서드를 구현할 수 있다. <br/>
`ngOnChanges` 메서드는 **프로퍼티 변화**가 감지되면 호출된다.

# ngOnChanges 메서드 호출

부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 예시로 알아보자.

- 템플릿에서 자식 컴포넌트 호출
- 자식 컴포넌트에게 'prop' 라는 프로퍼티 이름으로 문자열 전달

```typescript
//부모 컴포넌트
import { Component } from "@angular/core";

@Component({
  selector: "app-rank",
  template: "
  <div>
    <app-child [prop]='test'>
    </app-child>
  </div>
  ",
})
export class ParentComponent {
  test = "hi";
  constructor() {}
}

```

<br/> <br/>

자식 컴포넌트에서 prop 데이터를 받아 화면에 출력한다. <br/>
이때 컴포넌트에 입력 프로퍼티가 존재하기 때문에 `ngOnChanges()`가 호출된다. 우리가 입력 프로퍼티에 변화를 주진 않았지만 **부모 컴포넌트**로부터 데이터를 바인딩 받았기 때문에 변화가 감지되었다.

- `OnChanges`: `ngOnChanges` 메서드 작성하기 위한 인터페이스
- `@Input()`: 부모 데이터를 입력 프로퍼티에 바인딩
- `ngOnChanges()`: 입력 프로퍼티 감지로

```typescript
import { Component, OnChanges, Input, SimpleChanges } from "@angular/core";

@Component({
  selector: "app-child",
  template: " {{prop}} ",
})
export class ChildComponent implements OnChanges {
  @Input() prop: string;

  constructor() {}

  ngOnChanges() {
    console.log("ngOnChanges working!");
  }
}
```

<br/> <br/>

변화를 자세히 살피기 위해 `ngOnChanges` 메서드를 조금 수정한다. <br/>
[SimpleChanges](https://angular.io/api/core/SimpleChange) 클래스를 이용하면 특정 입력 프로퍼티의 **변화 이전 값, 이후 값, 첫번째 변화**를 확인할 수 있다.

- `SimpleChanges` : ngOnChanges 메서드 파라미터로 입력받기
- `previousValue`: 변화 이전 값을 갖는 클래스 변수
- `currentValue`: 변화 이후 값을 갖는 클래스 변수
- `firstChange`: 첫 변화시 **true** 반환하는 boolean 타입의 클래스 변수

```typescript

import { SimpleChanges } from "@angular/core"; //추가
//..
@Input() prop: string;
//..
ngOnChanges(changes: SimpleChanges) {
    console.log(
      changes.prop.previousValue + " => " + changes.prop.currentValue
    );  // 출력: undefined => hi

    if (changes.prop.firstChange)
        console.log("first change"); // 출력: first change
    else
        console.log("change");
  }
```

<br/>

특정 입력 프로퍼티를 가리킬때 다른 표현과 메서드를 사용할 수 있다.

- `isFirstChange()`: 첫 변화시 **true** 반환하는 메서드

```typescript
if (changes.prop.firstChange) console.log("first change"); // 출력: first change

if (changes["prop"].isFirstChange()) console.log("first change"); // 출력: first change
```

<br/>

## SimpleChanges 클래스 객체 사용시 주의해야할 점

SimpleChanges 클래스의 변수를 사용하며 주의할 점은 아래처럼 작성할 경우 위와 다르게 동작된다. 이는 changes 객체가 오직 입력 프로퍼티만 감지하고 있진 않음을 의미한다.

- `changes.currentValue` 의 값에 **undefined**가 저장됨
- `firstChange` 메서드는 **false** 반환

```typescript

ngOnChanges(changes: SimpleChanges) {
    console.log(
      changes.previousValue + " => " + changes.currentValue
    );  // 출력: undefined => undefined


    if (changes.firstChange)
        console.log("first change");
    else
        console.log("change"); // 출력: change
  }
```

참고시 도움이 되는 글

- [https://medium.com/@sithummeegahapola/what-is-angular-simplechanges-in-ngonchange-method-2e6b8e7f411d](https://medium.com/@sithummeegahapola/what-is-angular-simplechanges-in-ngonchange-method-2e6b8e7f411d)
