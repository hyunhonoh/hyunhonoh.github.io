---
layout: post
title:  "Splunk 특정 위치 xml 파싱 명령어 만들기"
date:   2018-11-29 23:23:35 +0900
categories: 
- Splunk
tags:
- python
- splunk
- xml
- TextMining
classes: wide
published: false
---



Splunk에는 여러 텍스트 형태의 데이터를 입력 할 수 있습니다. 일반적인 기계로그부터 json, xml형태의 데이터를 입력할 수 있습니다. 이들은 모두 수집시 파싱 형태를 지정해 주면 알아서 splunk에서 인식을 합니다.

또한 spath라는 명령어를 이용해서도 데이터를 인식하고 파싱해서 필드로 사용할 수 있습니다.





```
include ld.so.conf.d/*.conf
/usr/local/lib
```