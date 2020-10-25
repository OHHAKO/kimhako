---
layout: post
title: "[java] 예외(Exception) 이해하기"
excerpt: "예외 작성하는 이유와 방법"
categories: [java]
comments: true
---
## 에러
프로그램 실행 시에 오작동 혹은 비정상적 종료가 일어날 때가 있다.
-	컴파일 에러: 컴파일 할 때 발생하는 에러
-	런타임 에러: 프로그램을 실행하는 도중에 발생하는 에러

## 오류를 에러, 예외로 구분하자
프로그램 오류가 발생한 이유를 기준으로 에러, 예외를 구분한다.
-	에러(error): 하드웨어,JVM 실행 문제로 발생한 응용프로그램 실행 오류
-	예외(Exception): 사용자의 잘못된 조작, 잘못된 코딩으로 발생한 응용프로그램 실행 오류

 위의 오류가 발생하면 프로그램은 종료된다.  <br>
에러는 개발자가 대처할 수 없으나 예외는 대처가 가능하다. <br>
예외 처리의 목표는 적절한 코드를 사용해 프로그램을 종료없이 정상실행 상태로 유지시키는 것이다. 
<!-- 예외처리의 방법은 예외 타입에 따라 다르다. -->

## 예외 작성시 지켜야할 규칙
### 1. 예외는 진짜 예외 상황에만 써라
예외는 예외 상황에 쓰도록 설계되어 있다. <br>
반복문, API 등을 쓰면서 일상적인 제어 흐름용으로 쓰면 효율이 떨어진다. 

반복문에 예외를 쓴 코드다.
```java
try{
  int i = 0;
  while(true)
    range[i++].climb();
}catch(ArrayIndexOutOfBoundsException e){
  //처리 수행
}
```

표준 관용구를 작성한 코드다. <br>
무슨 일을 하는지 직관적으로 알아볼 수 있다.
```java
for(Mountain m : range){
  m.climb();
  }
```
반복문에 예외를 쓰면 오히려 속도, 가독성이 떨어진다. <br>
혹여나 반복문에 버그가 있다면 디버깅에 혼란을 줄 수 있다. <br><br>

특정상태의 동작을 결정하고 싶다면 예외보단 'API'를 쓰자. <br>
잘 설계된 API를 쓰면 예외를 사용할 일이 없다. <br><br>
다음은 Iterator 인터페이스를 사용한 코드다. <br>
hasNext(): 다음 요소가 존재할 때 true를 반환한다.<br>
Next(): 다음 요소로 이동시킨다.
```java
for(Iterator<Foo> i = collection.iterator(); i.hasNext(); ){
  Foo foo = i.next();
  ...
}
```

next는 상태 의존적 메서드 <br> 
hasNext는 상태 검사 메서드 <br> 
- 상태검사 메서드: 상태를 검사하여 결과를 반환한다
- 상태의존적 메서드: 특정 상태에서만 호출할 수 있다

상태가 올바르지 않을 때는 빈 옵셔널(Optional&lt;T&gt; 빈값) 혹은 null 을 반환하는 방법이 있다.

### 2. 예외 종류에 따라 쓰임새를 달리하자
자바는 예외 타입(throwable) 세가지를 제공한다. 
상황에 따라 예외를 작성하면 
-	검사 예외
-	런타임 예외
-	에러
<br><br>

#### 검사 예외
일부 메서드는 예외를 던지도록 설계되어 있다. <br>
해당 API를 사용할 때는 반드시 try/catch를 사용해 상황을 복구하도록 구현한다. <br> 
예외 상황에서 벗어나기 위해 필요한 정보를 알려주는 메서드를 함께 제공하자 <br>


#### 런타임 예외(RuntimeException)
비검사 예외 라고도 한다. 런타임 에러는 프로그램 오류 시, 특히 개발자 부주의로 발생한다. <br>
검사 예외와 달리 예외 처리에 강제성이 없다. <br>
발생 가능한 런타임 예외의 종류는 RuntimeException을 상속한 하위 클래스다. <br>
(ArithmeticException, IndexOutOfBoundsException,illegalArgumentException,IOException 등이 런타임 예외에 해당)


복구 가능하면 검사 예외를 쓰고, 프로그래밍 오류는 런타임 예외를 사용하라. <br>
런타임 예외나 에러는 잡을 필요가 없거나 통상 잡지 말아야 한다.

### 3. 필요 없는 검사 예외는 피하라
검사 예외는 발생 문제를 처리해 안정성을 높인다. <br>
그러나 과하게 사용하면 오히려 쓰기 불편한 API가 된다.

#### 이 경우가 아니라면 ‘검사예외’ 를 피하자
1. API를 제대로 사용하더라도 예외가 발생할 때
2. 개발자가 유의미한 조치를 취할 수 있을 때

#### 검사 예외 회피하는 방법
메서드가 단 하나의 검사 예외만 던질 때는 검사 예외를 쓰기가 부담스럽다.<br>
그럴때는 검사 예외를 던지는 대신 빈 옵셔널을 반환해 검사예외를 회피하는게 좋다.

다음은 검사 예외를 던지는 코드다
```java
try{
  obj.action(args);
}catch(TheCheckedException e){
  ...//예외 상황에 대처한다
}
```

코드를 리팩터링 하면 다음처럼 된다
```java
if(obj.actionPermitted(args)){
  obj.action(args);
}else{
  ...//예외 상황에 대처한다
}
```
이 리팩터링을 적용한다면 더 쓰기 편한 API를 제공할 수 있다.

<br><br><br><br><br>
---
아래를 참고해 작성했습니다.<br>
EFFECTIVE JAVA 3/E <br>
https://deftkang.tistory.com/44 <br>
https://hyeonstorage.tistory.com/199 <br>
