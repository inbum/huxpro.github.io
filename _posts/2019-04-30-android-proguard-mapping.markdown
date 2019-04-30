---
layout:     post
title:      "안드로이드 ProGuard 난독 해제 파일 등록"
subtitle:   "정확한 오류 스택 추적을 위한 mapping파일 업로드"
date:       2019-04-30 14:20:00
author:     "Inbum"
header-img: "img/post-cp0-android-proguard.png"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.7
    changefreq: 'monthly'
    lastmod: 2019-04-30 14:20:00
---

### ProGuard 난독화
안드로이드 앱을 출시하기 전, 보통 ProGuard를 이용하여 코드 및 리소스 축소, 코드 난독화 처리를 합니다.
난독 처리된 코드는 APK의 리버스 엔지니어링을 어렵게 만들고, 보안에 민감한 앱의 경우 유용하게 사용할 수 있습니다.
코드 및 리소스 축소 방법은 [링크](https://developer.android.com/studio/build/shrink-code)에 자세히 설명되어 있습니다.

이번 포스트에서는,
난독 처리된 APK파일의 ANR및 비정상 종료 로그를 Google Play Console에서 자세히 보는 방법에 대해 정리해 보겠습니다.

### 1. ProGuard 난독 해제
'Google Play Console > Android vitals > ANR 및 비정상 종료' 메뉴를 이용하여 사용자의 Android 기기에서 수집된 모든 ANR 및 비정상 종료 로그를 조회할 수 있습니다.
그런데, ProGuard를 이용해 난독화한 APK파일의 경우, 다음과 같이 **난독화 된 소스코드의 위치가 표시되어** 실제 소스코드 위치를 파악하기가 쉽지 않습니다.
![ProGuard](/img/post-cp0-android-proguard.png)

### 2. 난독 해제 파일
실제 소스코드의 패키지, 클래스, 메소드명을 파악하기 위해서는 **난독 해제 파일을 업로드** 해야 합니다.
난독 해제 파일은 **<module-name>/build/outputs/mapping/release/** 경로에 저장되며 mapping.txt 파일명으로 저장되어 있습니다.
그 외 dump.txt, seeds.txt, usage.txt등 ProGuard를 이용하여 빌드 시 생성된 파일들도 함께 존재합니다.

### 3. mapping.txt 업로드
'Google Play Console > Android vitals > 난독 해제 파일' 메뉴를 통해 해당 버전의 '업로드' 버튼을 클릭하여 **난독 해제 파일을 업로드** 합니다.
![ProGuard Menu](/img/post-cp1-android-proguard.png)
![ProGuard Upload](/img/post-cp2-android-proguard.png)

### 4. ANR 및 비정상 종료 로그
업로드 후 보고되는 ANR및 비정상 종료 로그는 정확한 패키지명, 클래스명, 메소드를 표시하고 실제 소스코드 위치까지 정확시 표시되는 것을 확인할 수 있습니다.
어려운 부분은 없지만, 앱 배포시 놓칠 수 있는 부분이니, **다시한번 체크하는** 습관을 길러야겠습니다. :)
![ProGuard Mapping](/img/post-cp3-android-proguard.png)

[https://developer.android.com/studio/build/shrink-code](https://developer.android.com/studio/build/shrink-code#shrink-code){:target="_blank"}
