---
layout: post
title:  "react native 연습"
date:   2020-11-13 23:23:35 +0900
categories: 
- develop
tags:
- react_native
- node
- ios
classes: wide
published: true
---


요즘에 관심을 가지고 있는 react native에 대해서 연습삼아 pycharm에서 수행이 된다고 하여 실행해 보았습니다. 결론은 무언가 부족한지 실행이 안되었습니다

===================================================

아래는 진행중 내용입니다.

react native 해보기

시작부터 아래와 같은 에러가 발생합니다.
```
Did you run "pod install" in iOS directory?
```

구글에 찾아보니 다른 글에는 다시 들어가서 설치했다고 하여 설치를 해 보았습니다.
```
https://github.com/bluelion2/Project-issue-repo/issues/14
```

```
cocoapods install error 참고하고 해결

프로젝트의 ios 폴더로 이동후

$ cd ios 
$ pod repo update
$ pod install
```

위와 같이 했는데 아래 메시지 추가 출력 되었습니다.

```
[!] Invalid `Podfile` file: [!] Unable to locate the executable `node`.
```

다시 구글 검색을 수행하여 아래 내용을 보고 수행하였습니다.
```
https://stackoverflow.com/questions/61479644/invalid-podfile-file-unable-to-locate-the-executable-node
```

확인 후 node 설치
```
brew install node
```
다시 pod install 수행

```
$ pod install
```

하지만 다시 처음 메시지가 나오면서 실패하여 무언가 아직 난 부족하고나 하고 우선 창을 닫았습니다.
그냥 visual studio code에서 하는 방법을 찾아 진행하는게 맞는거 같습니다.

