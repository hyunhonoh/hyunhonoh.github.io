---
layout: post
title:  "Splunk eventgen app file 0.6.x"
date:   2018-05-21 18:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- eventgen
- git
classes: wide
published: true
---

Splunk의 앱을 설치하면 일부 앱에 데이터를 계속 생성시켜주는 설정이 있습니다. eventgen.conf로 설정된 이 파일은 splunk에서 제공하는 eventgen 을 설치하면 로그를 주어진 양식에 맞게 생성해 줘서 테스트 및 데모 용도로 사용을 할 수 있습니다.

github에 올려져 있는 주소 : [http://github.com/splunk/eventgen](http://github.com/splunk/eventgen)

기존에는 이 git을 다운받는 자체로 app으로서 수행이 가능했는데 지금은 바뀌어서 다양하게 활용 할 수 있도록 앱의 규모가 커졌습니다. 도커 컨테이너를 만들거나 pypi를 이용해서 설치하거나 app으로 변경할 수 있게 되었습니다.
그래서 splunk_app으로 만들기 위해서는 [SETUP.md](https://github.com/splunk/eventgen/blob/develop/documentation/SETUP.md)를 읽고 약간의 과정을 거쳐야 합니다.

SETUP.md를 읽고 만든 결과물 입니다. 개인적인 사용의 편리를 위해서 링크를 달아둡니다. 

[sa_eventgen_0.6.1.spl](/images/sa_eventgen_0.6.1.spl)