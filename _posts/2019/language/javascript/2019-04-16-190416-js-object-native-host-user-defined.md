---
title: "자바스크립트 객체의 종류"
excerpt: "자바스크립트 객체는 크게 3가지로 구분할 수 있습니다. 그 3가지는 네이티브 객체, 호스트 객체, 사용자 정의 객체입니다."
search: true
categories: 
  - Language
  - Javascript
tags: 
  - Javascript
  - 자바스크립트
  - 네이티브 객체
  - 호스트 객체
  - 사용자 정의 객체
last_modified_at: 2019-04-16T23:25:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190416-js-object-native-host-user-defined-560x315.png
---

## 자바스크립트 객체 종류

자바스크립트의 객체는 크게 3가지로 구분할 수 있습니다. 네이티브 `Native` 객체, 호스트 `Host` 객체, 사용자 정의 `User defined` 객체가 있습니다.

## 네이티브 객체

네이티브 객체(`Native Object`)는 `ECMAScript`에서 정의된 객체를 뜻합니다. 네이티브 객체에는 `Obejct`, `String`, `Number`, `Boolean`, `Array`, `Function`과 같이 내장 생성자가 있고, `JSON`, `Math`, `Reflect` 등이 있습니다.  

`JSON`은 `JSON` 데이터 처리를 할 때 사용하는 객체입니다. `Math`는 수학적인 함수와 상수를 사용할 때 이용하는 객체입니다. `Reflect`는 `ES6`에서 생긴 객체로, 자바스크립트의 작업을 중간에 가로챌 수 있는 내장 객체입니다.  

**참고** Function 생성자는 함수를 만들어주는 생성자 함수입니다. 자세한 내용은 <a href="/language/javascript/190416-js-function-object/" target="_blank">여기</a>를 누르시면 글을 확인 할 수 있습니다.
{: .notice--info}


## 호스트 객체

호스트 객체는 `ECMAScript`에는 정의가 되지 않았지만, 브라우저에서 제공해주는 객체로 `Window`, `Navigator`, `History`, `Location` 등이 있습니다. 즉 클라이언트 측(브라우저 `javascript`)에 정의된 객체라고 생각하면 됩니다.  

웹 개발을 좀 해보신 개발자분들은 충분히 많이 사용해본 객체들일겁니다. 그래도, 간단하게 설명을 하자면, `window` 객체는 DOM 문서를 포함한 브라우처의 창을 나타냅니다. `Navigator`는 브라우저의 정보를 제공하는 객체입니다. `Location` 인터페이스는 객체가 연결된 장소(URL)를 표현합니다. 

## 사용자 정의 객체

사용자 정의 객체는 말 그대로 사용자가 생성한 객체를 뜻합니다.

## Reference

[https://developer.mozilla.org/ko/docs/Web/API/Window](https://developer.mozilla.org/ko/docs/Web/API/Window "모질라 Winodw 객체")  

[https://opentutorials.org/course/1363/6650](https://opentutorials.org/course/1363/6650 "생활코딩 Navigator 객체")

[https://developer.mozilla.org/ko/docs/Web/API/Location](https://developer.mozilla.org/ko/docs/Web/API/Location "모질라 Location 객체")  

