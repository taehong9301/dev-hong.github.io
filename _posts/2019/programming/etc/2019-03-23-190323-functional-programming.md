---
title: "함수형 프로그래밍"
excerpt: "프로그래밍 패러다임(`programming paradigm`)에는 여러가지 형태가 있습니다. 명령형 프로그래밍(절차지향 프로그래밍과 객체지향 프로그래밍)과 선언형 프로그래밍(함수형 프로그래밍)에 대해 알아봅니다."
search: true
categories: 
  - Programming
tags: 
  - Functional programming
  - 함수형 프로그래밍
  - Javascript
  - 자바스크립트
last_modified_at: 2019-03-25T21:40:00+09:00
toc: true
toc_sticky: true
comments: true
---
## 1. 프로그래밍 패러다임
### 1.1. 명령형 프로그래밍과 선언형 프로그래밍

프로그래밍 패러다임(`programming paradigm`)에는 여러가지 형태가 있습니다. 보통 학생때나 처음에 프로그래밍을 시작하면 절차지향 프로그래밍을 먼저 배우게 됩니다. (사실 자신이 하고 있는 방식이 절차지향 프로그래밍이라는 사실도 모른채 배우게 됩니다.) 그리고 C++이나 Java를 배우면서 객체지향 프로그래밍을 익히게 됩니다.  

이렇게 절차지향 프로그래밍과 객체지향 프로그래밍은 명령형 프로그래밍(`Imperative programming`)으로 분류가 됩니다. 이와 반대로 함수형 프로그래밍은 **선언형 프로그래밍**으로 명령형 프로그래밍과는 반대가 되는 개념입니다.  

본 글에서는 **함수형 프로그래밍**에 대해서 알아보려합니다.  

### 1.2. 함수형 프로그래밍

함수형 프로그래밍은 어떻게(How) 보단 무엇(What)을 표현하는 방식의 프로그래밍입니다. 사실 글로만 봐서는 이해하기 어렵습니다. 글보단 코드를 보면서 이해하는 편이 쉽습니다. 아래에 자바스크립트 코드를 보면  

```javascript
/*
Javascript
1. 명령형 프로그래밍
*/
var person = [
  { id: 1, name: "TH", age: 27 },
  { id: 2, name: "SJ", age: 32 },
  { id: 3, name: "IH", age: 38 },
  { id: 4, name: "SH", age: 29 },
  { id: 5, name: "WH", age: 30 },
  { id: 6, name: "WH", age: 36 },
]

// 30살 이상의 사람들을 리스트업한다.
var over30_person = []
for (var i = 0; i < person.length; i++) {
  if (person[i].age >= 30) {
    over30_person.push(person[i]);
  }
}
console.log(over30_person);
```

```javascript
// 결과
0: {id: 2, name: "SJ", age: 32}
1: {id: 3, name: "IH", age: 38}
2: {id: 5, name: "WH", age: 30}
3: {id: 6, name: "WH", age: 36}
```

간단한 코드입니다. `person`이란 리스트를 `for`문을 이용하여 하나씩 꺼내 30이란 숫자와 비교하고, 30 이상인 경우 새로운 리스트에 담아두는 코드입니다. `for`문을 돌리는 방법도 있지만 `while`문을 사용해도되고, `if`문 말고 `switch`문을 사용해도 됩니다. 이렇게 명령형 프로그래밍은 알고리즘에 대해 초점을 두고 프로그래밍을 합니다.  

반면 함수형 프로그래밍은 알고리즘보단 목표(What)를 표현하는 방식의 프로그래밍입니다. 

```javascript
/*
Javascript
2. 함수형 프로그래밍
*/
console.log(
  _filter(person, function(ps) { return ps.age >= 30})
);
```

위 코드만 보면, 내부 로직이 어떻게 되든 별로 상관이 없습니다. 다만 30살 이상인 사람들의 리스트를 알고 싶을 뿐입니다.  

### 1.3. 요약

명령형 프로그래밍: 목표는 명시하지 않고, 알고리즘(How)을 명시.

- 절차지향 프로그래밍(C, C++)
- 객체지향 프로그래밍(C++, JAVA, C#)

선언형 프로그래밍: 알고리즘은 명시하지 않고, 목표(What)를 명시.

- 함수형 프로그래밍(클로저, 하스켈, 리스프)


## 2. 사전 지식 (함수형 프로그래밍을 위한)

함수형 프로그래밍과 함께 알아야하는 개념이 있는데, 크게 3가지로 나눠본다면

- 순수 함수(Pure Function)
- 불변성(Immutablility)
- 1급 객체(First Class)

## 2.1. 순수함수 Pure Function

순수함수는 2가지로 설명할 수 있습니다.

1. Same Input, Same Output: 동일한 입력에는 항상 동일한 출력값을 반환한다.
2. No Side Effect: 부수효과가 없어야 한다.

[순수함수](/programming/190323-pure-function/ "순수함수 보러가기")를 보면 자세한 내용을 알 수 있습니다.  

## 2.2. 불변성 Immutablility

기본적으로 함수형 프로그래밍에서는 데이터가 변하면 안됩니다. 즉, 데이터가 변하지 않는 `불변성`을 지켜야 합니다.지고 있습니다. Scalar 또는 Kotlin의 val로 만들어진 데이터를 예를 들 수 있습니다.  

만약 데이터의 변경이 필요한 경우, 원본 데이터를 건드리지 않고 새로운 변수에 원본 데이터를 복사하여 작업합니다.

```python
# 변수가 바뀜
def string_to_int(str_list):
  str_list = map(int, str_list)
  return str_list

# 새로운 변수 int_list에서 작업을 진행.
def string_to_int(str_list):
  int_list = map(int, str_list)
  return int_list
```

## 2.3. 1급 객체 First Class

1급객체는 아래 3가지를 만족해야 합니다.

1. 함수를 변수나 데이터 구조에 넣을 수 있다.
2. 파라미터로 함수를 넘길 수 있다.
3. 리턴값으로 함수를 리턴할 수 있다.

[1급객체](/programming/first-class-citizen/ "1급 객체 보러가기")를 통해서 자세한 내용을 알 수 있습니다.  


## Reference

[WIKI-프로그래밍 패러다임](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%ED%8C%A8%EB%9F%AC%EB%8B%A4%EC%9E%84 "프로그래밍 패러다임")  

[WIKI-명령형 프로그래밍](https://ko.wikipedia.org/wiki/%EB%AA%85%EB%A0%B9%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D "명령형 프로그래밍")  

[WIKI-선언형 프로그래밍](https://ko.wikipedia.org/wiki/%EC%84%A0%EC%96%B8%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D "프로그래밍")  

[WIKI-함수형 프로그래밍](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D "함수형 프로그래밍")  

[https://velog.io/@kyusung/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9A%94%EC%95%BD](https://velog.io/@kyusung/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9A%94%EC%95%BD "함수형 프로그래밍")  