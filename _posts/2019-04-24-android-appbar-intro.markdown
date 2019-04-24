---
layout:     post
title:      "안드로이드 AppBar 설정"
subtitle:   "v7 appcompat support library's Toolbar를 AppBar로 사용하는 방법"
date:       2019-04-24 18:00:00
author:     "Inbum"
header-img: "img/post-bg-android-appbar.png"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.7
    changefreq: 'monthly'
    lastmod: 2019-04-24 18:00:00
---

### 안드로이드 AppBar
저의 경우 신규 안드로이드 프로젝트 진행시 주요 과정을 몇몇 단계별로 나눠 작업을 진행합니다. 대략적으로, 가장 먼저 프로젝트를 설정하고, ui 구현, 비즈니스 로직 개발, 테스트 및 배포의 과정을 진행 합니다.
안드로이드 AppBar는 위 개발 과정 중 ui구현에서 가장 먼저 직면하게 되는 내용 입니다. AppBar의 style 변경, Title text style 변경, 네비게이션 버튼, action button, overflow menu, action view 등 다양한 ui 구현 이슈들이 존재합니다.
또한, 사용자에게 일관된 경험을 제공하기 위해 AppBar는 가장 중요한 디자인 요소중 하나이며, 이에 대한 정확한 이해는 필수라고 할 수 있습니다.
이번 포스트에서 AppBar에 관한 내용들을 정리하겠습니다.

> 이번 포스트에서는 v7 appcompat support library의 Toolbar 위젯을 사용하여 AppBar를 구현하도록 하겠습니다. Toolbar를 사용하여 구현할 경우 ActionBar에 비해 더욱 유현합니다. 수많은 종류의 기기에서 일관된 앱 동작을 보장할 수 있으며, 더욱 쉽게 색상, 위치, 크기등을 변경할 수 있고 label, logos, navigation icon 이나 기타 다른 view들을 add할 수 있습니다. ActionBar의 경우 Object를 상속받고, Toolbar는 ViewGroup을 상속 받기 때문입니다.

### 1. Toolbar를 AppBar로 사용하기 위한 기본 설정  
#### 1.1 v7 AppCompat 지원 라이브러리 프로젝트 추가 확인
안드로이드 스튜디오에서 새로운 프로젝트 생성 시, appcompat-v7 지원 라이브러리가 자동으로 추가 되지만, 정상적으로 적용되어 있는지 확인해 보겠습니다.
1. 앱 모듈의 build.gradle 파일을 엽니다.
2. dependencies 섹션에 지원 라이브러리 확인
```
dependencies {
    ...
    implementation 'com.android.support:appcompat-v7:28.0.0'
    ...
}
```

#### 1.2 Activity가 AppCompatActivity를 확장하는지 확인
이 부분도 마찬가지로 신규 프로젝트 생성 후 Empty Activity 생성 시 자동으로 확장되어 있지만, Toolbar를 AppBar로 사용하기 위해 필요한 부분이기 때문에 확인합니다.
```java
public class MainActivity extends AppCompatActivity {
  // ...
}
```

#### 1.3 NoActionBar Theme 설정
application의 theme를 AppCompat의 NoActionBar 테마 중 하나로 사용하게 변경합니다. 신규 프로젝트 생성 시 자동으로 설정 되어 있는 styles.xml의 AppTheme 의 parent 테마를 다음과 같이 수정 합니다. 아래 코드를 적용하면 AppBar가 사라지게 됩니다.
```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <!--
    ...
    -->
</style>
```

#### 1.4 activity 레이아웃에 Toolbar 추가
activity_main.xml 파일을 다음과 같이 수정하였습니다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <android.support.v7.widget.Toolbar
        android:id="@+id/my_toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary"
        android:elevation="4dp"
        android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:layout_gravity="center"/>

</LinearLayout>
```

#### 1.5 setSupportActionBar() 호출
마지막으로 Activity의 onCreate() 메소드에서, setSupportActionBar() 메소드를 호출하여 툴바를 Activity의 AppBar로 설정합니다.
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Toolbar myToolbar = (Toolbar) findViewById(R.id.my_toolbar);
        setSupportActionBar(myToolbar);
    }
}
```

#### 1.6 실행결과

![실행결과](/img/post-cp1-android-appbar.png)


이제 Toolbar를 AppBar로 사용하기 위한 기본적인 설정이 끝났습니다. 추가로, 스타일 변경 및 v7 AppCompat support library ActionBar 클래스에서 제공하는 다양한 유틸리티 메소드를 활용하는 방법에 대해 작성 하겠습니다. 곧...
