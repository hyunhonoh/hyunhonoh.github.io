---
layout: post
title:  "앱을 이용해서 bear에 글쓰기"
date:   2020-02-27 10:23:35 +0900
categories: 
- etc
tags:
- bear
- 단축어
- 글쓰기
classes: wide
published: true
---

지난 글에 [단축어를 이용해서 bear에 글쓰기](https://hyunhonoh.github.io/etc/write_to_bear_using_shortcuts/)를 작성하고 아이폰에서 열심히 매일의 일상을 기록하고 있습니다. 하지만 맥을 사용하는 시간에도 입력을 하기 위해서는 다시 아이폰을 들어서 해야한다는 부족함이 있네요. 사람은 하나가 편해지면 다른 것을 찾는거 같습니다.

그래서 맥에서도 비슷한 방식으로 글을 입력할 수 있는 방법을 찾는 중에 x-callback-url에 대해서 알게 되었습니다. 그리고 검색 하다가 파이썬 스크립트 까지 찾게 되었습니다. 
[python xcall](https://github.com/robwalton/python-xcall)이라는 파이썬 스트립트인데 이것도 [xcall](https://github.com/martinfinke/xcall)이라는 커멘트 실행 프로그램의 wrapper였습니다. 다운을 받아서 테스트를 해 보았는데 문제가 2.7 버전으로 만들어져 있어서 한글 입력등의 문제도 있고 코드를 수정해야 하는 부분도 있고 UI가 없어서 글을 입력하는데 어려움이 발생했습니다. 빠르게 잠깐의 생각을 넣기 위한 것인데 그러면 의미가 없어보입니다.

다시 검색을 시작하니 베어에 노트를 작성해주는 앱도 찾게 되었습니다.

[https://github.com/Savjee/bearclaw](https://github.com/Savjee/bearclaw)

![logo-bearclaw.png](/images/logo-bearclaw.png)

![image](https://camo.githubusercontent.com/d7c2a63de92cfb9d1b97d09600ba4a9b81e9fe0f/68747470733a2f2f7361766a65652e6769746875622e696f2f62656172636c61772f696d672f73637265656e73686f742e706e67)

메뉴바에 위치하면서 내가 원하는 본문이나 그림을 빠르게 입력하여 새로운 노트를 만들어 주는 앱입니다. 본문만은 괜찮은데 개발이 멈춰서 그런지 잘 못 한건지 Catalina 에서는 그림을 캡쳐하면 배경화면이 캡쳐되어서 들어갑니다. 
또한 본문만 입력해주는 앱이라 원하는 기능인 자동으로 날짜가 지정되고 본문이 append가 되지 않습니다. 대신 소스를 받아서 수정해보니 원하는 용도로 변경할 수 있었습니다.

![mac_bear_writing.png](/images/mac_bear_writing.png)

아이폰에서는 위치를 알 수 있는데 맥에서는 어떻게 처리하는지 몰라서 위치를 넣을 수 있는지 찾고 있는데 못 찾고 있습니다. 조금 더 수정하고 코드를 넣으면 원하는 용도로 수정이 가능할 듯 합니다. 완성이 된다면 기능 개선 request 라도 보내봐야겠습니다.

