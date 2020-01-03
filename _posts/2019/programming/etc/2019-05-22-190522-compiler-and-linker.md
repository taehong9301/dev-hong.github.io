---
title: "컴파일러와 링커"
excerpt: "컴파일러, 어셈블러, 링커, 로더. 평소에 많이 듣지만 정확하게 모르는 사람도 적지않다."
search: true
categories: 
  - Programming
tags: 
  - Programming
  - Complier
  - Linker
  - Loader
  - Assembler
last_modified_at: 2019-05-22T22:28:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190522-compiler-and-linker.png
compiler_gallery:
  - url: https://user-images.githubusercontent.com/26136312/58178545-5bbb5400-7ce1-11e9-8562-9bc6bd138c5c.png
    image_path: https://user-images.githubusercontent.com/26136312/58178545-5bbb5400-7ce1-11e9-8562-9bc6bd138c5c.png
    alt: "Compiler"
    title: "Compiler?"
linker_gallery:
  - url: https://user-images.githubusercontent.com/26136312/58178542-5b22bd80-7ce1-11e9-876f-8d19400cd5c9.png
    image_path: https://user-images.githubusercontent.com/26136312/58178542-5b22bd80-7ce1-11e9-876f-8d19400cd5c9.png
    alt: "Linker"
    title: "Linker?"
---

## 컴파일러

사람은 기계어를 이해하기 힘들고 마찬가지로 컴퓨터는 고급언어를 이해하기 힘들다. 그렇기 때문에 중간에서 번역하는 역할을 해줘야하는데, 이 역할을 컴파일러(`Compiler`)가 한다. 그리고 번역하는 행위를 컴파일(`Compile`)이라고 한다.  

{% include gallery id="compiler_gallery" caption="출처: https://codeforwin.org/2017/05/compiler-and-its-need.html" %}

사람이 고급언어로 작성한 파일을 원시파일 또는 소스파일(`Source File`)이라고 한다. 소스파일을 컴파일러가 기계어로 번역하여 파일을 만드는데, 이때 생성된 파일을 목적파일(`Object File`)이라고 한다.  

## 어셈블러

고급 언어를 기계어로 번역하는 일을 컴파일러가 한다면, 어셈블리어는 누가 번역을 할까? 어셈블리어는 어셈블러(`Assembler`)가 기계어로 번역하는 일을 한다. 간단하게 설명하면 컴파일러나 어셈블러나 (기계어보다) 수준 높은 언어로 작성된 파일을 번역하여 기계어로 만들어주는 역할을 한다.  

## 링커

프로그래밍을 개발을 할 때는 하나의 파일에 모든 코드를 작성하지 않는다. 협업을 하거나 추후 프로젝트 관리를 위하여 보통 파일을 나눠서 효율적으로 프로그램을 만든다. 컴파일러에 의해 수 많은 목적파일(Object File)이 생성이 되는데 이 목적파일을 묶어서 실행을 할 수 있는 파일을 만드는것을 링커(`Linker`)가 한다. 이렇게 묶는 작업을 링킹(Linking)이라고 부른다.  

여기서 실행파일이 뭔지 궁금할 수도 있는데, Window 운영체제를 예를들면, `.exe` 파일이나 `.com` 파일을 말한다.  

{% include gallery id="linker_gallery" caption="출처: https://towardsdatascience.com/understanding-compilers-for-humans-version-2-157f0edb02dd" %}

## 라이브러리

반복되는 코드가 있는 경우, 매번 그 코드를 작성하여 프로그래밍을 하지 않는다. 보통은 라이브러리(Library)로 만들어서 파일로 저장을 해둔다. 그리고 필요할때 해당 파일을 불러와서 사용을 한다. (반복된 코드를 줄일 수 있으니 효율적이다!) 라이브러리는 `.dll`과 같은 파일을 말한다.

## 로더

프로그래밍 언어로 프로그래밍을 하고 원하는 프로그램을 만들고 했다면... 실행은 누가 할까?  

실행은 로더(`Loader`)가 프로그램을 주기억장치 또는 메모리에 로드(Load)하여 실행을 한다. (메모리는 `RAM`을 말함) 참고로 실행되지 않는 실행 파일을 프로그램이라고 하며, 프로그램을 실행하여 메모리에 적재(Load)된 경우 프로세스(`Process`)라고 부른다.  