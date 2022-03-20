---
layout: post
title:  "Nanum Font Webfont 적용"
date:   2018-07-02 22:23:35 +0900
categories: 
- life
tags:
- webfont
- font
- nanum gothic
- https
classes: wide
published: true
---


블로그 테마를 정비하면서 '나눔고딕' 폰트의 웹폰트를 적용하고자 여러 문서를 찾아보고 적용을 해 보았습니다. 그런데 css를 적용 후에 아래와 같은 메시지가 발생하면서 웹폰트 적용이 되지 않는 현상이 있었습니다.

![jsloaderror](/images/jsloaderror.png)

자세히 알아보니 현재 사용하고 있는 github pages 는 https 방식이고 인터넷에서 찾은 웹폰트를 가져오는 주소는 http여서 불안정한 주소의 로딩이라고 하여 크롬에서 차단이 됩니다.

```
@import url(http://fonts.googleapis.com/earlyaccess/nanumgothic.css);
```

지금도 수행되긴 하지만 역시 인터넷에서 그냥 자료를 가져오는 것에 대해서 다시 한번 검토가 필요하다고 느끼는 부분이었습니다.
나눔고딕만을 따로 적용하는 방법을 찾았는데 역시 서비스 제공 업체의 글이 제일 정확하다는 것을 다시 한번 확인했습니다.

[https://fonts.google.com/](https://fonts.google.com/)

![googlewebfont](/images/googlewebfont.png)

물론 이 방법을 보기 전에 [https://xetown.com/board/948391](https://xetown.com/board/948391) 사이트에서 정보를 얻고 이해를 하긴 했습니다.

최종적으로 아래의 내용을 추가하면 원하는 결과를 얻을 수 있습니다.

```
@import url('https://fonts.googleapis.com/css?family=Nanum+Gothic|Nanum+Gothic+Coding|Sunflower:300');
```

