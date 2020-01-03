---
title: "리액트(ReactJS) 프로젝트 생성하기"
excerpt: "본 글은 리액트 프로젝트를 만드는 방법에 대해 다룹니다. 사전에 Node.js, yarn이 설치가 되어 있어야 합니다."
search: true
categories: 
  - Programming
  - ReactJS
tags: 
  - Programming
  - yarn
  - ReactJS
last_modified_at: 2019-05-01T00:30:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190501-create-react-project.png
gallery:
  - url: https://user-images.githubusercontent.com/26136312/56974835-9599ae80-6baa-11e9-843b-dbf552078964.PNG
    image_path: https://user-images.githubusercontent.com/26136312/56974835-9599ae80-6baa-11e9-843b-dbf552078964.PNG
    alt: "React project"
    title: "React project"
---

<i class="fas fa-feather-alt"></i> **yarn 설치** yarn 설치하는 방법은 아래 링크를 통해서 진행해주시길 바랍니다. 본 글에서는 생성하는 방법에 대해서만 다룹니다.  
<a href="/programming/190430-install-yarn/" target="_blank">/programming/190430-install-yarn/</a>
{: .notice--info}

## create-react-app 패키지 설치

리액트를 프로젝트를 만드는 방법은 2가지가 있습니다. 첫번째, webpack, 바벨 등을 일일이 설정해서 만드는 방법이 있습니다. (이 방법은 꽤나 복잡합니다.) 두번째, `create-react-app`을 이용하여 만드는 방법입니다.  

create-react-app은 리액트 앱을 만들어주는 도구로 복잡하게 설정하는 부분을 알아서 해줍니다. 그래서 손쉽게 리액트 프로젝트를 만들 수 있다는 장점이 있습니다. 본 글에서는 `create-react-app`을 이용하여 프로젝트를 만들어 보도록 하겠습니다.  

### 본격적인 설치

**설치를 진행하기 전에** yarn을 이용하여 패키지를 다운 받을때는 2가지 방법이 있습니다. 첫째, 로컬(local)에 설치하는 방법. 두번째, 전역(global)에 설치하는 방법입니다.  
둘의 차이는 해당 디렉토리에서만 사용하는가?(local) 아니면 모든 디렉토리에서 사용하는가?(global) 입니다. `creat-react-app`은 어디서든 사용할 수 있어야 하기 때문에 `global`로 설치를 해야합니다.
{: .notice--warning}

아래 명령어를 입력하여 설치를 진행합니다. 반드시 옵션에 `global add`를 넣어서 설치하시길 바랍니다.  

```bash
$ yarn global add create-react-app
```

또는

```bash
$ yarn install -g create-react-app
```

설치를 완료했으면, 리액트 프로젝트를 만들기위한 준비는 100% 모두 끝났습니다.  


## 리액트 프로젝트 생성

위에서 설치한 `create-react-app` 도구를 이용하여 프로젝트를 생성합니다.  

```bash
$ create-react-app [프로젝트 이름]
```

예를들어,

```bash
$ create-react-app hello-react
```

위 명령어를 입력하면 프로젝트를 생성하기 시작합니다. 완료되는데, 시간이 조금 걸립니다.

```
Creating a new React app in C:\dev\react\hello-react.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts...

yarn add v1.15.2

# 생략...

Initialized a git repository.

Success! Created hello-react at C:\dev\react\hello-react
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd hello-react
  yarn start

Happy hacking!
```

위와 같이 프로젝트가 생성이되면, 생성한 프로젝트를 실행해봅니다.  


## 리액트 프로젝트 실행

아래 명령어를 이용하여 프로젝트를 실행합니다. 실행을 할때는 프로젝트가 있는 디렉토리로 이동하여 `yarn start`로 실행을 합니다.  

```bash
$ cd hello-react
```

```bash
$ yarn start
```

<i class="fas fa-feather-alt"></i> **Note** 기본적으로 리액트 서버를 실행하면 포트 `3000`번으로 실행이 됩니다. 그리고, 리액트의 장점(?)으로 파일이 수정될때마다 프로젝트를 다시 빌드하고 웹 페이지를 리로딩하기 때문에 따로 리로딩을 할 필요가 없습니다.
{: .notice--info}

## 결과

{% include gallery caption="리액트 실행화면." %}

Hello React!