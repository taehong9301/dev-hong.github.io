---
title: "스칼라 인터프리터"
excerpt: "스칼라를 도구(인텔리제이)와 같은 방법으로 개발할 수도 있지만, 인터프리터를 이용하여 대화하듯이 프로그래밍을 할 수도 있습니다. 이런 스칼라 인터프리터 쉘`sell`을 `scala`라고 부릅니다."
search: true
categories: 
  - Language
  - Scala
tags: 
  - Scala
  - 스칼라
last_modified_at: 2019-04-26T20:04:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190426-scala-interpreter.png
---

## 스칼라 인터프리터란?

스칼라를 시작하기 가장 쉬운방법은 **인터프티터를 이용하는 방법**입니다. 스칼라를 통합개발환경(인텔리제이와 같은)을 이용하여 개발을 할 수도 있지만, 간단한 프로그램을 작성할 때는  굳이 사용을 하지 않아도 됩니다.  

스칼라 인터프리터를 이용하면, 대화를 하듯이 프로그래밍을 할 수 있습니다. 스칼라의 대화형 쉘은 `scala`라고 부릅니다.  

<i class="fas fa-feather-alt"></i> **스칼라 인터프리터** 파이썬의 인터프리터와 사용하는 방법이나 형식은 동일하다. 다만 문법의 차이와 보여주는 화면이 다르다. 그리고 이러한 대화형 쉘을 인터프리터라고 부른다.
{: .notice--info}

## 스칼라 인터프리터 사용법

본 글은 `Window10`환경에서 진행하기 때문에 `cmd`를 열어서 `scala` 명령어를 입력합니다.

```
C:\Users\75385>scala
```

<i class="fas fa-exclamation-circle"></i> **스칼라 설치** 인터프리터를 사용하기 위해서는 기본적으로 스칼라를 설치하는 것은 당연합니다. 본 글에서는 설치하는 내용에 대해 다루지 않습니다. 설치하는 방법에 대해서는 아래 링크를 확인 바랍니다.  
<a href="/programming/190421-install-openjdk/" target="_blank">OpenJDK 설치</a>  
<a href="/programming/190421-install-scala/" target="_blank">스칼라 설치</a>
{: .notice--danger}

아래와 같이 나온다면, 스칼라 인터프리터를 사용하기 위한 준비는 끝났습니다.

```
Welcome to Scala 2.12.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_201-1-ojdkbuild).
Type in expressions for evaluation. Or try :help.

scala>
```

간단한 사용방법에 대해 살펴봅시다.

```bash
scala> 1 + 2
```

```bash
res0: Int = 3
```

위 예제를 보고 스칼라의 특징을 알 수 있는데,

- 세미콜론 `;`을 사용하지 않는다.
- `res0`은 인터프리터에서 자동으로 생성해준 변수명으로, 해당 변수명에는 0번째 `result` 결과라는 뜻이 있다.
- 콜론 `:` 다음 결과 타입이 등장한다. 위 예제에서는 `Int`를 뜻한다.
- 자바의 경우 `int`라는 타입을 사용하지만, 스칼라는 `Int` 클래스를 사용한다.

```bash
scala> res0 * 3
res1: Int = 9
```

그리고, 자동으로 생성된 `res0`이란 변수를 다시 사용할 수 있습니다. 위에서 `res0=3`이었기 때문에 곱하기 `3`을 하면 값이 `9`가 나옵니다. 마찬가지로 인터프리터에 의해 `res1`이란 변수명으로 생성되는 것을 알 수 있습니다.  

프로그래밍 언어를 처음배운다면 항상 해보는 `Hello, World!`...

```bash
scala> println("Hello, World!")
Hello, World!
```

### 자바의 타입과 스칼라의 타입

자바에는 원시 타입(`primitive type`)이 존재하는데, `int`, `boolean`, `float` 등이 있습니다. 스칼라에서는 `scala.Int`, `scala.Boolean`, `scala.Float`를 사용합니다. (이렇게 객체를 사용하는 이유는 스칼라의 특징 중 하나인 "**모든것이 객체**"를 만족시키기 위해서)  

```bash
scala> import scala.Int
import scala.Int
```

```bash
scala> import scala.Boolean
import scala.Boolean
```

```bash
scala> import scala.Float
import scala.Float
```

단 착각할 수도 있는 타입이 존재하는데, 문자열 `String`입니다. 문자열의 경우는 기존 자바에서도 객체로 사용하고 있었기 때문에 scala.String은 존재하지 않고 기존 자바의 것을 사용합니다. 그래서, 아래와 같이 `import` 에러가 발생합니다.  

```bash
scala> import scala.String
<console>:14: error: object String is not a member of package scala
       import scala.String
              ^
```

```bash
scala> import java.lang.String
import java.lang.String
```

## 변수 정의 (val, var)

스칼라에는 변수를 정의하는 방법이 2가지가 있습니다. `val`을 이용한 방법과 `var`를 이용한 방법입니다.  

2개의 차이는 쉽게 말하면, 변경이 가능한가? 변경이 불가능한가? 로 나눌 수 있습니다. `val`의 경우는 자바의 `final`과 같이 변경이 불가능한 변수입니다. 반면, `var`의 경우는 계속해서 재할당이 가능한 변수입니다.  

- `val`: 자바의 `final`과 같이, 결코 다시 변경이 불가능한 `immutable` 변수
- `var`: 처음 선언이 되고 타입만 같다면, 계속해서 재할당이 가능한 `mutable`한 변수

### val 예시

```bash
scala> val msg = "Hello, world!"
msg: String = Hello, world!
```

```bash
scala> msg = "Hello, scala!"
<console>:16: error: reassignment to val
       msg = "Hello, scala!"
           ^
```

`Hello, world!`를 `val`을 이용하여 선언한 뒤에 `Hello, scala!`로 변경하려하지만 에러가 발생합니다. (`val`은 변경이 불가능하기 때문에!)

### var 예시

```bash
scala> var msg = "Hello, world!"
msg: String = Hello, world!
```

```bash
scala> msg = "Hello, scala!"
msg: String = Hello, scala!
```

반면, `var`를 이용하게되면 재할당을 하더라도 에러가 발생하지 않습니다. `val`과 반대로 `var`는 변경이 가능하기 때문입니다.

## 타입 추론

```bash
scala> val msg = "Hello, world!"
msg: String = Hello, world!
```

위 예제를 보면, 문자열의 데이터를 변수(`msg`)에 담았지만 `String`이라는 타입을 지정해주지 않았습니다. 마치 동적타입의 언어처럼 보이지만 **스칼라는 정적타입의 언어**입니다.  

위와같이 가능한 이유는 스칼라의 특징중 하나인 **타입 추론** 때문입니다. 변수를 선언할 때 그 뒤의 데이터 타입을 보고 문자열 `String`이라 타입을 추론합니다. 그리고 `String`으로 타입을 결정합니다.  

굳이 명시적으로 작성을 한다면, 아래와 같이 작성할 수 있습니다.

```bash
scala> val msg: java.lang.String = "Hello, world!"
msg: String = Hello, world!
```

또는

```bash
scala> val msg: String = "Hello, world!"
msg: String = Hello, world!
```

## 인터프리터 종료

인터프리터 종료를 할 때는 `:quit` 또는 `:q`를 이용합니다.  

```
scala> :q

C:\Users\75385>
```

<i class="far fa-laugh-wink"></i> **마지막으로,** 본 글은 스칼라 공부를 하면서 작성한 글입니다. 잘못된 내용이 있거나 추가적인 내용이 필요한 경우, 댓글이나 이메일로 알려주세요. 해당 글을 가져가실 때는 출처를 남겨주시면 감사하겠습니다. :)
{: .notice--success}