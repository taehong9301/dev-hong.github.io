---
title: "1급 객체 First Class"
excerpt: "1급 객체는 3가지 조건을 만족해야 합니다. 변수나 데이터 구조안에 담을 수 있다, 파라미터로 전달 할 수 있다, 리턴값으로 사용할 수 있다."
search: true
categories: 
  - Programming
tags: 
  - First Class
  - 1급 객체
  - Javascript
last_modified_at: 2019-03-23T17:20:48+09:00
toc: true
toc_sticky: true
comments: true
---
## 1. 1급 객체(First Class Citizen)
1급 객체는 아래 3가지 조건을 만족해야 합니다.
1. 변수나 데이터 구조안에 담을 수 있다.
2. 파라미터로 전달 할 수 있다.
3. 리턴값으로 사용할 수 있다.

### 1.2. 변수나 데이터 구조 안에 담을 수 있다.
`Javascript`에서는 여러가지 방법으로 함수를 변수에 담을 수 있습니다.  

#### 1.2.1. var 키워드를 이용한 방법
자바스크립트에서 변수를 선언할 때, `var`를 이용하는데 함수를 `var` 변수에 담으면 됩니다.  

>익명함수를 담아도 되고, 명명함수를 담아도 되고 둘다 가능하다!  

```javascript
// 익명 함수
var func = function(num1, num2) {
    return num1 + num2;
}
console.log( func(2, 3) );  // 5

// 명명 함수
var func2 = function add(num1, num2) {
    return num1 + num2;
}
console.log( func2(3, 5) );  // 8
```

#### 1.2.2. Map을 이용한 방법

Map 데이터 구조안에 함수(function)를 넣는 방법도 있습니다.  

```javascript
var person = {
    name: "Taehong",
    post: 1234,
    greet: function(){
        console.log("Hello!");
    }
}
person.greet.call();
```

### 1.3. 파라미터로 전달 할 수 있다.
```javascript
function add_func (num1, num2) {
    return num1 + num2
}

function add (func) {
    var result = func(3, 5)
    return result;
}

console.log( add(add_func) );  // 8
```

파라미터 값으로 함수를 받을 수도 있습니다. 사실 함수를 바로 반환해도 됩니다.  


### 1.4. 리턴값으로 사용할 수 있다.

함수가 함수를 받을 수도 있고, 함수를 반환할 수도 있습니다.  

```javascript
function f3(f) {
    return f()
}

var a = f3(function() { return "Hello"; });
console.log(a);  // Hello
```

## 2. 결론

자바스크립트(`Javascript`)는 아래 3가지 조건을 만족하기 때문에 1급 객체라고 할 수 있습니다.  

1. 함수를 변수나 데이타에 할당 할 수 있다.
2. 함수를 인자로 전달 할 수 있다.
3. 함수를 리턴 할수 있다.

물론 1급 객체에는 자바스크립트 뿐만 아니라 스칼라, 파이썬 등 여러가지 종류가 있습니다.  


## Reference

[https://medium.com/@soeunlee](https://medium.com/@soeunlee/javascript%EC%97%90%EC%84%9C-%EC%99%9C-%ED%95%A8%EC%88%98%EA%B0%80-1%EA%B8%89-%EA%B0%9D%EC%B2%B4%EC%9D%BC%EA%B9%8C%EC%9A%94-cc6bd2a9ecac "1급 객체")  