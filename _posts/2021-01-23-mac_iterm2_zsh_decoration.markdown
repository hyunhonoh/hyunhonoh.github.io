---
layout: post
title:  "iterm2 와 zsh 터미널 환경 변경"
date:   2021-01-23 23:23:35 +0900
categories:
- mac
tags:
- iterm2
- zsh
- snazzy
- dracula
- mac
classes: wide
published: true
---

mac의 기본 쉘을 zsh로 되면서 이쁘게 할 수 있다는 글을 보고 변경하였습니다. 기존 bash 쉘보다는 표현할 수 있는 것도 많고 기능도 많은거 같습니다.

대략적인것으로 보면 다음과 같습니다. 
- 테마기능
- 경로자동완성
- 스펠링 검사
- git 정보

나머지 기능은 좀 더 사용하면서 확인해 보려고 합니다.

![zsh_theme](/images/20210123_zsh_01.png)

설치하기 위한 방법은 다음과 같습니다.

### oh-my-zsh 

```
# oh-my-zsh 설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```


### zsh theme 설정

- dracula theme : [https://draculatheme.com/zsh/](https://draculatheme.com/zsh/)
- 다운로드
- ```~/.oh-my-zsh/custom/themes```에 위치 

```bash
(base) ➜ ~ cat .zshrc | grep THEME
# to know which specific one was loaded, run: echo $RANDOM_THEME
#ZSH_THEME="robbyrussell"
ZSH_THEME="dracula"
# Setting this variable when ZSH_THEME=random will cause zsh to load
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )
(base) ➜ ~
```


### iterm2 설정

- iterm2 color schemes repo : [https://github.com/mbadolato/iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)
- 원하는 테마 다운로드






참고 

- [https://ohmyz.sh](https://ohmyz.sh)

- [https://tutorialpost.apptilus.com/code/posts/tools/mac-cli-with-iterm2-zsh/](https://tutorialpost.apptilus.com/code/posts/tools/mac-cli-with-iterm2-zsh/)

- [https://awesometic.tistory.com/115](https://awesometic.tistory.com/115)

- [https://github.com/hmml/awesome-zsh](https://github.com/hmml/awesome-zsh)
