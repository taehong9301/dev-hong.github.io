---
title: "맥북 homebrew 설치하기"
excerpt: "본 글은 MacOS에 homebrew 설치하는 방법에 대해 다룹니다."
search: true
categories: 
  - Programming
tags: 
  - Programming
  - homebrew
last_modified_at: 2019-05-07T14:49:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190507-install-homebrew.png
---

## homebrew이란?

맥북에서 `homebrew`를 이용하여 패키지를 다운 받을 수 있습니다. 물론 맥북 뿐만 아니라 다른 리눅스 체제에서도 사용이 가능합니다.  

## homebrew 설치

<a href="https://brew.sh/index_ko" target="_blank">https://brew.sh/index_ko</a>에서 `homebrew`를 설치할 수 있습니다.  

사이트를 들어가면, 설치를 하기위한 명령어를 보여줍니다. (아래 명령어와 동일)  

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

간편하게 설치가 완료되고, 그 뒤에 homebrew를 이용하여 여러가지 패키지를 설치할 수 있습니다. (저의 경우는 `yarn` 설치를 위해서 `homebrew`를 설치했습니다.)  

## homebrew를 이용한 패키지 설치

`homebrew`에서 패키지를 설치할때는 `brew install`이라는 명령어를 이용합니다.  

```
$ brew install [패키지명]
```

예를들어, `yarn` 패키지를 설치하기 위해서는 아래와 같이 합니다.  

```
$ brew install yarn
```
