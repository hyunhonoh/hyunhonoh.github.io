---
layout: post
title:  "iterm2 에서 한글 자소 분리 현상 해결"
date:   2020-12-15 23:23:35 +0900
categories:
- mac
tags:
- iterm2
- nfc
- mac
classes: wide
published: true
---

맥 재설치 후 iterm2를 이용해서 접속시 한글이 분리되는 현상이 발생하였습니다.

기본 터미널에서는 정상으로 표시가 되나 사용하고 있는 iterm2에서는 분리가 되어 확인시 다음의 설정으로 정상으로 표현이 가능했습니다.

[참조 블로그](https://ifuwanna.tistory.com/267)

해당 기능은 3.3.10 이후에 발생한다고 합니다.

- iTerm2 > Preferences (⌘,) 
- Profile > Text > Unicode 항목에서 Unicode normalization form 옵션을 기존 None 에서 NFC로 변경

변경하면 정상으로 한글이 표시가 됩니다.
