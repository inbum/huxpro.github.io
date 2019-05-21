---
layout:     post
title:      "안드로이드 BottomNavigationView ripple 효과"
subtitle:   "BottomNavigationView의 itemBackground 속성을 지정하면 ripple효과가 없어진다???"
date:       2019-05-17 18:00:00
author:     "Inbum"
header-img: "img/post-bg-android-bottomnavigationview.png"
header-mask: 0.3
category:   android
catalog:    true
multilingual: false
tags:
    - android
sitemap:
    priority: 0.7
    changefreq: 'monthly'
    lastmod: 2019-05-21 18:00:00
---

### 안드로이드 BottomNavigationView
App에서 Bottom Navigation Bar구현은 사용자가 한번의 탭동작으로 최상위 화면의 전환 및 탐색시 사용할 수 있는 유용한 방법입니다. 이를 구현할 수 있는 방법은 다양하겠지만, Android의 표준 구현 방식은 [BottomNavigationView](https://developer.android.com/reference/android/support/design/widget/BottomNavigationView) 위젯을 이용하는 방법 입니다. BottomNavigationView는 2017년 9월에 발표된 Revision 26.1.0 에 포함되었으며 메뉴 리소스 파일을 이용하거나 프로그램방식으로 메뉴를 추가하여 하단 텝 메뉴의 제목 및 아이콘을 쉽게 표시할 수 있습니다.
![BottomNavigationView](/img/post-cp0-android-bottomnavigationview.jpg)

### 메뉴 구분선 표시
BottomNavigationView로 구현된 App에서 메뉴 사이에 구분선을 넣어달라는 디자인 요구사항이 생겼습니다.
![메뉴 구분선 디자인](/img/post-cp1-android-bottomnavigationview.jpg)
BottomNavigationView의 배경을 구분선이 포함된 배경으로 변경하면 간단히 처리할수는 있겠지만, 메뉴개수가 변경되면 디자인을 다시 변경해야 하는 문제가 발생합니다. 물론 디자인 변경해서 적용하면 쉽게 해결되긴 합니다.

### itemBackground 적용
별도의 디자인 요청없이 메뉴 구분선 적용 방법을 찾다가, 아래 내용의 drawable xml(bottom_menu_item_bg.xml)을 작성하여 itemBackground 속성에 적용해 보았습니다. layer-list 요소를 이용하여 선색상 rectangle 위에 배경색 rectangle rectangle 3개를 겹쳐그려 좌우에 0.5dp의 선을 표현했습니다
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item>
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_divider" />
        </shape>
    </item>

    <item android:top="0dp" android:bottom="40dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

    <item android:top="16dp" android:bottom="16dp" android:left="0.5dp" android:right="0.5dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

    <item android:top="40dp" android:bottom="0dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

</layer-list>
```

```
  ...
    <android.support.design.widget.BottomNavigationView
      android:id="@+id/navigation"
      android:layout_width="0dp"
      android:layout_height="wrap_content"
      android:layout_marginStart="0dp"
      android:layout_marginEnd="0dp"
      android:background="?android:attr/windowBackground"
      app:itemBackground="@drawable/bottom_menu_item_bg"
      app:layout_constraintBottom_toBottomOf="parent"
      app:layout_constraintLeft_toLeftOf="parent"
      app:layout_constraintRight_toRightOf="parent"
      app:menu="@menu/navigation" />
  ...
```

### ripple 효과는 어디갔나
디자인 요구사항대로 구현은 되었으나, Android 5.0(API 21)에 추가된 [ripple effect](https://developer.android.com/reference/android/graphics/drawable/RippleDrawable)가 나타나지 않습니다.
[Google Developers Reference](https://developer.android.com/reference/com/google/android/material/bottomnavigation/BottomNavigationView.html#setItemBackground)를 찾아보니, itemBackground를 지정하면 ripple backgrounds가 사라진다고 되어있습니다.

### item drawable에 ripple 효과 적용
이대로 ripple효과를 포기할 수는 없으니, 위에 적용했던 itemBackground drwable xml(bottom_menu_item_bg.xml)에 ripple 효과를 적용해 보았습니다.

> 위에서도 언급했듯이 ripple 효과는 API21 부터 적용되기 때문에, res/drawable-21 폴더를 생성하여 아래 내용의 xml파일을(bottom_menu_item_bg.xml) 생성합니다.

```
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@color/bottom_menu_item_ripple">
    <item>
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_divider" />
        </shape>
    </item>

    <item android:top="0dp" android:bottom="40dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

    <item android:top="16dp" android:bottom="16dp" android:left="0.5dp" android:right="0.5dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

    <item android:top="40dp" android:bottom="0dp">
        <shape android:shape="rectangle" >
            <solid android:color="@color/bottom_menu_item_bg" />
        </shape>
    </item>

</ripple>
```
![itemBackground ripple 적용 결과](/img/post-cp2-android-bottomnavigationview.jpg)

### 더 좋은 방법
BottomNavigationView의 background 속성에 drawable을 적용하는 방법이 더 좋을까요? ㅎㅎ
