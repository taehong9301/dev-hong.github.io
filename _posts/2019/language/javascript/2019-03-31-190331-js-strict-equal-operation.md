---
title: "자바스크립트에서 ==와 === 차이"
excerpt: "자바스크립트의 관계 연산자 중에서는 ==와 ===가 있습니다. ==와 ===의 차이를 살펴봅니다."
search: true
categories: 
  - Language
  - Javascript
tags: 
  - Javascript
  - 자바스크립트
  - StrictEqualOperation
last_modified_at: 2019-03-31T20:14:00+09:00
toc: true
toc_sticky: true
comments: true
---

## 1. 서론: 관계연산자

관계연산자는 값을 비교하기 위해서 사용하는 연산자를 뜻합니다. 관계 연산자에는 `>`, `<`, `>=`, `<=`, `==`, `===`이 있습니다. 본 글에서는 `==`, `===`에 대해서 살펴보고 차이점에 대해 알아봅니다.


## 2. Equal operator ==

우리는 보통 ==를 이용하여 값을 비교할 때 사용합니다. 쉬운 예제를 보면,

```javascript
console.log( 3 == 3 );  // true
console.log( (1+2) == 3 );  // true
console.log( "hello" == "hello" );  // true
console.log( "Hello" == "hello" );  // false
```

위 예제는 쉬워서 쉽게 이해할 수 있습니다. 반면, 아래 예제는 잘 모르시는 분들이 많습니다.

```javascript
console.log( 3 == "3" );
console.log( (1+2) == "3" );
```

딱 봤을때는 왼쪽 `3`은 숫자이고, 오른쪽 `"3"`은 문자열이기 때문에 `false`가 나올것이라 생각할 수 있지만, 결과는 `true`입니다.  

`javascript`에서 **`==` 동치 연산자(Equal operator)는 타입까지는 비교하지 않고 오직 값을 비교**합니다. 그렇기 때문에 `true`가 나옵니다.  

그렇다면, 타입까지 비교하려면 어떻게 해야할까 의문이 생깁니다. 이때, 사용하는 것이 Strict equal operator(`===`)입니다. `=`를 3개를 사용합니다.  

## 3. Strict equal operator ===

타입까지 비교하는 Strict equal operator(`===`)에 대해 예제를 보면서 살펴봅시다.  

`Strict`는 `엄격한`, `엄밀한`이란 뜻을 가진 단어입니다. Strict equal operator는 엄격한 동치 연산자라는 뜻이 됩니다. 즉, **타입까지 비교하는 연산자** 이름에 딱 맞는 표현 같습니다.
{: .notice--info}

### 3.1. 숫자와 문자열 숫자

```javascript
console.log(3 == "3");  // true
console.log(3 === "3");  // false
console.log(3.14 == "3.14");  // true
console.log(3.14 === "3.14");  // false
```

숫자 `3`과 문자열 `"3"`은 `==`를 사용하면 타입까지 비교하진 않기 때문에 같다고 연산을 합니다. 반면 `===` 엄격한 동치 연산자를 사용하는 경우 다르다고 값이 나옵니다.

### 3.2. undefined와 null

`undefined`는 애초에 값이 없다는 뜻이고, `null`은 임의적으로 값이 없다고 명시한 것이기 때문에 엄연히 `undefined`와 `null`은 다른 타입입니다. 그래서 `==`를 했을 때는 값이 같지만, `===`를 했을때는 다르다는(`false`) 결과가 나옵니다.  

```javascript
console.log(undefined == null);  // true
console.log(undefined === null);  // false
```

### 3.3. boolean과 문자열 boolean

주의해야 할 점은 숫자와 문자열 숫자를 `==`로 비교했을 때, 값이 같다면 `true`가 나왔지만 `boolean`의 경우는 그렇지 않습니다.

```javascript
console.log( true == "true");  // false
console.log( true === "true");  // false
```

`==`를 사용하든 `===`를 사용하든, `true`와 `"true"`는 엄연히 다른 값으로 연산합니다.
{: .notice--danger}


## Reference

[https://programmers.co.kr/learn/questions/25](https://programmers.co.kr/learn/questions/25 "move")  

[https://hermeslog.tistory.com/303](https://hermeslog.tistory.com/303 "move")