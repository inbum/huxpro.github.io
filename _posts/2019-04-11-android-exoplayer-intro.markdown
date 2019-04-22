---
layout:     post
title:      "안드로이드 ExoPlayer2 소개"
subtitle:   "ExoPlayer란?"
date:       2019-04-11 12:33:00
author:     "Inbum"
header-img: "img/post-bg-android-exoplayer-intro.jpg"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.5
    changefreq: 'monthly'
    lastmod: 2019-04-11 12:33:00
---

### ExoPlayer?
현재 근무하고 있는 회사에서 주로 하고 있는 업무는 안드로이드 미디어 스트리밍 서비스 앱 개발입니다. 기존에 제작되어 서비스 하고 있는 어플리케이션에서는 3rd party 라이브러리를 사용하고 있는데, 속도, 안정성, 성능등의 다양한 이슈 때문에 ExoPlayer 도입을 고려하고 있는 상황입니다. 이번 포스트에서는 ExoPlayer에 대해 소개 하고자 합니다.

> ExoPlayer는 안드로이드용 오픈소스 미디어 재생 라이브러리 입니다. 원래는, YouTube나 Google Play Movies와 같은 구글의 Android Application을 위해 개발 되었으나 2014년 open source로 공개 되었습니다. ExoPlayer의 표준 오디오 및 비디오 구성 요소는 Android 4.1(API Level 16)에서 출시된 Android's MediaCodec API를 기반으로 합니다. [DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)콘텐츠를 재생하도록 설계되었으며, 출시 이후 다양한 기술과 formats을 지원합니다. 지원 포멧에 대한 내용은 [링크](https://exoplayer.dev/supported-formats.html)에서 확인 가능합니다.

### ExoPlayer2를 활용하여 동영상 플레이어 앱 만들기
바로 간단한 앱을 만들어 보겠습니다. 프리롤 광고, 미드롤 광고가 포함된 스트리밍 재생 동영상 플레이어 입니다.
아래 내용은 [ExoPlayer Demo App](https://github.com/google/ExoPlayer)의 내용을 참고하여 작성 하였습니다.
#### 1-1. repositories 및 ExoPlayer module dependencies 추가
새로운 프로젝트를 생성 후 루트 폴더에 있는 build.gradle 파일에 Google 및 JCenter저장소가 포함되어 있는지 확인합니다.
```
repositories {
    google()
    jcenter()
}
```

다음으로 앱 모듈의 build.gradle 파일에 ExoPlayer module dependencies를 추가합니다.
```
dependencies {
    implementation 'com.google.android.exoplayer:exoplayer:2.X.X'
}
```

여기서는 전체 라이브러리를 추가하는 대신, 필요한 라이브러리들만 추가하겠습니다.
```
dependencies {
    implementation 'com.google.android.exoplayer:exoplayer-core:2.9.6'
    implementation 'com.google.android.exoplayer:exoplayer-ui:2.9.6'
    implementation 'com.google.android.exoplayer:extension-ima:2.9.6'
}
```
* exoplayer-core: Core functionality (required).
* exoplayer-ui: UI components and resources for use with ExoPlayer.
* exoplayer-ima: Insert ads alongside content.
exoplayer-core와 exoplayer-ui는 개별적인 라이브러리 모듈이고, 추가 기능을 제공하기 위해 외부 라이브러리에 의존하는 확장 모듈들이 있습니다. exoplayer-ima 확장모듈도 그 중 하나이며, VAST규격의 광고를 요청하고 플레이어 타임라인에 추가하는데 사용 됩니다. 추가적인 외부 라이브러리 확장모듈에 대한 설명은 [링크](https://github.com/google/ExoPlayer/tree/release-v2/extensions)를 참조하시기 바랍니다.

#### 1-2. 자바8 지원 켜기
ExoPlayer를 사용하기 위해서는 자바8지원 기능을 on 시켜야 합니다. 앱 모듈의 build.gradle 파일 내 android section 부분에 아래 내용을 추가 합니다.
```
compileOptions {
  targetCompatibility JavaVersion.VERSION_1_8
}
```
#### 2-1. PlayerView 생성
activity_main.xml 파일에 PlayerView를 추가하고 id를 다음과 같이 player_view 라고 부여 합니다.
```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <com.google.android.exoplayer2.ui.PlayerView
      android:id="@+id/player_view"
      android:layout_width="match_parent"
      android:layout_height="match_parent" />
</FrameLayout>
```
#### 2.2 Player 초기화  
* onCrate 메소드에서 Player 뷰에 접근하기 위해 뷰를 찾아오고, 구글 샘플 소스에 포함된, AD_TAG_URI를 이용해 ImaAdsLoader를 초기화 합니다.
* onStart 메소드에서는 ExoPlayerFactory.newSimpleInstance method를 이용해 플레이어를 초기화 하고, 광고가 포함된 미디어소스를 생성하여 플레이어에 적용 합니다. 그리고 setPlayWhenReady(true) 메소드를 호출하여 Player.getPlaybackState() == Player.STATE_READY 상태가 되면 영상이 자동으로 재생되게 설정 합니다.
* onStop, onDestroy 메소드에서 ExoPlayer.release를 호출하여 다른 어플리케이션에서 사용할 수 있게 video decoder와 같은 더이상 필요 없는 리소스들을 릴리즈 합니다.

```java
public class MainActivity extends Activity {

  private PlayerView playerView;
  private SimpleExoPlayer player;
  private ImaAdsLoader adsLoader;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    playerView = findViewById(R.id.player_view);
    adsLoader = new ImaAdsLoader(this, AD_TAG_URI);
  }

  @Override
  protected void onStart() {
    super.onStart();

    player = ExoPlayerFactory.newSimpleInstance(this, new DefaultTrackSelector());
    playerView.setPlayer(player);
    adsLoader.setPlayer(player);

    DataSource.Factory dataSourceFactory = new DefaultDataSourceFactory(
        this,
        Util.getUserAgent(this, getString(R.string.app_name)));
    ExtractorMediaSource mediaSource = new ExtractorMediaSource.Factory(dataSourceFactory)
        .createMediaSource(MP4_URI);
    MediaSource adsMediaSource =
        new AdsMediaSource(mediaSource, dataSourceFactory, adsLoader, playerView);
    player.prepare(adsMediaSource);

    player.setPlayWhenReady(true);
  }

  @Override
  protected void onStop() {
    adsLoader.setPlayer(null);
    playerView.setPlayer(null);
    player.release();
    player = null;

    super.onStop();
  }

  @Override
  protected void onDestroy() {
    adsLoader.release();

    super.onDestroy();
  }

}
```

#### 3. 실행결과

![실행결과](/img/post-cp1-android-exoplayer.png)

---

너무나 쉽고 빠르게 VAST규격의 광고가 포함된 미디어를 재생하는 플레이어를 만들어 보았습니다. 추후 파일 탐색, 라이브 스트림 URL추가 등 확장 기능들을 생각해 보고 구현하여 조금 더 진화 된 미디어 플레이어를 구현해 보도록 하겠습니다. 감사합니다. :)

---

참고자료
* [Source](https://github.com/google/ExoPlayer/tree/io18/video-app){:target="_blank"}
* [Google I/O '18](https://www.youtube.com/watch?v=svdq1BWl4r8){:target="_blank"}
