---
layout: post
title:  "Mac 에서 사용하는 터널링 관리 프로그램"
date:   2019-08-26 20:23:35 +0900
categories: 
- mac
tags:
- ssh
- tunneling
- mac
- coccinellida
classes: wide
published: true
---



외부에서 회사 업무를 수행하게 되었을 때 보통은 VPN이라는 것을 연결해서 사용하였습니다. 하지만 저희 회사는 공유기에 연결하는 VPN이라는 문제도 있고 공유기라서 그런지 생각만큼 속도가 안나옵니다. 

다른 방법을 찾던 중 그동안 이해가 어려워(?) 적용을 못하던 SSH 터널링에 대해서 다시 알아보고 있었습니다. 보통 윈도우의 경우 putty를 이용한 방법이 많이 있습니다. 하지만 이 방법은 좀 어렵습니다. 그 외에는 잘 안 알려져 있는 터널링 프로그램을 이용해서 작업하는 형태가 있습니다. 이 부분은 나중에 윈도우에서 설정할 때 작성해 보겠습니다.



맥에서도 접근이 가능한 방법이 여러가지가 있는데 맥은 기본적으로 ssh 가 내장되어 있어 그런지 모든 예제가 ssh 를 이용해서 하는 방법으로 되어 있었습니다.

[https://blog.lael.be/post/845](https://blog.lael.be/post/845)

이 링크가 가장 간단하게 바로 적용할 수 있는 형태로 이해되게 작성되어 있었습니다. 

하지만 터미널을 항상 열어두고 있어야 하고 접속이 끊기면 수동으로 다시 연결을 해 주어야 하고 명령줄이 길어지는 문제가 있었습니다.



좀 더 구글링을 해보니 다음과 같은 맥용 터널링 프로그램을 발견했습니다.



- https://github.com/primalmotion/sshtunnel
- https://coressh.io
- [http://coccinellida.sourceforge.net](http://coccinellida.sourceforge.net)



그중에 마지막 버전을 선택해서 적용중입니다. 아이콘과 효과음에 조금 놀라긴 하지만 켜면 자동으로 연결하고 알람을 해주는게 제가 원하던 기능만 있는 버전이었습니다.

![Coccinellida](/images/Coccinellida.png)



또한 여러개의 접속을 설정할 수 있게 되어 있습니다. 

![Coccinellida_2](/images/Coccinellida_2.png)



기존 VPN보다는 속도가 더 나오는것 같고 효율적인 형태라고 보입니다. 