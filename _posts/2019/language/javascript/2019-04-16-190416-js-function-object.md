---
title: "자바스크립트 Function 객체"
excerpt: "자바스크립트 Function 객체 사용하는 방법에 대해 살펴봅니다. 간단하게 설명하면, Function 생성자는 함수를 생성하는 내장 객체입니다. 하지만, 전역변수와 인자로 넘겨준 지역변수만 사용할 수 있다는 단점이 있습니다."
search: true
categories: 
  - Language
  - Javascript
tags: 
  - Javascript
  - 자바스크립트
  - Function
last_modified_at: 2019-04-16T22:50:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190416-js-function-object-560x315.png
---

## Function 생성자

자바스크립트에는 `Function`이라는 생성자가 있습니다. `Function` 생성자는 함수를 생성하는 내장 객체입니다. 예를들어서, 제곱을 하는 함수 `square`를 생성해봅시다.  

`square` 함수는 일반적으로 생성 할 때, 아래와 같이 생성 할 수 있습니다.

```javascript
function square(x){
    return x * x
}
console.log(square(3)); // 9
```

`Function`을 이용하여 생성할 때는 아래와 같이 생성합니다.

```javascript
const square = new Function("x", "return x * x");
console.log(square(3)); // 9
```

## 정의하는 방법

일반적으로 인수가 `n`개 일때는 아래와 같이 사용합니다.

```javascript
var 변수이름 = new Function(첫번째인수, 두번째인수, ... , n번째인수, 함수본문);
```

예제를 좀 더 살펴보면

```javascript
// 더하기와 뺄셈
const add = new Function("a", "b", "return a + b");
const sub = new Function("a", "b", "return a - b");
console.log(add(5, 4)); // 9
console.log(sub(8, 1)); // 7
```

```javascript
// 리턴값이 없는 함수
const my_alert = new Function("console.log('Alert!'); alert('Alert!!');");
my_alert(); // 콘솔창에는 "Alert!" 출력. 그리고 "Alert!!"이라는 알림창이 뜸.
```

## 단점

하지만, 전역변수와 인자로 넘겨준 지역변수만 사용할 수 있다는 단점이 있습니다. 그리고 `Function`의 특징을 악용하여, 블랙 해커가 `Function` 생성자의 함수 본문에 악성 코드를 넣어 실행할 수도 있다는 단점이 있습니다.
