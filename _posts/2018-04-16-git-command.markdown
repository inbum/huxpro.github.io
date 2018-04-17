---
layout:     post
title:      "Git 기본 명령어"
subtitle:   "프로그래머라면 반드시 알아야 할 깃 기본 명령어"
date:       2018-04-16 12:00:00
author:     "Inbum"
header-img: "img/post-bg-git-logo.png"
header-mask: 0.3
category:   git
catalog:    true
multilingual: false
tags:
    - git
sitemap:
    priority: 0.5
    changefreq: 'monthly'
    lastmod: 2018-04-16 12:00:00
---

### Git 
개발자가 꼭 알아야 할 도구는 어떤것들이 있을까요? 여러 도구들이 있겠지만, Version control system 중 하나인 Git 또한 모든 프로그래머가 꼭 알아야 할 도구 중 하나입니다. Git은 공동 작업으로 프로젝트를 진행하는 동안, 파일 및 기록을 추적하고 프로젝트를 관리해 주는 강력한 도구 입니다. 무료로 사용할 수 있는 오픈 소스 플랫폼입니다. Git을 사용하는 프로젝트를 지원하는 웹호스팅 서비스인 GitHub는	 가장 인기있는 사이트 입니다. 제 블로그 역시 GitHub Pages를 이용해 만들어져 있으며, 모든 포스팅 및 블로그 내용들을 Git으로 관리하고 있습니다. 이번 포스팅에서는, 가장 기본이 되는 Git 명령어 몇가지를 정리 하고자 합니다. 본 포스팅은 추후 업데이트 혹은 분할 할 예정입니다.


### 1. git config
Git을 설치하고 나서 가장 먼저 해야 하는 것은 사용자 이름과 이메일 주소를 설정하는 것입니다.
git config 명령어를 이용해 사용자 정보를 설정합니다. \-\-global 옵션을 사용하여 전역으로 최초 1회만 설정하면 됩니다. 만약 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 \-\-global 옵션을 빼고 명령을 실행하면 됩니다.
~~~
$ git config --global user.name "Inbum Oh"
$ git config --global user.email inbum.o@gmail.com
~~~

#### 설정확인 ####
\-\-list 옵션을 이용해 현재 설정정보를 조회할 수 있습니다.
~~~
$ git config --global --list
$ git config --list
~~~


### 2. git init
기존 디렉토리를 Git 저장소로 만들때 사용하는 명령어 입니다. 프로젝트의 디렉토리로 이동해서 아래와 같은 명령어를 실행하면 *.git*이라는 하위 디렉토리(저장소에 필요한 뼈대파일-Skeleton)가 만들어지면서, 기존 프로젝트에 Git 저장소가 만들어 집니다. 
이 명령만으로는 아직 프로젝트의 어떤 파일도 관리되지 않습니다.
~~~
$ git init
~~~


### 3. git remote
리모트 저장소를 관리할 줄 알아야 다른 사람과 함께 일할 수 있습니다. 리모트 저장소는 인터넷이나 네트워크 어딘가에 있는 저장소를 말합니다. 저장소는 여러 개가 있을 수 있는데 어떤 저장소는 읽고 쓰기 모두 할 수 있고 어떤 저장소는 읽기만 가능할 수 있습니다. 간단히 말해서 다른 사람들과 함께 일한다는 것은 리모트 저장소를 관리하면서 데이터를 거기에 Push 하고 Pull 하는 것입니다. 리모트 저장소를 관리한다는 것은 저장소를 추가, 삭제하는 것뿐만 아니라 브랜치를 관리하고 추적할지 말지 등을 관리하는 것을 말합니다.

아래 명령을 통해 현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있습니다. *-v* 옵션을 주어 리모트 저장소의 단축 이름과 URL을 함께 볼 수 있습니다.
~~~
$ git remote -v
origin	https://github.com/inbum/inbum.github.io.git (fetch)
origin	https://github.com/inbum/inbum.github.io.git (push)
~~~


### 4. git clone
다른 프로젝트에 참여하려거나(Contribute) Git 저장소를 복사하고 싶을 때 사용하는 명령어 입니다. git clone 을 실행하면 프로젝트 히스토리를 전부 받아옵니다.
~~~
$ git clone https://github.com/inbum/inbum.github.io.git
Cloning into 'inbum.github.io'...
remote: Counting objects: 3235, done.
remote: Total 3235 (delta 0), reused 0 (delta 0), pack-reused 3235
Receiving objects: 100% (3235/3235), 49.19 MiB | 2.18 MiB/s, done.
Resolving deltas: 100% (1930/1930), done.
~~~
이 명령을 실행하면 inbum.github.io이라는 디렉토리를 만들고 그 안에 .git 디렉토리를 만듭니다.
(참고로, 위 저장소는 제 블로그 저장소 입니다)
그리고 저장소의 데이터를 모두 가져와서 자동으로 가장 최신 버전을 Checkout해 놓습니다.
inbum.github.io 디렉토리로 이동하면 Checkout으로 생성한 파일을 볼 수 있고 당장 하고자 하는 일을 시작할 수 있습니다.


### 5. git status
작업 디렉토리의 상태를 확인할 때 보통 git status 명령을 사용합니다. Clone 한 후에 바로 이 명령을 실행하면 아래와 같은 메시지를 볼 수 있습니다.
~~~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working tree clean
~~~
위의 내용은 파일을 하나도 수정하지 않았다는 것을 말해줍니다. Tracked 파일(이미 스냅샷에 포함되 있던 파일)은 하나도 수정되지 않았다는 의미입니다. Untracked 파일은 아직 없어서 목록에 나타나지 않습니다. 그리고 현재 작업 중인 브랜치를 알려주며 서버의 같은 브랜치로부터 진행된 작업이 없는 것을 나타냅니다. 기본 브랜치가 master이기 때문에 현재 브랜치 이름이 *master*로 나옵니다.

위 상태에서 README 파일을 하나 만들고 *git status*를 입력 해 보면 다음과 같은 결과가 나옵니다.
~~~
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README

nothing added to commit but untracked files present (use "git add" to track)
~~~
README 파일은 새로 만든 파일이기 때문에 'Untracked files'에 들어 있습니다. Untracked 파일은 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 봅니다. 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않습니다. 그래서 일하면서 생성하는 바이너리 파일 같은 것을 커밋하는 실수는 하지 않게 됩니다. 


### 6. git add
파일을 새로 추적할 때 사용하는 명령어 입니다. 아래 명령을 실행하면 Git은 README 파일을 추적합니다.

~~~
$ git add README
~~~
git status 명령을 다시 실행하면 README 파일이 Tracked 상태이면서 커밋에 추가될 Staged 상태라는 것을 확인할 수 있습니다.

~~~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
~~~
“Changes to be committed” 에 들어 있는 파일은 Staged 상태라는 것을 의미합니다. 커밋하면 git add 를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남게 됩니다. 이 명령을 통해 디렉토리에 있는 파일을 추적하고 관리하도록 합니다. git add 명령은 파일 또는 디렉토리의 경로를 아규먼트로 받습니다. 디렉토리면 아래에 있는 모든 파일들까지 재귀적으로 추가가 됩니다.


### 7. git commit
수정한 것을 저장하는 것을 커밋이라고 합니다. Unstaged 상태의 파일은 커밋되지 않습니다. Git은 생성하거나 수정하고 나서 git add 명령으로 추가하지 않은 파일은 커밋하지 않습니다. 그 파일은 여전히 Modified 상태로 남아 있습니다. 커밋하기 전에 git status 명령으로 모든 것이 Staged 상태인지 확인할 수 있습니다. 그 후에 git commit 을 실행하여 커밋합니다.

~~~
$ git commit
~~~
위 명령어를 실행하면 Git 설정에 지정된 편집기가 실행되고, 커밋한 내용을 쉽게 기억할 수 있도록 메시지를 작성 후 내용을 저장하고 편집기를 종료하면 Git은 입력된 내용(#로 시작하는 내용은 제외)으로 새 커밋을 하나 완성합니다.

*-m* 옵션을 이용하면 메시지를 인라인으로 첨부할 수도 있습니다. 
~~~
$ git commit -m "Issue 87: Fix benchmarks for speed"
[master 463dc4f] Issue 87: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
~~~
Git은 Staging Area에 속한 스냅샷을 커밋한다는 것을 기억해야 합니다.


### 8. git branch
모든 버전 관리 시스템은 브랜치를 지원합니다. 개발을 하다 보면 코드를 여러 개로 복사해야 하는 일이 자주 생깁니다. 코드를 통째로 복사하고 나서 원래 코드와는 상관없이 독립적으로 개발을 진행할 수 있는데, 이렇게 독립적으로 개발하는 것이 브랜치입니다.

사람들은 브랜치 모델이 Git의 최고의 장점이라고, Git이 다른 것들과 구분되는 특징이라고 말합니다. Git의 브랜치는 매우 가볍습니다. 순식간에 브랜치를 새로 만들고 브랜치 사이를 이동할 수 있습니다. 다른 버전 관리 시스템과는 달리 Git은 브랜치를 만들어 작업하고 나중에 Merge 하는 방법을 권장합니다. Git 브랜치에 능숙해지면 개발 방식이 완전히 바뀌고 다른 도구를 사용할 수 없게 됩니다.

브랜치에 관해 알아야 할 내용이 정말 중요하지만, 현재 단계에서는 가장 기본적인 명령어만 알아보겠습니다.
~~~
$ git branch
  iss53
* master
  testing
~~~
*git branch* 명령은 단순히 브랜치의 목록을 보여줍니다. \*기호가 붙어 있는 master 브랜치는 현재 Checkout 해서 작업하는 브랜치를 나타냅니다.

*-a* 옵션을 이용하면 로컬 브랜치와 리모트 브랜치의 모든 목록을 볼 수 있습니다.
~~~
$ git branch -a
  iss53
* master
  testing
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/revert-42-revert-41-master
~~~


### 9. git checkout
'git checkout 브랜치명/태그명' 명령을 이용하면 해당 브랜치나 태그로 작업트리를 변경합니다.
*-b* 옵션을 이용하면 새로운 브랜치를 만들면서 동시에 작업트리를 변경할 수 있습니다.
~~~
$ git branch testing
$ git checkout testing
Switched to branch 'testing'
$ git checkout -b new_branch
Switched to a new branch 'new_branch'
~~~


### 10. git reset
~~~
$ git reset <옵션> <돌아가고싶은 커밋>
~~~
위 명령어를 사용하면 돌아 가려는 커밋으로 리파지토리는 재설정되고, 해당 커밋 이후의 이력은 사라집니다


지금까지 알아본 명령어는 가장 기본적인 git 명령어 입니다. 조금 더 자세한 사용법은 아래 링크로 대신합니다. :)

[Pro Git book](https://git-scm.com/book/ko/v2){:target="_blank"}
