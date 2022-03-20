---
layout: post
title:  "github ssh key 등록"
date:   2021-01-12 23:23:35 +0900
categories:
- git
tags:
- ssh
- github
- push
classes: wide
published: true
---

만들고 있는 스크립트가 있고 서버와 동기화를 git을 이용하려고 합니다. 

기존에 만들어둔 oracle cloud에서 작업을 하는데 vscode remote는 자원을 많이 써서 그런지 서버가 쉽게 다운이 되었습니다.

어쩔 수 없이 cli에서 작업하려고 하는데 계속 아이디/패스워드 요청 창이 나와서 이번에 추가를 해 보았습니다.


#### ssh setting 메뉴얼

https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh


#### ssh key 새로 만들기
https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

#### ssh key 등록하기
https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account


#### git 설정 변경
ssh키를 등록하고 git pull을 해도 지속적으로 username/password를 확인하는 입력창이 나와서 찾아보니 재설정하거나 해야 하는데 이미 사용하고 있어서 지울 수 없는 상태에서 찾아보니 다른 방법이 있었습니다.

- https://stackoverflow.com/questions/9960897/why-doesnt-my-ssh-key-work-for-connecting-to-github


.git/config 에 있는 repo 주소를 변경하여 설정 변경

```
    url = ssh://git@github.com/path/to/repository
```

그런데 늘 자동으로 되어서 그런지 로컬은 이해가 되는데 remote의 vscode는 어떻게 자동으로 올려주었는지 모르겠습니다.
