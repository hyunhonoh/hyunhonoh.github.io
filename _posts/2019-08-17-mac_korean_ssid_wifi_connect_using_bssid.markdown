---
layout: post
title:  "Mac 에서 한글로 된 SSID 접속 안될 때 Terminal에서 접속"
date:   2019-08-17 20:23:35 +0900
categories: 
- mac
tags:
- mac
- iterm2
- wifi
- ssid
- git
classes: wide
published: true
---


아이와 함께 키즈카페에 왔는데 키즈카페의 무선 ssid가 한글로 되어 있어서 기본 설정의 맥에서는 연결이 안되었습니다. 인터넷에 찾아보니 진단으로 연결을 할 수 있다고 하는데 아마도 그 방법들은 sierra부터는 안되는거 같았습니다.

![ssid](/images/20190817_ssid.png)

다른 방법을 찾아보니 bssid라는 값을 이용해서 접속할 수 있고 그런 프로그램이 있네요.

```
https://github.com/deekayw0n/airport-bssid
```

우선은 이런 환경이 되기 전에 미리 다운을 받았어야 했다는 것이네요. 순간 핸드폰 마저 없었으면 이걸 사용할 수도 없었을 수도 있었네요.
어째든 사용법은 다음과 같습니다.

```bash
git clone https://github.com/deekayw0n/airport-bssid.git
cd airport-bssid
```

확인 할 떄 무선 장비의 이름을 알고 있어야 조회가 가능합니다. 

```
ifconfig
```

확인이 되면 다음과 같이 ssid를 조회할 수 있습니다.

```bash
$ ./Build/Release/airport-bssid en0
***** Scanned networks *****
...
- [ssid: ¾ÆÀÌµé¼¼»ó    , bssid:  90:9f:33:6a:62:53, channel:   8, rssi: -46 dBm]
****************************
Network scan completed. If you want to connect to specific BSSID, please enter the command below:
 ./Build/Release/airport-bssid <ifname> <bssid> [<password>]
```
찾은 bssid를 확인 후에 뒤에 bssid와 비밀번호를 적어주면 접속이 잘 됩니다.

```bash
$ ./Build/Release/airport-bssid en0 90:9f:33:6a:62:53 [<password>]
```

이 방법을 이용하면 인터넷 접속도 편리하게 바꾸면서 활용할 수 있을거 같습니다.