---
title: "React Native(RN) 설치 및 Android 개발하기 (MAC OS 환경)"
excerpt: "React Native 설치를 하고, 프로젝트를 생성해보는 방법을 소개합니다."
search: true
categories:
  - ReactNative
tags:
  - ReactNative
  - RN
  - React
  - Javascript
last_modified_at: 2020-01-04T01:29:00+09:00
toc: true
toc_sticky: true
comments: true
# header:
#   teaser:
click-custom:
  - url: https://user-images.githubusercontent.com/26136312/71737273-8f28a880-2e96-11ea-9901-faca36998e3b.png
    image_path: https://user-images.githubusercontent.com/26136312/71737273-8f28a880-2e96-11ea-9901-faca36998e3b.png
    alt: "custom 클릭"
    title: "custom 클릭"
4-check:
  - url: https://user-images.githubusercontent.com/26136312/71737272-8f28a880-2e96-11ea-9f52-413dad45b341.png
    image_path: https://user-images.githubusercontent.com/26136312/71737272-8f28a880-2e96-11ea-9f52-413dad45b341.png
    alt: "4개 체크 박스에 체크"
    title: "4개 체크 박스에 체크"
install-android:
  - url: https://user-images.githubusercontent.com/26136312/71737271-8f28a880-2e96-11ea-8f94-8b4b00fd1f86.png
    image_path: https://user-images.githubusercontent.com/26136312/71737271-8f28a880-2e96-11ea-8f94-8b4b00fd1f86.png
    alt: "안드로이드 다운로드"
    title: "안드로이드 다운로드"
start-project:
  - url: https://user-images.githubusercontent.com/26136312/71738284-8e454600-2e99-11ea-88d7-d11483696cd2.png
    image_path: https://user-images.githubusercontent.com/26136312/71738284-8e454600-2e99-11ea-88d7-d11483696cd2.png
    alt: "프로젝트 실행"
    title: "프로젝트 실행"
---

# 0. 환경 및 준비

- 운영체제: 맥킨토시 `MAC OS`
- 준비물: `homebrew`

# 1. nodejs & watchman 설치

본 글에서는 `brew`를 이용하여 설치를 합니다. 아래 명령어를 수행합니다.

```
$ brew install node
$ brew install watchman
```

<i class="fas fa-feather-alt"></i> **참고** 여기서 `watchman`은 `Facebook`에서 만든 도구로, `Filesystem`의 변화를 감지합니다. (쉽게 말하면 내가 만든 프로젝트의 파일들에게 변화가 있는지 확인함)
{: .notice--info}

# 2. openjdk1.8 설치

`react-native` 는 `homebrew` 를 통해서 `jdk`를 다운받는것을 추천합니다.

```
$ brew tap AdoptOpenJDK/openjdk
$ brew cask install adoptopenjdk8
```

설치 확인

```
$ java -version
```

# 3. Android Studio 설치 및 SDK 설치

가장 번거로운 작업으로 안드로이드 스튜디오와 `SDK`를 설치합니다.

- <a href="https://developer.android.com/studio/index.html" target="_blank">https://developer.android.com/studio/index.html</a> 를 접속하여 다운로드 받습니다.

다운로드가 완료되면, 초기 세팅에서 `custom`을 누릅니다.

{% include gallery id="click-custom" caption="custom을 체크하고 Next 누르세요." %}

아래 그림과 같이, 총 4개의 항목을 체크하여 다운로드 받습니다.

- Android SDK
- Android SDK Platform
- Performance (Intel ® HAXM)
- Android Virtual Device

{% include gallery id="4-check" caption="총 4개 체크" %}

{% include gallery id="install-android" caption="다운로드 중" %}

## Android SDK 와 기타 도구 환경변수 설정

`$HOME/.bash_profile` 에 환경변수 추가

```bash
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

`echo` 명령어를 통해 환경변수가 설정됐는지 확인한다.

```
$ echo $PATH
```

# 4. React Native 프로젝트 생성 및 실행

본 글에서는 프로젝트 이름을 `TestApp` 이라고 하겠습니다.

## 생성

```
$ npx react-native init TestApp
```

또는

```
$ react-native init TestApp
```

## 실행

방금 만든 프로젝트를 실행한다.

```
$ cd TestApp
$ react-native run-android
```

또는

```
$ cd TestApp
$ npx react-native run-android
```

아래와 같이 에뮬레이터 및 프로젝트가 실행되면 성공.

{% include gallery id="start-project" caption="프로젝트 실행" %}

# Reference

- <a href="https://facebook.github.io/react-native/docs/getting-started" target="_blank">https://facebook.github.io/react-native/docs/getting-started</a>
