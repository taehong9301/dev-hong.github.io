---
title: "React Native Firebase admob 연동하기"
excerpt: "React Native 와 Google Firebase admob을 연동하여 화면에 광고를 띄우는 방법"
search: true
categories:
  - ReactNative
tags:
  - ReactNative
  - RN
  - React
  - Javascript
  - Firebase
last_modified_at: 2020-01-05T16:49:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser:
photo1:
  - url: https://user-images.githubusercontent.com/26136312/71777258-d58a1e80-2fe0-11ea-92e5-66d23e1e0f8a.png
    image_path: https://user-images.githubusercontent.com/26136312/71777258-d58a1e80-2fe0-11ea-92e5-66d23e1e0f8a.png
    alt: "Firebase 프로젝트 이름 정하기"
    title: "Firebase 프로젝트 이름 정하기"
photo2:
  - url: https://user-images.githubusercontent.com/26136312/71777259-d58a1e80-2fe0-11ea-9825-89f280f31f7f.png
    image_path: https://user-images.githubusercontent.com/26136312/71777259-d58a1e80-2fe0-11ea-9825-89f280f31f7f.png
    alt: "계속 클릭"
    title: "계속 클릭"
photo3:
  - url: https://user-images.githubusercontent.com/26136312/71777260-d622b500-2fe0-11ea-85aa-b82efbbfeffd.png
    image_path: https://user-images.githubusercontent.com/26136312/71777260-d622b500-2fe0-11ea-85aa-b82efbbfeffd.png
    alt: "Firebase 프로젝트 생성 완료"
    title: "Firebase 프로젝트 생성 완료"
photo4:
  - url: https://user-images.githubusercontent.com/26136312/71777261-d622b500-2fe0-11ea-8010-ab6e83fd2700.png
    image_path: https://user-images.githubusercontent.com/26136312/71777261-d622b500-2fe0-11ea-8010-ab6e83fd2700.png
    alt: "콘솔에 안드로이드 앱 추가"
    title: "콘솔에 안드로이드 앱 추가"
photo5:
  - url: https://user-images.githubusercontent.com/26136312/71777262-d622b500-2fe0-11ea-859d-1e19af8e3dc5.png
    image_path: https://user-images.githubusercontent.com/26136312/71777262-d622b500-2fe0-11ea-859d-1e19af8e3dc5.png
    alt: "프로젝트에 google-services.json 파일 추가"
    title: "프로젝트에 google-services.json 파일 추가"
photo6:
  - url: https://user-images.githubusercontent.com/26136312/71777263-d6bb4b80-2fe0-11ea-9df3-ae2ef2b6aaaa.png
    image_path: https://user-images.githubusercontent.com/26136312/71777263-d6bb4b80-2fe0-11ea-9df3-ae2ef2b6aaaa.png
    alt: "스킵"
    title: "스킵"
photo7:
  - url: https://user-images.githubusercontent.com/26136312/71777323-7b3d8d80-2fe1-11ea-9fb7-ce561ac78857.png
    image_path: https://user-images.githubusercontent.com/26136312/71777323-7b3d8d80-2fe1-11ea-9fb7-ce561ac78857.png
    alt: "결과 확인"
    title: "결과 확인"
---

# 0. 환경

- 운영체제: MAC OS
- `react-native` 모듈이 설치되어 있고, 프로젝트가 생성되어 있다는 가정하에 본 글을 작성했습니다.
  - 프로젝트 앱 생성 명령어: `$ react-native init 앱이름`

# 1. Firebase 프로젝트 생성

<a href="https://console.firebase.google.com/" target="_blank">https://console.firebase.google.com/</a>

{% include gallery id="photo1" caption="Firebase 프로젝트 이름 정하기" %}

{% include gallery id="photo2" caption="계속 클릭" %}

{% include gallery id="photo3" caption="Firebase 프로젝트 생성 완료" %}

# 2. gradle 파일 수정

{% include gallery id="photo4" caption="Firebase 콘솔에 안드로이드 앱 추가" %}

{% include gallery id="photo5" caption="프로젝트에 google-services.json 파일 추가" %}

`app` 수준의 `build.gradle`에서 `defaultDonfig > applicationId` 에 명시된 `com.admobtestapp` 을 넣습니다.

```gradle
...
defaultConfig {
    applicationId "com.admobtestapp"
    minSdkVersion rootProject.ext.minSdkVersion
    targetSdkVersion rootProject.ext.targetSdkVersion
    versionCode 1
    versionName "1.0"
}
...
```

프로젝트 수준의 `build.gradle`(`PROJECT/build.gradle`) 의 내용을 추가합니다.

```gradle
buildscript {
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository // ** 추가 **
  }
  dependencies {
    ...
    // Add this line
    classpath 'com.google.gms:google-services:4.3.2' // ** 추가 **
  }
}

allprojects {
  ...
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository // ** 추가 **
    ...
  }
}
```

앱 수준의 `build.gradle` (`PROJECT/APP_MODULE/build.gradle`) 의 내용을 추가 합니다.

```gradle
...
apply plugin: 'com.android.application' // ** 추가 **

dependencies {
  // add the Firebase SDK for Google Analytics
  implementation 'com.google.firebase:firebase-analytics:17.2.0' // ** 추가 **
  // add SDKs for any other desired Firebase products
  // https://firebase.google.com/docs/android/setup#available-libraries
}
...
// Add to the bottom of the file
apply plugin: 'com.google.gms.google-services' // ** 파일의 맨 마지막에 추가 **
```

{% include gallery id="photo6" caption="스킵" %}

# 3. AndroidManifest.xml 수정

`AndroidManifest.xml` 파일에 자신의 `admob ID`를 추가합니다.

```xml
<manifest>
    <application>
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="[ADMOB_APP_ID]"/> <!-- 예를 들어서 App ID: ca-app-pub-3940256099942544~3347511713 -->
    </application>
</manifest>
```

# 4. Firebase admob 과 관련된 모듈 설치

`@react-native-firebase/admob` 을 모듈을 설치 받습니다. (`npm` 또는 `yarn` 이용)

- 본 글에서는 `yarn` 을 이용하여 모듈 설치

`@react-native-firbase/admob` 은 `@react-native-firbase/app` 를 의존하기 때문에 둘다 설치해줍니다.

```
$ yarn add @react-native-firebase/app
$ yarn add @react-native-firebase/admob
```

# 5. firebase.json 설정

`RN` 프로젝트 상위에 `firebase.json` 파일을 생성하여 앱 `ID` 를 작성합니다.

- 예: `ca-app-pub-3940256099942544~3347511713`

```json
{
  "react-native": {
    "admob_android_app_id": "ca-app-pub-xxxxxxxx~xxxxxxxx",
    "admob_ios_app_id": "ca-app-pub-xxxxxxxx~xxxxxxxx"
  }
}
```

# 6. 결과 확인

마지막으로 admob banner가 정상적으로 띄어지는지 확인합니다.

- `App.js`

```javascript
import React from "react";
import { SafeAreaView, Text, StatusBar } from "react-native";

import { BannerAd, BannerAdSize, TestIds } from "@react-native-firebase/admob";

const App = () => {
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <SafeAreaView>
        <Text>Admob Test</Text>
        <BannerAd
          unitId={TestIds.BANNER}
          size={BannerAdSize.FULL_BANNER}
          requestOptions={{
            requestNonPersonalizedAdsOnly: true
          }}
          onAdLoaded={() => {
            console.log("Advert loaded");
          }}
          onAdFailedToLoad={error => {
            console.error("Advert failed to load: ", error);
          }}
        />
      </SafeAreaView>
    </>
  );
};

export default App;
```

{% include gallery id="photo7" caption="결과 확인" %}

# Reference

- <a href="https://invertase.io/oss/react-native-firebase/v6/admob/quick-start" target="_blank">https://invertase.io/oss/react-native-firebase/v6/admob/quick-start</a>
