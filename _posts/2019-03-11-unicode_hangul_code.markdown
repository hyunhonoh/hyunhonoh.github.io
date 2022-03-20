---
layout: post
title:  "유니코드 한글 코드 영역"
date:   2019-03-11 19:23:35 +0900
categories: 
- etc
tags:
- python
- unicode
classes: wide
published: true
---



NLP처리를 하면서 한글에 대한 범위를 여러개 지정하는데 보통은 [가-힣]만 알아도 대부분의 한글을 뽑을 수 있습니다.

하지만 작업하다가 갑자기 기억 범위의 한글만 다시 작업을 해야 하는데 순간 가의끝이 어디이지? 하는 의문의 생겨서 유니코드를 찾아보았습니다.

확인해 보니 "나" 전의 단어는 "낗" 이었습니다. 코드는 B097 이고요.



위키피디아에 보니 한글 코드에 대한 범위가 있어서 참고용으로 기록해 둡니다.



[유니코드 A000~AFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_A000~AFFF)

[유니코드 B000~BFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_B000~BFFF)

[유니코드 C000~CFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_C000~CFFF)

[유니코드 D000~DFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_D000~DFFF)





추가 참조 주소 

[[유니코드\] 한글 음절과 자모의 영역/주소 - Unicode Hangul Code Point Map](http://mwultong.blogspot.com/2006/09/unicode-hangul-code-point-map.html)

