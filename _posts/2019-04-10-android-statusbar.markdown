---
layout:     post
title:      "안드로이드 StatusBar 색상 변경"
subtitle:   "안드로이드 상태바 색상 변경 방법"
date:       2019-04-10 12:00:00
author:     "Inbum"
header-img: "img/post-bg-android-statusbar.png"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.5
    changefreq: 'monthly'
    lastmod: 2019-04-10 12:00:00
---

### StatusBar 색상 변경
최근 진행한 프로젝트에서, StatusBar 색상 변경에 대한 요구사항이 있었습니다. Black, White, Blue 3가지 타입의 스타일이며, 유틸함수를 만들어 적용한 내용을 정리하는 차원에서 포스팅 합니다.

안드로이드에서 StatusBar 색상을 변경 시킬 수 있는 방법은 두가지가 있습니다. 먼저 Themes를 이용한 방법과, Code를 이용한 방법 입니다. 저의 경우 3가지 타입의 스타일을 동적으로 적용해야 했으므로 Code를 이용한 방법으로 적용 하였습니다.

> StatusBar 색상 변경은 Android 5.0+(Lollipop API 21)부터 가능합니다.   

### 1. Theme를 이용한 방법
먼저, Theme를 이용하여 StatusBar 색상을 변경하는 방법 입니다.
AppCompat Theme를 사용하고 있다면, res/values/style.xml 파일 에 다음 내용을 추가합니다.
```
<?xml version="1.0" encoding="utf-8"?>
<style name="statusBarTheme" parent="Theme.AppCompat.Light">
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/myStatusBarColor</item>  // statusbar color 적용
    <!-- Other attributes -->
</style>
```

Material Theme를 사용하고자 하면, res/value-v21 폴더안에 style.xml 파일을 생성하고 아래 내용을 추가합니다.
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" parent="android:Theme.Material">
        <item name="android:colorPrimaryDark">@color/myStatusBarColor</item> // statusbar color 적용
    </style>
</resources>
```

### 2. Code를 이용한 방법
만약 프로그래밍 방식으로 StatusBar 색상을 변경하고자 하면, 아래의 순서대로 진행합니다.
#### Utility Class 생성
먼저, 어떤 Activity에서든 접근 가능한 utility class를 만듭니다. 적당한 페키지를 생성 한 뒤, Utils.java 파일을 생성하고 아래 코드를 입력합니다.
```java
public class Utils {
  public enum StatusBarColorType {
     BLACK_STATUS_BAR( R.color.black ),
     GREY_STATUS_BAR( R.color.grey_color_01 ),
     BLUE_STATUS_BAR( R.color.sky_blue_color );

     private int backgroundColorId;

     StatusBarColorType(int backgroundColorId){
        this.backgroundColorId = backgroundColorId;
     }

     public int getBackgroundColorId() {
        return backgroundColorId;
     }
  }

  public static void setStatusBarColor(Activity activity, StatusBarColorType colorType) {
     if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        activity.getWindow().setStatusBarColor(ContextCompat.getColor(activity, colorType.getBackgroundColorId()));
     }
  }
}
```
Enum 클래스 형을 기반으로 한 StatusBarColorType을 선언하여 3가지 타입의 StatusBarColor를 정의 하였습니다. 열거형 상수와 관련된 값을 생성자를 통해 연결시켜 사용 하였습니다.
그리고 API level 21에서 추가된 [setStatusBarColor](https://developer.android.com/reference/android/view/Window.html#setStatusBarColor(int))함수를 이용해 열거형 상수와 연결된 값으로 StatusBar 색상을 변경 하였습니다.

#### 사용방법  
아래와 같이 StatusBar 색상을 변경하고자 하는 Activity에서, activity와 정의해 놓은 열거형 상수 타입을 포함하여 호출합니다.
```java
public class MainActivity extends AppCompatActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    // call with activity, StatusBarColorType params
    Utils.setStatusBarColor(this, Utils.StatusBarColorType.BLUE_STATUS_BAR);
  }
}
```
누군가에게 도움이 되었기를 바랍니다.
