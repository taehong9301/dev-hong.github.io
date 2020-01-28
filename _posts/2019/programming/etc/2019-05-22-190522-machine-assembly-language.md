---
title: "기계어와 어셈블리어에 대하여"
excerpt: "컴퓨터가 이해할 수 있는 언어는 기계어가 있다. 기계어는 0과 1로 이루어진 언어이다. 기계어에서 조금 더 사람이 이해할 수 있게 만들어진 언어가 어셈블리어이다. LDA, ADD, STR과 같은 명령어가 있다."
search: true
categories:
  - Programming
tags:
  - Language
  - Programming
  - Machine Language
  - Assembly Language
last_modified_at: 2019-05-22T22:18:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: https://user-images.githubusercontent.com/26136312/58178010-5c9fb600-7ce0-11e9-9fe7-103623096cb7.png
gallery:
  - url: https://user-images.githubusercontent.com/26136312/58178010-5c9fb600-7ce0-11e9-9fe7-103623096cb7.png
    image_path: https://user-images.githubusercontent.com/26136312/58178010-5c9fb600-7ce0-11e9-9fe7-103623096cb7.png
    alt: "프로그래밍이란"
    title: "프로그래밍이란"
---

## 프로그래밍이란

**사람과 컴퓨터가 의사소통을 하기 위해 필요한 방법**으로 언어를 사용하는데, 이때 사용하는 언어를 프로그래밍 언어라고 한다. 그리고, 프로그래밍 언어를 이용하여 컴퓨터에게 명령을 내리는 일을 프로그래밍 또는 코딩이라고 한다.

{% include gallery id="gallery" caption="출처: https://hackr.io/blog/what-is-programming" %}

## 기계어

기계어는 `0`과 `1`로 이루어진 프로그래밍 언어로 컴퓨터가 직접 이해할 수 있는 유일한 언어이다. 유일하다는 말은 따로 번역이 필요없이 (어셈블리어에 의한 또는 컴파일러에 의한...) 컴퓨터가 이해할 수 있다는 뜻이다.

하지만, 기계어는 사람이 이해하기에는 전문적인 개발자 또한 어렵다. 그래서 사람이 좀더 이해하기 쉽게 나온 언어가 어셈블리어이다.

## 어셈블리어

어셈블리어는 0과 1로 이루어진 기계어를 **사람이 좀 더 이해하기 쉽도록 일대일로 대응시킨 언어**이다. 예로, `LDA`(Load Address), `ADD`(Add), `STA`(Store Address)와 같은 명령어가 있다. 간략하게 설명하면, LDA는 메모리 주소로부터 값을 가져오는 역할, ADD는 덧셈을 하는 역할, STA는 메모리 주소에 값을 저장하는 역할을 한다.

하지만 어셈블리어 또한 사람이 사용하기에 어려움이 있다. 이렇게 기계어와 어셈블리어는 사람보단 컴퓨터에 가까운 언어이다. 이런 언어를 **저급언어**(`Low Level Laguage`)라고 부른다.

그렇다면 반대로, 고급 언어(`High Level Laguage`)도 있지 않을까? 고급언어는 반대로 사람이 이해하기 좋은 언어이다. 예로는 포트란, 코볼, 베이직과 같은 고급언어가 개발되어 프로그램을 작성할 때 사용이 되었다. 이는 1950년 이전에 사용하던 언어로 현재는 거의 사용하지 않는다.

요즘은 `C`, `C++`, `JAVA`, `Python` 등과 같이 수 많은 언어가 등장하여 개발자 개인 취향에 맞게 사용하고 있다.
