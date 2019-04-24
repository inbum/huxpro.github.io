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
#### 1.1
