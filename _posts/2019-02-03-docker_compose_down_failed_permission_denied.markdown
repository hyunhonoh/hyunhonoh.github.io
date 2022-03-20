---
layout: post
title:  "docker compose 사용 시 stop 할 때 permission denied 처리 "
date:   2019-02-03 10:02:35 +0900
categories: 
- linux
- docker
tags:
- linux
- docker
- docker-compose
classes: wide
published: true
---



오랜만에 docker를 가지고 빌드 환경을 만들기 위해서 테스트를 해보다가 다음과 같은 에러가 발생하다가 정리가 안되는 현상이 있어 정리를 합니다.



docker 환경에서 apache2, django를 환경을 구현하는 것을 만들고 있는데 테스트 환경에서 docker-compose를 실행하니 에러가 나서 종료를 하려고 하니 다음과 같은 에러가 발생했습니다.



```bash
$ sudo docker-compose restart
[sudo] djangoapache의 암호:
Restarting djangoapache-docker ... error

ERROR: for djangoapache-docker  Cannot restart container fd5a350b5a60d9fb14858ea0b112dbdfd8f7f15e3fe48f3034f3526e1c30571a: Cannot kill container fd5a350b5a60d9fb14858ea0b112dbdfd8f7f15e3fe48f3034f3526e1c30571a: unknown error after kill: docker-runc did not terminate sucessfully: container_linux.go:393: signaling init process caused "permission denied"
: unknown
$ 
```



관리자 권한으로 실행을 해도 권한이 없다고 하면서 종료가 안되는 형태입니다. 

결국은 다음의 링크를 찾아서 수행해 보니 정상적으로 수행이 되었습니다.

https://stackoverflow.com/questions/50482315/docker-compose-down-fails-due-to-permission-denied



```bash
$ sudo killall docker-containerd-shim
$ docker-compose down
Removing djangoapache-docker ... done
Removing network djangoapachedocker_default
$
```



docker 문서를 다시 봐야겠네요.
