---
layout:     post
title:      "안드로이드 SSLHandshakeException 이슈"
subtitle:   "Android 4.4이하 버전에서 TLS 1.2 사용"
date:       2019-04-25 17:30:00
author:     "Inbum"
header-img: "img/about-bg-lib.png"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.7
    changefreq: 'monthly'
    lastmod: 2019-04-25 18:00:00
---

### SSLHandshakeException 발생!
진행하고 있는 프로젝트에서 서버 이관 작업이 발생하였습니다. 변경된 서버로 테스트 하는 중, Android 4.4이하 버전에서 SSLHandshakeException이 발생하는 이슈가 발생합니다.

### 1. 구글링
구글링 결과 stackoverflow에 [링크](https://stackoverflow.com/a/26586324)와 같은 답변을 찾았습니다.
구글 개발자 사이트에 명시된 SSLEngine 버전별 기본 설정을 보면, API 20미만은 TLSv1 Protocol만 지원하는 걸로 명시 되어 있습니다.

### 2. SSL Server Test
SSL LABS를 이용해 바로 확인 해 보았습니다. 기존 서버의 SSL Protocols 설정과 이관할 서버의 SSL Protocols의 설정을 비교한 결과, 예상대로 TLS 1.0 설정이 No로 되어 있습니다.
![기존서버](/img/post-cp1-android-sslexception.png)
![이관서버](/img/post-cp2-android-sslexception.png)

> [SSL LABS](https://www.ssllabs.com/)는 SSL과 관련된 자료, 툴등을 제공해 주는 사이트 입니다. 서버 취약점 분석 및 브라우저 취약점 분석, SSL취약점 통계 등 유용한 기능들을 제공합니다.

### 3. 해결 방안
- 가장 쉽고 빠르게 해결할 수 있는 방법은, 서버 설정을 변경하는 방법 입니다. TLSv1 프로토콜 지원하는 것으로... 하지만, 보안상의 이슈로 TLS v1/v1.1을 점점 지원하지 않는 추세에, 이 방법은 좋은 해결책은 아닌 듯 합니다.
- 그렇다면, 안드로이드 API 20 이하에서 TLS 1.2를 활성화 시켜야 하는데, 아주 간단한 해결 방법이 있습니다. 바로, security provider를 업데이트 하는 방법 입니다.
앱 실행시 아래 소스 코드를 추가 해 줍니다.
```java
try {
    ProviderInstaller.installIfNeeded(getContext());
} catch (GooglePlayServicesRepairableException e) {
    // Indicates that Google Play services is out of date, disabled, etc.
    // Prompt the user to install/update/enable Google Play services.
    GooglePlayServicesUtil.showErrorNotification(
        e.getConnectionStatusCode(), getContext());
    return;
} catch (GooglePlayServicesNotAvailableException e) {
    // Indicates a non-recoverable error; the ProviderInstaller is not able
    // to install an up-to-date Provider.
    return;
}
```
너무나 간단히 해결되었습니다. 동기, 비동기 업데이트에 대한 자세한 내용은 아래 링크로 대신합니다. 감사합니다. :)

[https://developer.android.com/training/articles/security-gms-provider](https://developer.android.com/training/articles/security-gms-provider){:target="_blank"}
