---
title: "개발을 도와주는 Prettier 설치"
excerpt: "효율적으로 개발을 할 수 있게 도와주는, Prettier 설치하는 방법에 대해 살펴봅니다. 개발환경은 VSC(Visual Studio Code)입니다."
search: true
categories: 
  - Programming
tags: 
  - Prettier
last_modified_at: 2019-03-26T23:04:00+09:00
toc: true
toc_sticky: true
comments: true
---
## 1. Prettier란

`Prettier`는 코드를 작성했을 때, 보기 좋게 코드를 재배열해주는 플러그인입니다. 사실 없어도 개발하는데 문제가 생기는 것은 아니지만, 가독성도 높아지고 개발을 효율적으로 할 수 있게 도와줍니다. 특히 협업으로 할 때는 더욱 느끼실겁니다. 예전 속담으로 이런말도 있죠...

> 보기 좋은 떡이 먹기도 좋다!  

옛말도 있듯이(?) 사용해 보시는걸 추천합니다.  


## 2. Install

그래서 본격적으로 어떻게 설치하느냐! 글쓴이는 VSC(`Visual Studio Code`)를 사용하기 때문에, `VSC` 환경에서 설치하는 방법에 대해 소개하겠습니다. (기회가 된다면 다른 편집기에서 설치하는 방법도 소개하겠습니다. 이건 추후에...)  

일단 [설치 가이드 사이트](https://prettier.io/docs/en/install.html "move")를 통해서 설치를 할 수 있지만, ~~영어로 되어있기 때문에~~ 눈에 안들어올 수 도 있습니다.  

[VSC MarketPlace](https://marketplace.visualstudio.com/items?itemName=remimarsal.prettier-now "move")에서도 설치를 할 수 있기 때문에, 해당 사이트에서 설치 하도록 하겠습니다. 단, VSC는 컴퓨터에 설치가 되어 있어야 합니다.  

![이미지1](https://user-images.githubusercontent.com/26136312/55007290-814e1900-5022-11e9-8226-af3d5fbe9359.PNG)  

`Install`을 누르고 설치를 진행하면 끝!  


## 3. 적용 후 설정

여기서 끝이 아니라, 설정을 해야하는 부분이 있습니다. 저장을 하면 바로 재배열이 되도록 설정하는건데, 정말 편합니다!  

일단, VSC에서 설정(좌측 하단에 톱니바퀴 모양)을 누릅니다.  

`settings`를 누릅니다.  

![이미지2](https://user-images.githubusercontent.com/26136312/55007350-9fb41480-5022-11e9-91b5-69f24e70043f.png)  

검색창이 나오는데, 검색창에 `prettier`라고 작성합니다.  

![이미지3](https://user-images.githubusercontent.com/26136312/55007359-a2166e80-5022-11e9-859b-df992773a27a.png)  

편집기로 수정을 해야하는데, 아래 내용을 입력해주면 됩니다. 참고로 [해당 페이지](https://github.com/prettier/prettier-vscode "move")에서 자세하게 사용법 및 기타 설정하는 방법에 대해서 나와있습니다.  

```javascript
// 기본값으로 false로 설정되어 있습니다.
"editor.formatOnSave": true,

// 만약 언어마다 설정하고 싶을 때는 아래와 같이 합니다.
//"[javascript]": {
//    "editor.formatOnSave": true
//}
```

## Reference

[https://prettier.io/docs/en/install.html](https://prettier.io/docs/en/install.html "move")  

[https://github.com/prettier/prettier-vscode](https://github.com/prettier/prettier-vscode "move")  