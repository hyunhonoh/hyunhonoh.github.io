---
layout: post
title:  "ubuntu mate 20.04 해상도 고정하기"
date:   2021-07-13 19:23:35 +0900
categories: 
- linux
tags:
- ubuntu
- virtualbox
- 해상도
- 고정
- linux
classes: wide
published: true
---

신규로 virtualbox에 ubuntu mate 20.04를 설치하였습니다. 기존에 만들어 놓은 이미지가 읽히지 않아 신규로 설치할겸 새로운 버전을 받아서 설치를 하고 전체화면 모드를 했는데 비율이 맞지 않았습니다.
실제 모니터의 비율은 1920*1080인데 virtualbox안의 ubuntu에서는 해당 해상도가 없었습니다.

원래 ubuntu에서 제공하는 해상도가 제한적인지는 모르겠지만 다음의 방법으로 해결하였습니다.

https://superuser.com/questions/758463/getting-1920x1080-resolution-or-169-aspect-ratio-on-ubuntu-or-linux-mint


```
/usr/share/X11/xorg.conf.d/ 경로에 10-monitor.conf 를 설정
재부팅
```

- 10-monitor.conf 설정 내용

```
Section "Monitor"
    Identifier "Virtual1"
    Modeline "p1920x1080"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
    Option "PreferredMode" "p1920x1080"
EndSection
```

해당 10-monitor.conf 설정이 있는 댓글의 위치가 20.04에서는 /usr/share/X11/xorg.conf.d/ 로 변경이 된 것으로 보입니다.

설정적용 후 재부팅 후에 정상적으로 고정되었습니다.

