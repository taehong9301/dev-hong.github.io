---
title: "스칼라 함수 정의하기"
excerpt: "스칼라 함수를 정의하는 방법에 대해 살펴봅니다. 스칼라에서는 함수를 정의할때 def라는 키워드를 이용합니다."
search: true
categories: 
  - Language
  - Scala
tags: 
  - Scala
  - 스칼라
last_modified_at: 2019-04-26T20:59:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190426-scala-function-definition.png
---

## 함수 정의

스칼라에서 함수를 만드는 방법에 대해 살펴봅시다. 아래 예제는 정수를 2개를 받아서 둘 중 큰 정수를 반환하는 메소드입니다.

```scala
def max(x: Int, y: Int): Int = {
    if (x > y)
        x
    else
        y
}
```

```scala
println(max(3, 5)) // 5
```

위 예제를 통해 알 수 있듯이,

- 스칼라에서는 함수를 정의할 때는 `def`를 이용한다. `def`
- 그 뒤에는 함수의 이름이 나온다. `max`
- 괄호와 쉼표를 이용하여 파라미터를 정의하고, 파라미터의 타입을 명시 할때는 콜론을 이용한다. `(x: Int, y: Int)`
- 그 뒤에는 반환값의 타입을 명시한다. `(x: Int, y: Int): Int`
- 중괄호를 이용하여 본문을 작성한다. `= {}`

## 함수의 본문이 한문장인 경우

위 `max` 함수와 같이 본문이 한 문장의 경우는 중괄호를 생략하여 식처럼 사용이 가능합니다.

```scala
def max2(x: Int, y: Int): Int = if(x > y) x else y
```

## 반환값이 없는 함수

프로그래밍을 하다보면 종종 반환값이 없는 함수를 정의하곤 합니다. 자바에서는 `void`를 이용하여 나타내지만, 스칼라에서는 `Unit()`을 이용하여 나타냅니다.

```scala
def greet(): Unit = println("Hello, Scala!")
greet() // Hello, Scala!
```

`Unit()`은 해당 함수가 반환하는 값이 없다는 것을 의미합니다. 자바의 `void` 타입과 비슷합니다.

<i class="far fa-laugh-wink"></i> **마지막으로,** 본 글은 스칼라 공부를 하면서 작성한 글입니다. 잘못된 내용이 있거나 추가적인 내용이 필요한 경우, 댓글이나 이메일로 알려주세요. 해당 글을 가져가실 때는 출처를 남겨주시면 감사하겠습니다. :)
{: .notice--success}