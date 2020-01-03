---
title: "인텔리제이 플러그인 수동으로 적용하기"
excerpt: "인텔리제이에서 플러그인을 수동으로 적용하는 방법에 대해 살펴봅니다. 회사 내부망 같은 경우는 인터넷이 안되기 때문에 플러그인을 적용 못하는 경우가 있을 수 있는데, 수동으로 적용하는 방법이 있습니다."
search: true
categories: 
  - Programming
tags: 
  - Programming
  - Plugin
  - Intellij
last_modified_at: 2019-04-25T21:17:00+09:00
toc: true
toc_sticky: true
comments: true
header:
    teaser: /assets/images/thumbnail/2019/190425-intellij-manual-plugin-560x315.png
---

## 인텔리제이 수동 플러그인

인터넷이 안되는 환경(회사 업무 내부망 같은 경우)에서는 플러그인을 수동으로 깔아야하는 경우가 있습니다. 본 글에서는 플러그인을 수동으로 적용하는 방법에 대해 살펴봅니다.

### Plugin repository

인텔리제이를 좀 더 효율적으로 사용하기 위해 여러가지 플러그인이 존재합니다. <a href="https://plugins.jetbrains.com/idea" target="_blank">https://plugins.jetbrains.com/idea</a>에서 플러그인을 다운 받을 수 있습니다.  


### Markdown Plugin 수동 적용

본 글에서는 `Markdown`을 작성하기 편하게 해주는 `Markdown Navigator` (<a href="https://plugins.jetbrains.com/plugin/7896-markdown-navigator" target="_blank">https://plugins.jetbrains.com/plugin/7896-markdown-navigator</a>)을 적용하려 합니다.  

다운을 받게 되면 압축파일(`zip`)을 다운받게 됩니다. (압축파일을 해제할 필요없습니다.)  

인텔리제이 `IDEA`를 실행합니다. 우측 하단에 **톱니바퀴** 모양을 누르고 `Plugin`을 선택합니다.  
![플러그인클릭](https://user-images.githubusercontent.com/26136312/56734874-f8e8a280-679e-11e9-9126-d0c149d1d4cf.png)  


우측상단에 메뉴를 누르고 `Install plugin from the disk ...`를 클릭합니다.  
![인스톨디스크플러그인](https://user-images.githubusercontent.com/26136312/56734872-f8500c00-679e-11e9-8ca8-e837fbc0058a.png)  


파일 불러오는 창이 나오는데, 처음에 홈페이지에서 다운받은 플러그인을 경로를 찾아서 클릭한다음 `Okay`를 누릅니다.  
![경로지정](https://user-images.githubusercontent.com/26136312/56734870-f8500c00-679e-11e9-93a4-3319170158c6.png)  


적용이 완료되면, 인텔리제이 `IDEA`를 재시작해줘야 합니다.  
![설치완료](https://user-images.githubusercontent.com/26136312/56734871-f8500c00-679e-11e9-81fc-6cb844b3432e.png)  


#### 결과 확인

마지막으로, 수동으로 적용한 플러그인이 제대로 동작하는지 확인해봅니다.  

![결과](https://user-images.githubusercontent.com/26136312/56734868-f7b77580-679e-11e9-96ee-bc2b698b9996.png)  


<i class="fas fa-feather-alt"></i> **참고** 본 글은 `Window10` 환경, 인텔리제이 커뮤니티 버전 `IDEA`에서 작성되었습니다.
{: .notice--info}