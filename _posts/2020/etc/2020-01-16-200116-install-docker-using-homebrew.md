---
title: "Homebrew를 이용하여 Docker(도커) 설치 (MAC OS)"
excerpt: "MAC OS 환경에서 도커(Docker) 설치하는 방법을 살펴본다. homebrew를 이용하여 설치하기 때문에 homebrew 설치가 우선적으로 필요하다."
search: true
categories:
  - Docker
tags:
  - Docker
  - Homebrew
last_modified_at: 2020-01-16T22:04:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: https://user-images.githubusercontent.com/26136312/72528110-9fd11980-38ad-11ea-8751-f4974a3042a5.png
install-docker:
  - url: https://user-images.githubusercontent.com/26136312/72528110-9fd11980-38ad-11ea-8751-f4974a3042a5.png
    image_path: https://user-images.githubusercontent.com/26136312/72528110-9fd11980-38ad-11ea-8751-f4974a3042a5.png
    alt: "Docker 설치 완료"
    title: "Docker 설치 완료"
jenkins-docker:
  - url: https://user-images.githubusercontent.com/26136312/72528603-d2c7dd00-38ae-11ea-9ecb-5f0ca52b9c06.png
    image_path: https://user-images.githubusercontent.com/26136312/72528603-d2c7dd00-38ae-11ea-9ecb-5f0ca52b9c06.png
    alt: "도커를 이용하여 Jenkins 띄우기"
    title: "도커를 이용하여 Jenkins 띄우기"
---

# 0. 환경 및 준비

- 운영체제: `MAC OS`
- 사전준비: `Homebrew`

본 글에서는 `Homebrew` 를 이용하여 `Docker` 를 설치한다. 따라서, `Homebrew` 의 설치가 우선적으로 필요하다.

```
$ brew --version
Homebrew 2.2.2
```

# 1. Docker 설치

brew 를 이용하여 Docker 를 설치할때는 brew cask install 명령어를 이용한다.

```
$ brew cask install docker
...
==> Moving App 'Docker.app' to '/Applications/Docker.app'.
🍺  docker was successfully installed!
```

<i class="fas fa-feather-alt"></i> **참고** 가끔 `Homebrew` 로 설치하다보면 `brew` 또는 `brew cask` 을 이용한다. 이 2개의 차이를 항상 궁금해 하다가 이번에 알게됐다. `brew` 는 패키지를 설치할 때 사용한다. `brew cask` 는 `GUI` 응용 어플리케이션을 설치할때 사용한다.
{: .notice--info}

위와 같이 정상적으로 설치되었다면, 응용프로그램에 `Docker` 가 설치되어있다.

{% include gallery id="install-docker" caption="Docker App 설치 완료" %}

설치된 앱을 실행한다.

# 2. Docker 실행

```
$ docker --version
Docker version 19.03.5, build 633a0ea
```

## 2.1. 테스트를 위한 이미지 가져오기

본 글에서는 `jenkins` 이미지를 이용하여 테스트 한다.

```
$ docker pull jenkins
```

```
$ docker images
REPOSITORY     TAG           IMAGE ID          CREATED           SIZE
jenkins        latest        cd14cecfdb3a      18 months ago     696MB
```

`jenkins` 이미지가 추가된것을 볼 수 있다.

## 2.2. docker를 이용하여 젠킨스 실행하기

```
$ docker run -p 8080:8080 -t jenkins
```

{% include gallery id="jenkins-docker" caption="Docker 로 jenkins 띄우기" %}

`localhost:8080` 으로 접속해보면 jenkins가 정상적으로 띄어진다.

# Reference

- <a href="https://docs.docker.com/docker-for-mac/" target="_blank">https://docs.docker.com/docker-for-mac/</a>
