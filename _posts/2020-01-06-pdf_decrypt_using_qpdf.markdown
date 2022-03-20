---
layout: post
title:  "복사방지 걸린 pdf 파일 해제"
date:   2020-01-06 23:23:35 +0900
categories: 
- etc
tags:
- mac
- pdf
- qpdf
classes: wide
published: true
---

공부하는 자료가 있는데 복사방지 암호가 걸려있네요. 번역을 하려면 붙여넣기가 안되서 계속 치고 있는 상태다 보니 너무 귀찮아서 찾아보니 암호를 해제해 주는 방법이 있어서 남깁니다.


https://www.cyberciti.biz/faq/removing-password-from-pdf-on-linux/


https://www.cyberciti.biz/faq/removing-password-from-pdf-on-linux/#comment-45548

```
gs -q -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -sOutputFile=unencrypted.pdf -c .setpdfwrite -f encrypted.pdf
```

위 링크는 gs를 이용해서 처리하라고 하며 예제가 나옵니다. 하지만 명령어가 길고 복잡한게 단점입니다.
다른 방법을 찾아보니 qpdf를 이용해서 짧게 처리하는 구문이 있어서 적용했더니 쉽게 되었습니다. 추가로 이게 더 빠르게 진행이 되었습니다.

https://www.cyberciti.biz/faq/removing-password-from-pdf-on-linux/#comment-779682


```
qpdf --decrypt protected.pdf unprotected.pdf
```

mac os에서 qpdf를 설치하는 방법이 있어 추가로 공유합니다.

[http://macappstore.org/qpdf/](http://macappstore.org/qpdf/)

brew가 설치되어 있는 상태에서 다음으로 설치가 가능합니다.

```
brew install qpdf
```