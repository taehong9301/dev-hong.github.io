---
title: "스칼라 배열 생성 및 파라미터화(Parameterization)"
excerpt: "스칼라에서 배열을 생성하는 방법과 파라미터화(Parameterization) 하는 방법을 소개하고, val로 만들어진 배열에 대해 소개합니다."
search: true
categories: 
  - Language
  - Scala
tags: 
  - Scala
  - 스칼라
last_modified_at: 2019-04-03T20:40:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190403-scala-parameterization.png
---

## 배열에 타입 파라미터를 지정

스칼라에서는 `new`를 사용하여 **클래스의 인스턴스(객체 인스턴스화)를 만들 수 있습니다.** 그리고, 값과 파라미터를 넘기는 것이 가능합니다.  

파라미터화를 할 때는 괄호`()` 또는 대괄호`[]` 안에 객체를 넘겨 생성자에게 넘기면 됩니다.


<i class="fas fa-feather-alt"></i> **파라미터화** `Parameterization`은 인스턴스를 생성할 때, 그 인스턴스를 설정(`configure`)한다는 뜻이다.
{: .notice--info}

파라미터화 하는 방법에는 2가지 방법이 있습니다.

- `값`을 이용한 방법
- `타입`을 이용한 방법


### 값을 이용한 파라미터화

값을 이용하여 파라미터화 할 때는 괄호안에 객체를 넘겨 파라미터화 합니다.

```scala
val big = new java.math.BigInteger("123451234512345")
println(big) // 123451234512345
```

위 예제는 `new` 키워드를 이용하여, 자바 모듈의 `BigInteger` 객체를 `123451234512345` 문자열을 넘겨서 인스턴스 `big`을 만드는 예제입니다.  


### 타입으로 파라미터화

타입으로 파라미터화 할 때는 대괄호를 이용합니다.

```scala
val greetStrings = new Array[String](3)
```

위 예제는 길이 3의 문자열 배열을 만드는 예제입니다. 문자열 배열을 만들때는 `new Array[String]` 타입 파라미터화를 이용하고, 길이 3을 지정할때는 `new Array[String](3)` 값으로 파라미터화 합니다.  

- `[String]`: 문자열 배열(array of string) - 타입으로 파라미터화
- `(3)`: 배열의 길이 - 값으로 파라미터화

배열 객체를 만들어서 `Hello, Scala!`를 출력해보면,

```scala
object Exam {
  def main(args: Array[String]) {
    val greetStrings = new Array[String](3)  // 인스턴스 생성
    
    // 배열에 접근할 때는 괄호를 이용한다.
    greetStrings(0) = "Hello"
    greetStrings(1) = ", "
    greetStrings(2) = "Scala!"

    // 출력
    for (i <- 0 to 2)
      print(greetStrings(i))
  }
}
```

```bash
Hello, Scala!
```

여기서 주의할점이 있는데, 스칼라에서는 배열에 접근할때 대괄호`[]`가 아닌 괄호`()`를 사용합니다.  

그리고, `greetStrings`에는 타입을 명시하지 않았지만, **타입추론**에 의해서 `Array[String]`으로 타입이 결정됩니다. 여기서 `greetStrings`의 타입은 `Array[String]`이지 `Array[String](3)`가 아닙니다.  

사실 위 코드는 아래와 같은 코드입니다.

```scala
val greetStrings = new Array[String](3)
greetStrings.update(0, "Hello")
greetStrings.update(1, ", ")
greetStrings.update(2, "Scala!\n")

for (i <- 0 to 2)
  print(greetStrings.apply(i))
```

스칼라 컴파일러는 `greetStrings(0) = "Hello"`는 `.update()` 메소드를 호출하는 것으로 바꿔줍니다. 그리고, `print(greetStrings(i))`의 경우도 `.apply()` 메소드를 호출하는 것으로 바꿔줍니다.  

<i class="fas fa-feather-alt"></i> **Note** 여기서 스칼라는 배열부터 수식에 이르는 **모든 것을 메소드 객체로 다룬다는 것**을 알 수 있습니다.
{: .notice--info}

## val 변수의 배열

스칼라에는 변수를 선언하는 방법이 2가지가 있습니다. `var`를 이용한 방법과 `val`를 이용한 방법이 있는데, `var`는 변경이 가능한(mutable)한 변수이고, `val`은 변경이 불가능한(immutable) 변수입니다.  

하지만, 위 예제를 보면 `greetStrings` 배열은 `val`을 이용하여 만들었지만 값을 넣을 수 있습니다. 이 부분이 착각을 할 수 있는 부부인데, 배열을 만들면 변수는 그 배열의 주소값(참조값)을 참조합니다. 

그래서 아무리 원소를 넣더라도 주소값은 변하지 않기 때문에 `val` **변수라도 원소를 추가할 수 있습니다.**  

```scala
val greetStrings = new Array[String](3)
greetStrings(0) = "Hello"
greetStrings(1) = ", "
greetStrings(2) = "Scala!\n"
for ( i <- greetStrings)
  print(i)

greetStrings(2) = "World!" // 값을 변경
for (i <- greetStrings)
  print(i)
```

```bash
Hello, Scala!
Hello, World!
```

위와같이 원소의 값을 변경하더라도, 문제없이 동작하는 것을 볼 수 있습니다.  

정리하면, `val`로 만들어진 배열은 다른 길이 또는 타입의 배열을 넣는것은 불가능 하지만, 그 배열의 **원소는 변경이 가능**(`mutable`) 합니다.


## 스칼라 특징

스칼라에서는 메소드가 파리미터 하나만을 요구하는 경우, 그 메소드를 점(.)과 괄호 없이 호출이 가능합니다.  

```scala
for (i <- 0 to 2)
  print(greetStrings(i))
```

위에서 `0 to 2`는 사실  `0.to(2)`입니다. 여기서 스칼라는 모든것이 함수 객체라는 것을 다시 알 수 있습니다.  

비슷한 내용으로 스칼라에는 우리가 아는 보통의 연산자가 없기 때문에, 연산자 오버로딩을 제공하지 않습니다. `+`, `-`, `*`, `/` 등은 메소드 이름으로 사용이 가능합니다. 그렇기 때문에 `1 + 2`는 사실 `1.+(2)`와 같습니다.  


<i class="fas fa-feather-alt"></i> **Note** 스칼라에서는 모든 연산자가 **메소드 호출**입니다.
{: .notice--info}


## 배열을 생성하는 다른 방법

위에서는 배열의 길이를 먼저 생성한 다음에 원소를 추가하는 형식으로 배열 생성 했지만, 처음부터 초기화를 하면서 생성할 수 있습니다.

```scala
val numNames = Array("one", "two", "three")
```

마찬가지로 사실은 `.apply()` 메소드를 호출하는 것과 같습니다.  

```scala
val numNames = Array.apply("one", "two", "three")
```

<i class="far fa-laugh-wink"></i> **마지막으로,** 본 글은 스칼라 공부를 하면서 작성한 글입니다. 잘못된 내용이 있거나 추가적인 내용이 필요한 경우, 댓글이나 이메일로 알려주세요. 해당 글을 가져가실 때는 출처를 남겨주시면 감사하겠습니다. :)
{: .notice--success}