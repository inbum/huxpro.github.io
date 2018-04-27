---
layout:     post
title:      "파이썬 정규표현식"
subtitle:   "프로그래밍을 배우기 전에 정규표현식부터 배워야 한다??"
date:       2018-04-26 12:00:00
author:     "Inbum"
header-img: "img/post-bg-python-regex.jpg"
header-mask: 0.3
category:   python
catalog:    true
multilingual: false
tags:
    - python
sitemap:
    priority: 0.5
    changefreq: 'monthly'
    lastmod: 2018-04-26 12:00:00
---

### 정규표현식 
정규표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어입니다. 정규 표현식은 많은 텍스트 편집기와 프로그래밍 언어에서 문자열의 검색과 치환을 위해 지원하고 있습니다. 특히 마이크로소프트 워드 또는 오픈오피스와 같은 편집기와 워드프로세서의 찾기 및 찾아 바꾸기 기능에서도 이 정규표현식 기반으로 검색 할 수 있지만 프로그래머가 아닌 많은 사람들은 이를 잘 알지 못합니다. 정규표현식을 쓰면 일반 사용자 뿐만 아니라 프로그래머도 엄청나게 시간을 절약할 수 있습니다. 기술 전문 저자인, 코리 닥터로우는 프로그래밍을 가르치기 전에 정규표현식부터 교육해야 한다고 주장할 정도 입니다.

> [정규표현식을] 안다는 것은 문제를 3단계로 해결하는 것과 3,000 단계로 해결하는 것만큼이나 차이가 있을 수 있다. 여기에 푹 빠지면 몇 차례의 키보드 입력만으로 해결할 수 있는 문제가 다른 사람들에게는 지루하고 오류가 일어나기 쉬운 작업이라는 사실조차도 잊어비럴 것이다.[^1]

이번 포스트에서는 파이썬에서 정규표현식으로 텍스트 패턴 찾는 방법 대해 알아보겠습니다.

### 1. 정규표현식 없이 텍스트 패턴을 찾는다면...
문자열 안에서 (서울의) 전화번호를 찾고 싶다고 가정해 보겠습니다. 실제 전화번호는 다양한 패턴이 존재하지만, 이해의 편의를 위해 검색 패턴은 2개의 숫자, 하이픈, 세 개의 숫자, 하이픈, 네 개의 숫자 순으로 정의하겠습니다. 즉, 다음과 같을 것입니다. 02-730-5800(청와대). 문자열이 이러한 패턴과 일치하는지 여부를 확인하는 함수를 만들어 보겠습니다. 실제로 이렇게 작성하면 좋지 않다는 것을 보여주는 함수입니다.
```python
def isPhoneNumber(text):
    if len(text) != 11:
        return False

    for i in range(0, 2):
        if not text[i].isdecimal():
            return False

    if text[2] != '-':
        return False

    for i in range(3, 6):
        if not text[i].isdecimal():
            return False

    if text[6] != '-':
        return False

    for i in range(7, 11):
        if not text[i].isdecimal():
            return False

    return True

print('02-730-5800는 전화번호 입니까???')
print(isPhoneNumber('02-730-5800'))
print('Spam 은 전화번호 입니까???')
print(isPhoneNumber('Spam'))

$ python3 isPhoneNumber.py
02-730-5800는 전화번호 입니까???
True
Spam 은 전화번호 입니까???
False
```

일단 정상동작 하는 함수를 만들기는 했습니다. 이 함수로, 긴 문자열에서 서울시 전화번호 패턴(02-XXX-XXXX로 가정)을 찾기 위한 코드를 작성하려면 어떻게 해야 할까요? 다음 코드를 추가해 보겠습니다.
```python
message = '청와대 민원실 전화번호는 02-730-5800 입니다. 자동응답기로 연결되네요. 02-730-5800 장난전화 하지 마세요.'
for i in range(len(message)):
    chunk = message[i:i+11]
    if isPhoneNumber(chunk):
        print('전화번호: ' + chunk)

$ python3 isPhoneNumber.py
전화번호: 02-730-5800
전화번호: 02-730-5800
```

위 소스코드는 원하는대로 동작하긴 하지만, 한눈에 보기에도 비효율적으로 보입니다. 예를들어, 다른 서울시의 전화번호 패턴을 찾고 싶다면 어떻게 해야 할까요. '02-XXXX-XXXX' 또는 '02-XXX', '(02)XXX-XXXX' 와 같은 형태의 패턴도 함께 찾고 싶다면? 또는 전국의 모든 전화번호, 핸드폰 번호, 팩스번호 까지 포함한 모든 전화번호를 찾고 싶다면? 슬슬 머리가 아파오기 시작합니다. 정규표현식을 쓰면 이러한 프로그램을 더욱 효율적으로 빨리 만들 수 있습니다.

### 2. 정규표현식으로...
이번에는 정규표현식을 이용해 위와 동일한 패턴(02-XXX-XXXX)의 문자열을 찾는 소스코드를 작성해 보겠습니다.
```python
import re

phoneNumRegex = re.compile(r'\d{2}-\d{3}-\d{4}')

message = '청와대 민원실 전화번호는 02-730-5800 입니다. 자동응답기로 연결되네요. 02-730-5800 장난전화 하지 마세요.'

for phoneNum in phoneNumRegex.findall(message)
    print('전화번호: ' + phoneNum)

$ python3 isPhoneNumber2.py
전화번호: 02-730-5800
전화번호: 02-730-5800
```

한눈에 보기에도 너무 간단한 소스코드가 되었습니다.
간단히 설명하면,
import re로 정규식 모듈을 가져온 뒤
re.compile() 함수로 Regex객체를 만듭니다. ( 이때, 문자열 첫번째 따옴표 앞에 r을 놓아 원시 문자열을 전달하였습니다.)
패턴과 일치하는 모든 문자열을 검색하기 위해 'message'변수를 findall()메소드로 전달한 뒤 출력했습니다.
> 파이썬에서 다른언어들과 마찬가지로 백슬래시(\\)를 이스케이프 문자로 사용합니다.
> \\n : line feed 줄바꿈
> \\t : tab
> \\r : carriage return
> \\0 : null
> \\ : back slash
> \\' : single quotation mark(단일 인용 부호)
> \\" : double quotation mark(다중 인용 부호)
> 하지만 문자열 값으 첫 번째 따옴표 앞에 r을 붙여 원시 문자열(raw string), 즉 글자를 이스케이프하지 않는 문자열로 지정할 수 있습니다.
> 따라서, 정규표현식에서는 백슬래시를 자주 사용하기 때문에 원시문자열을 이용하여 전달하면 편리하게(백슬래시를 두번 사용할 필요 없이) 정규표현식을 표현할 수 있습니다.

### 3. 자주 사용하는 파이썬 정규표현식 문법
| 메타문자 | 설명 | 
|---|---|
| ? | 그 앞의 그룹이 0~1번 나타나는 패턴과 일치시킨다. |
| * | 그 앞의 그룹이 0번 이상 나타나는 패턴과 일치시킨다. |
| + | 그 앞의 그룹이 1번 이상 나타나는 패턴과 일치시킨다. |
| {n} | 그 앞의 그룹이 정확히 n번 나타나는 패턴과 일치시킨다. |


계속 작성중...



[^1]: https://www.theguardian.com/technology/2012/dec/04/ict-teach-kids-regular-expressions