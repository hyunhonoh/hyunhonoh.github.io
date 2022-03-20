---
layout: post
title:  "Splunk rex사용시 한글 범위 검색 패턴"
date:   2018-06-15 23:23:35 +0900
categories: 
- Splunk
tags:
- Splunk
- rex
- hangul
- 한글
classes: wide
published: true
---



Bootcamp를 수행하면서 한글 주소에서 앞부분의 시도를 추출하기 위해서는 기존의 방식처럼 다음과 같은 방식을 사용했다.

```bash
index=bootcamp sourcetype=bootcamp
| table 행정구역명
| rex field=행정구역명 "(?<addr>[가-힣]+)"
| stats count by addr
```



한글의 유니코드 범위를 나타내는 방법만을 알고 있어서 이렇게 강의도 하면서 알려줬는데 다른 방법이 있었네요. 역시 새로운 블로그여도 기존의 상투적인(?) 방법을 지속적으로 소개하는 듯 하다. 아니면 다른 방법을 못 찾았던거 였기도 하고.

문장 분리를 해야해서 그 부분을 찾다가 다음의 블로그를 보고 충격(?)을 받았다. 

https://www.lucypark.kr/blog/2013/03/21/chunking-korean-one-liner/#fn:2

regex를 사용하는데 한글을 ```\p{Hangul}``` 이런 방법으로 사용을 하였다. 



스플렁크에서도 적용하니 결과가 잘 나왔다.  왜 이 방법을 모르고 있었을까?

```bash
index=bootcamp sourcetype=bootcamp
| table 행정구역명
| rex field=행정구역명 "(?<addr>\p{Hangul}+)"
| stats count by addr
```

| - | addr | count|
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| 1                                                            | 경기도                                                       | 387  |
| 2                                                            | 경상남도                                                     | 8    |
| 3                                                            | 대전광역시                                                   | 36   |
| 4                                                            | 울산광역시                                                   | 40   |
| 5                                                            | 인천광역시                                                   | 4    |
| 6                                                            | 충청북도                                                     | 24   |
