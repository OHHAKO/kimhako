---
layout: post
title: "[java] 자바 특수문자, 정수, 유리수 출력"
excerpt: "특수문자를 출력하는 방법과 정수,유리수 타입의 변수 출력하기"
categories: [java]
modified: 2019-09-28
comments: true
---
## List Types
### Ordered Lists

1. 특수문자 출력하기
2. 정수 유리수 출력하기

### 1.특수문자 출력하기

자바에서 특수문자는 그대로 출력이 안됩니다. <br> 
출력스트림 System.out.println() 을 이용해 출력하면 `디버깅 오류`가 납니다.<br>
그대로 출력이 안되는 특수문자가 2개 있습니다. <br>
바로 \와 " 입니다. 이를 문자 그대로 출력하기 위해선 앞에 \를 붙여주어야 합니다. <br>

#### 예시 출력
출력스트림을 이용해 강아지를 출력했습니다.
{% highlight console %}
|\_/|
|q p|   /}
( 0 )"""\
|"^"`    |
||_/=\\__|
{% endhighlight %}


코드에는 이렇게 "와 \앞에 \를 포함해주어야 합니다.
{% highlight java %}
{% raw %}
System.out.println("|\\_/|");
System.out.println("|q p|   /}");
System.out.println("( 0 )\"\"\"\\");
System.out.println("|\"^\"`    |");
System.out.println("||_/=\\\\__|");
{% endraw %}
{% endhighlight %}


### 2.정수 유리수 출력하기

자바 데이터 타입중 int는 정수 double은 실수를 표현합니다.<br>
double형 변수를 선언해 소수점을 표현할때 주의할 점이 있습니다.

{% highlight java %}
int a=123;
double b = 100/3;
double c = 100.0 / 3.0;
		
System.out.println(a);
System.out.println(b);
System.out.println(c);
{% endhighlight %}

콘솔 출력 화면입니다.
{% highlight java %}
123
33.0
33.333333333333336
{% endhighlight %}

변수 b가 double형으로 선언되었지만 출력상엔 계산상 소수점이 나타나지 않았습니다.<br>
int형 피연산자의 계산 때문인데요. <br>
변수 c의 우변에 피연산자를 모두 실수로 표현했더니 결과값이 소수값 포함으로 나타났습니다.

`피연산자`가 숫자 아닌 `변수` 일때도 마찬가지 입니다.
{% highlight java %}
int a=123;
double d = a/10;
double e = (double)a/10;
		
System.out.println(d);
System.out.println(e);
{% endhighlight %}

콘솔 출력 화면입니다.
{% highlight java %}
12.0
12.3
{% endhighlight %}

`double형` 변수 d의 출력 결과는 `12.0`

`double형` 변수 e의 출력 결과는 `12.3`<br>
d와 e의 연산 차이는 (double)입니다.

a가 정수타입이기 때문에 실질적 연산 결과가 정수 타입의 영향을 받았고<Br>
이를 double형 즉 실수 변수에 담았기 때문에 12.0이 나오게 되었습니다.<Br>
정확한 값 표현을 위해 정수 타입의 영향을 `실수 타입의 영향` 으로 바꾸려면<Br>
`double e = (double)a/10;` 의 `(double)` 같은 형변환이 필요합니다.<Br>





