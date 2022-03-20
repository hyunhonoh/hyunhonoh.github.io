---
layout: post
title:  "FC660M Mac 키보드 설정"
date:   2021-03-03 23:23:35 +0900
categories:
- mac
tags:
- fc660m
- keyboard
- autohotkey
- karabiner
- mac
classes: wide
published: true
---

맥에서 마우스 사용하는게 많은 작업을 할때는 편리하지 않아 마우스를 붙였더니 키보드도 어색해 져서 윈도우용으로 사용하던 FC660M을 맥북에 붙여 보았습니다.
키보드 자체는 나쁘지 않은데 맥용 키랑 매칭이 되지 않아 어려우면서 더불어 원격으로 윈도우를 사용하는데 키가 복잡하여 이 기회에 정리를 해 보았습니다.

기존에 FC660M을 사용할 때는 딥스위치를 3번에 놓고 사용하였습니다. 
![](/images/fc660m_dipswitch.png)

- FC660M 스위치별 옵션

```
SW1: 왼쪽 Ctrl, Caps Lock 작동을 서로 바꿈
SW2: Windows, 왼쪽 Alt 작동을 서로 바꿈
SW3: Windows, Fn 작동을 서로 바꿈
SW4: Windows 비활성화
```

맥에 맞추기 위해 찾아본 글에는 SW2로 설정하라고 하여 해당 내용을 참조해서 변경을 하고 karabiner를 이용해서 추가 설정을 하니 쾌적해졌습니다.

![](/images/mac_karabiner.png)

```
right_control → right_option
right_option → right_command
```


이 상태에서 원격으로 접속하는 윈도우의 한영키가 이번에는 정상적이지 않게 되어 다시 설정을 찾아보니 한영키를 왼쪽shift + space로 변경해서 사용하는 내용이 있었습니다.
윈도우의 키보드 설정은 설정1로 지정을 했습니다. 

- autohotkey 설정

```
+space::Send, {vk15sc138}
Return
```


설정을 마치면 맥에서는 한영 변환을 casplock으로 하고 원격 윈도우에서는 왼쪽 shift + space로 한영 변환을 합니다. 두개를 같은 키로 설정을 하면 MacOS의 설정에 의해서 원격의 한글이 정상적으로 변경이 안되는 경우가 있어 따로 키를 분리하는게 사용하는데 불편함이 없었습니다.




참고 

- [https://oinkbread-days.tistory.com/3](https://oinkbread-days.tistory.com/3)

- [https://harryhan24.github.io/post/2020/02/16/auto-hot-key-windows/](https://harryhan24.github.io/post/2020/02/16/auto-hot-key-windows/)

- [https://previous-on-johnpark82.tistory.com/2460791](https://previous-on-johnpark82.tistory.com/2460791)
