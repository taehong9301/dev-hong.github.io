---
title: "자바스크립트에서 HTML, CSS와 Attribute 다루기"
excerpt: "Element API를 이용하여 HTML, CSS와 속성(attribute)을 다루는 방법을 소개합니다. innerText, innerHTML, style, getAttribute, setAttribute."
search: true
categories: 
  - Language
  - Javascript
tags: 
  - Javascript
  - 자바스크립트
  - Element API
last_modified_at: 2019-04-04T22:58:00+09:00
toc: true
toc_sticky: true
comments: true
---

## 1. HTML 다루기

`innerHTML`, `innerText` 이용하여 HTML 문서를 다룰 수 있습니다.

- innerHTML: 엘리먼트 안의 HTML 코드를 변경
- innerText: 엘리먼트 안의 텍스트를 변경

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width">
        <title>DOM에 대해</title>
    </head>
    <body>
        <h1 id="title">Hello!</h1>
        <p id="contentHtml"></p>
        <p id="contentText"></p>
        <script>
            // innerHTML
            const contentHtml = document.getElementById("contentHtml");
            contentHtml.innerHTML = "This is <b>Bold!</b>";

            // innerText
            const contentText = document.getElementById("contentText");
            contentText.innerText = "This is <b>Bold!</b>";
        </script>
    </body>
</html>
```

![스크린샷 2019-04-04 오후 10 42 29](https://user-images.githubusercontent.com/26136312/55561178-ac282380-572c-11e9-9615-65781d199ca9.png)  

예제 결과를 보면 알 수 있듯이, `innerHTML`은 html 코드를 넣을 수 있고 `innerText`는 html코드가 아닌 텍스트를 넣을 수 있습니다.

## 2. CSS 다루기

CSS를 다룰때는 `.style` 을 이용합니다.

- style: CSS를 변경

```html
<body>
  <h1 id="title">Hello!</h1>
  <p id="contentHtml"></p>
  <p id="contentText"></p>
  <script>
      // innerHTML
      const contentHtml = document.getElementById("contentHtml");
      contentHtml.innerHTML = "This is <b>Bold!</b>";

      // innerText
      const contentText = document.getElementById("contentText");
      contentText.innerText = "This is <b>Bold!</b>";

      // style
      const title = document.getElementById("title");
      title.style.color = "yellowgreen";
      title.style.fontSize = "50px";
  </script>
</body>
```

![스크린샷 2019-04-04 오후 10 48 45](https://user-images.githubusercontent.com/26136312/55561176-ac282380-572c-11e9-98d8-7e327b2daee4.png)  


## 3. Attribute 다루기

속성값을 가지고 오거나 변경을 할때는 `getAttribute`, `setAttribute`를 이용합니다.

- getAttribute: 엘리먼트의 속성값을 가져옴.
- setAttribute: 엘리먼트의 속성값을 변경.

```html
<body>
    <a id="google" href="https://google.com">구글</a>
    <script>
        const google = document.getElementById("google");
        console.log(google.getAttribute("href"));

        google.setAttribute("href", "https://naver.com");
        console.log(google.getAttribute("href")); // naver로 바뀜.
    </script>
</body>
```

![스크린샷 2019-04-04 오후 10 52 54](https://user-images.githubusercontent.com/26136312/55561175-ac282380-572c-11e9-9ffb-09d9c42fd5ac.png)  

원래는 구글을 향하는 앵커 태그였지만, `setAttribute`에 의해 네이버 앵커태그로 바꼈습니다.