---
layout:     post
title:      "rsync를 이용한 서버 백업"
subtitle:   "AWS EC2로 생성한 리눅스 인스턴스의 파일 백업"
date:       2018-03-21 18:00:00
author:     "Inbum"
header-img: "img/post-bg-rsync-intro.jpg"
header-mask: 0.3
category:   rsync
catalog:    true
multilingual: false
tags:
    - rsync
sitemap:
    priority: 1.0
    changefreq: 'weekly'
    lastmod: 2018-03-21 18:00:00
---

### rsync란? 
rsync는 빠르고 기능이 아주 많은, 원격(로컬) 파일 복사 툴 입니다. rsync의 강력한 스크립트기능(scriptability)및 속도(speed), 유연함(flexibility) 덕분에 표준 리눅스 유틸리티가 되었고, 대부분의 리눅스 배포판에 포함되어 있습니다. Windows( Cygwin ), Ubuntu, CentOS, macOS에도 포팅되어 있습니다.

rsync의 몇가지 추가특징들은 다음과 같습니다.
- links, devices, owners, groups, and permissions 복사를 지원 합니다.
- GNU tar와 비슷한 exclude와 exclude-from옵션을 제공합니다.
- CVS exclude 모드 - CVS에서 무시하는 동일한 파일들을 무시.
- ssh 또는 rsh를 포함하여 어떠한 remote shell 을 사용할 수 있습니다.
- super-user 권한이 필요하지 않습니다.
- 대기 시간 비용을 최소화하기위해 파일 전송에 파이프 라이닝을 사용합니다.
- 익명사용자 또는 rsync 데몬 인증을 지원합니다.(미러링에 이상적)

rsync는 백업 및 미러링에 주로 쓰이고, 고급 복사 기능을 사용하기위한 명령으로도 널리 사용됩니다.

### 주로 사용하는 구문
~~~
rsync [OPTION] … SRC … [USER@]HOST:DEST
rsync [OPTION] … [USER@]HOST:SRC [DEST]
~~~


### 예를들어,
AWS EC2로 생성한 리눅스 인스턴스에 있는 파일을 백업하고자 할때, 아래 명령어 한줄을 이용하면 아주 쉽게 백업이 가능합니다.
추후 변경된 부분만 복사하고자 할 경우에도, 동일한 명령어를 사용하여 빠르고 효과적으로 복사가 가능합니다.
~~~
rsync -rave "ssh -i my-key-pair.pem" ubuntu@public_dns_name:/home/ubuntu/ myaws_backup/ 
~~~

> 참고로
> Amazon Linux의 경우 사용자 이름은 ec2-user입니다. 
> Centos의 경우 사용자 이름은 centos입니다. 
> Debian의 경우 사용자 이름은 admin 또는 root입니다. 
> Fedora의 경우 사용자 이름은 ec2-user입니다. 
> RHEL의 경우 사용자 이름은 ec2-user 또는 root입니다. 
> SUSE의 경우 사용자 이름은 ec2-user 또는 root입니다. 
> Ubuntu의 경우 사용자 이름은 ubuntu 또는 root입니다.

***

스크립트 작성 및 Cron을 이용하면 아주 손 쉽게 백업 시스템을 구축할 수 있겠지요.:smiley:

