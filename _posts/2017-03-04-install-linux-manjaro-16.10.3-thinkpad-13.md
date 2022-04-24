---
layout: post
title:  thinkpad13 manjaro linux 16.10.3 설치기
date:   2017-03-04 03:37:33 +0900
categories: linux manjaro thinkpad13
---
작년에 구매한 thinkpad13(i3, ddr4)에 ubuntu 16.04 LTS 버전을 사용하고 있었다. 잘 사용하고 있었지만, 시스템이 무겁다는 생각을 버릴 수가 없어서 OS 재설치를 결심했다.


OS선정
======

---

지금까지 사용해본 리눅스 배포판 중 가장 오래 사용한 배포판은 우분투10.04LTS 버전(MSI U200X)이다.

이 랩탑에 설치된 GCC버전을 무리하게 업데이트하면서 의존성이 꼬였고 의존성이 꼬인상태에서 다음 버전으로 업그레이드가 안되면서 포맷을 하게되었는데, 그때는 이미 랩탑을 구입한지 4년후였다.

이후에는 저사양에 적합한 리눅스 배포판을 찾기위해 포맷 -> 하루이틀 사용 -> 포맷이 반복되었다.

결국 크런치뱅이라는 데비안 기반의 배포판을 사용하게 되었고 DE로는 오픈박스를 사용하게 되었다.

오픈박스는 우분투에 설치된 그놈이나 유니티와는 다르게 GUI설정이 아닌 - 물론 그놈이나 유니티도 GUI 설정프로그램이 파일을 수정한다. - 설정파일을 편집기에서 수정하여 커스터마이징을 하는데, 그 심플함이 마음에 들었다.

아무 걱정하지 말고 -그놈이나 유니티 등등의 다른 DE에서는 서치 -> 설정 실패 -> 서치 -> 다른 방식으로 설정했던 일이 비일비재했다. - 설정파일만 맞게 수정하면 가볍고 빠르게 의도한대로 동작했었다.

이 때의 향수가 남아있는지 스카이레이크 기반의 i3 CPU가 장착된 랩탑도 우분투16.04버전이 무겁게 느껴졌나 보다.

그래서 다시 포맷을 하기로 결정하였고 다음과 같은 기준으로 검색을 시작했다.

1.	심플하게 설정가능 할 것
2.	HiDPI 적용 시 유려하게 나올 것 - 맥처럼
3.	심플하게 설정가능 할 것

여러 후보군 - Debian, arch, Antergos - 이 있었지만,나에겐 xfce를 기본으로 배포하는 arch - 롤링릴리즈 방식 - 기반의 manjaro linux가 가장 적합할 것으로 판단했다.

다운로드
========

---

[https://manjaro.org/get-manjaro/https://manjaro.org/get-manjaro/] 에서 다운로드

왠만하면 토렌트로 다운로드할 것, 소스포지는 너무 느림

boot USB 생성
=============

---

USB 스틱을 포트에 삽입한 이후

```
# /dev/sdb/는 마운트 상태에 따라 마지막 알파벳이 달라질 수 있다.
sudo dd if=manjaro.iso of=/dev/sdb bs=4M
```

install
=======

---

부팅과정은 GPT를 사용하기 위해 바이오즈를 설정한 것과 파티셔닝외에 특별한 것이 없다.

manjaro linux는 라이브모드와 GUI 설치를 지원한다.

UFI부팅
-------

---

bios에서 부팅을 UFI만 하도록 설정해야 USB로 부팅 시 UFI로 부팅이 된다.

파티셔닝
--------

---

-	메모리가 8G인 관계로 swap은 쓰지않는다.
-	임시 / 로그성 파일은 다른 파티션으로 분리한다.

| 마운트 포인트 | 플래그 | 포맷형식 | 할당사이즈 | | /boot/efi | esp, boot | fat32 | 128m | | / | root | ext4 | max - 173G | | /var | - | ext4 | 2G |

설치 후 설정
============

---

업데이트
--------

---

2017년 03월 03일 기준으로 약 800M의 업데이트가 있다. 그래서 /var의 크기를 2G로 잡았다.

```
sudo pacman -Syy
```

SSD
---

---

ext4를 사용하는 경우, 옵션에 defaults,noatime,discard가 있는지 확인.

없으면 추가

```
sudo vi /etc/fstab
```

firefox memory cache
--------------------

---

1.	open up about:config (type it into the url bar)
2.	type browser.cache into the filter bar at the top.
3.	Find browser.cache.disk.enable and set it to false (by double clicking on it).
4.	set browser.cache.memory.enable to true
5.	create a new preference by right clicking anywhere, hit New, and choose Integer.
6.	Call the new preference browser.cache.memory.capacity and hit OK.
7.	In the next window, where it asks for the number of kilobytes you want to assign to the cache, just enter -1 to tell Firefox to dynamically determine the cache size.

[http://forum.notebookreview.com/threads/ssd-owners-set-firefox-to-memory-cache-instead-of-disk.533359/\]

HiDPI
-----

모니터크기와 해상도에 해당하는 DPI는 아래 사이트에서 확인

[https://www.sven.de/dpi/\]

[Whisker menu] -> [settings] -> [Appearance] -> custm DPI Settings 165(13.3inch / 1920*1080)

Dock
----

---

```
sudo pacman -S plank
```

theme
-----

---

```
yaourt xfce4-windowck-plugin
yaourt numix
yaourt numix-circle-icon-theme-git
yaourt numix-icon-theme-git
yaourt plank-theme-numix
```

한글입력기
----------

---

```
sudo pacman -Sy fcitx-hangul fcitx-configtool fcitx libhangul fcitx-gtk2 fcitx-gtk3 fcitx-qt5
```

font
----

---

기본폰트도 훌륭하지만 익숙한 폰트로 교체했다.

```
sudo pacman -Sy noto-fonts sudo pacman -Sy noto-fonts-cjk
```

이후 xfce4 기본폰트 / 파이어폭스 폰트를 설정에서 변경

tlp
---

---

```
sudo pacman -Sy tlp
sudo tlp stat // 확인용
```

thinkpad fan controller
-----------------------

---

```
yaourt -Sy thinkfan
```

이후

1.	modprobe.d에 등록
2.	설정파일 생성
3.	sysctl에 등록

xfdashboard
-----------

---

맥용 Expose와 비슷하게 동작한다.

```
yaourt xfdashboard
```

설치 후 F9을 단축키로 등록한다.

시작프로그램에 데몬형태로 수행되도록 등록한다.

touchpad gesture
----------------

---

2핑거 제스쳐는 이미 훌륭하게 동작

3핑거 / 4핑거 제스쳐에 대해 등록

```
yaourt libinput-gestures
sudo gpasswd -a $USER input
libinput-gestures-setup autostart
sudo vi /etc/libinput-gestures.conf
```

trouble shooting
================

---

아직 리눅스는 모든 장치를 추가설정 없이 사용하기는...ㅜㅜ

파이어폭스에 테마가 적용되지 않는 현상
--------------------------------------

---

기본 테마 제거

```
rm -rf ~/.mozilla/firefox/manjaro.default/chrome
```

plank 설치 후 모니터에 이상한 라인이 생기는 현상
------------------------------------------------

---

[Whisker menu] -> [settings] -> [Window Manager tweak] -> [Compositor] -> [Show shadow under doxk window] checkbox 해제

듀얼디스플레이 사용 시 확장 모니터에서 윈도우 전체 화면 시 패널사이즈 만큼 바탕화면이 보이는 현상
-------------------------------------------------------------------------------------------------

---

어떻게 해결했는지 기억이 나지 않는다.

아마 아래 과정에서 해결되지 않았을까 생각된다.

[https://forum.xfce.org/viewtopic.php?id=8052\]

듀얼디스플레이 사용 시 Panel의 창단추가 다른 모니터의 창단추를 보여주는 현상
----------------------------------------------------------------------------

---

[http://kwonnam.pe.kr/wiki/linux/xfce] 참고

아래 스크립트를 시작프로그램으로 등록

```
#!/bin/sh sleep 5 xfce4-panel -r

```

부팅 시 가끔 X가 실행되지 않는 현상
-----------------------------------

---

스카이레이크의 그래픽카드를 지원하는 커널모듈에 버그가 있어 가끔씩 X가 수행되지 않고 검은화면만 보이는 경우가 있다.

따라서 아래를 실행한다.

위 사항이 커널 4.4.X에서만 발생하는 것인지 4.9.X에서도 발생하는지는 명확하지 않으나 커널 업데이트 이후 실험은 생략한다.

```
sudo vi /etc/modprobe.d/i915.conf options i915 enable_fbc=0 enable_psr=0
```

sleep상태에서 wakeup이 되지 않는 현상
-------------------------------------

---

현재 버전의 manjaro는 linux 4.4.X의 커널을 사용중인데 해당 커널에는 버그가 있어 sleep 모드로 들어갔다가 wakeup이 되지 않는다.

따라서 LTS버전인 linux 4.9.13-1 버전으로 업데이트하였다.

[Whisker menu] -> [Setting] -> [manjaro Setting Manager] ->[Kernel] 기능으로 쉽게 업데이트할 수 있다.

## Manjaro XFCE Setting

### remove window titlebar in xfce

Check out Settings Manager > Window Manager Tweaks > Accessibility > Hide title of windows when maximized.
Maximized windows will not have the title bar then, while non-maximized windows will have it. Tested on ArchLinux, xfce version 4.12.0-4.

Settings Manager > Panel > uncheck Don't reserve space on borders.

### merge both window's titlebar(title, button) and ffce pannel
yay -S xfce4-windowck-plugin
use below plugin in pannel preference
window header - button
window header - title

### dock 
yay -S plank

### application launcher
QT_SCALE_FACTOR env is for HIDPI
QT_SCALE_FACTOR=1 albert

### terminal
yay -S tilix
Settings Manager > default appilcaions > Utilities > Terminal Emulator

### browser
yay -S naver-whale-stable
Settings Manager > default appilcaions > Internet > Naver Whale
