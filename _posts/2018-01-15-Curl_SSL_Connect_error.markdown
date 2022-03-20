---
layout: post
title:  "curl SSL Connect error"
date:   2018-01-15 23:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- ssl
- 스플렁크
- curl
- 7.19.7
classes: wide
published: true
---

## curl을 이용시 접속 에러 처리.

curl 명령어를 이용해서 https를 접근할 때가 있습니다. 보통은 -k로만 설명이 되어 있습니다.
스플렁크에서도 Rest 를 수행할 때 curl을 사용합니다. 그런데 보통은 -k만으로도 되는데 처음으로 아래와 같은 에러가 나면서 접속이 되지 않았습니다.

```bash
$ curl -k -vvv -u admin:xxxx https://10.0.0.10:9089
* About to connect() to 10.0.0.10 port 9089 (#0)
*   Trying 10.0.0.10... connected
* Connected to 10.0.0.10 (10.0.0.10) port 9089 (#0)
* Initializing NSS with certpath: sql:/etc/pki/nssdb
* warning: ignoring value of ssl.verifyhost
* NSS error -12190
* Error in TLS handshake, trying SSLv3...
* Server auth using Basic with user 'admin'
> GET / HTTP/1.1
> Authorization: Basic 
> User-Agent: curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2
> Host: 10.0.0.10:9089
> Accept: */*
> 
* Connection died, retrying a fresh connect
* Closing connection #0
* Issue another request to this URL: 'https://10.0.0.10:9089'
* About to connect() to 10.0.0.10 port 9089 (#0)
*   Trying 10.0.0.10... connected
* Connected to 10.0.0.10 (10.0.0.10) port 9089 (#0)
* TLS disabled due to previous handshake failure
* warning: ignoring value of ssl.verifyhost
* NSS error -12286
* Closing connection #0
* SSL connect error
curl: (35) SSL connect error
$ curl --version
curl 7.19.7 
```

추가 검색을 해보니 아래와 같은 해결 방안이 있었습니다.

- [https://github.com/userify/shim/issues/25](https://github.com/userify/shim/issues/25)
	```
	I fixed the install by add -1 force TLS.
	```
- 주소에 추가적으로 ```-1```을 넣어주면 된다고 합니다.
	```bash
	$ curl -k -vvv -1 -u admin:xxxx https://10.0.0.10:9089
	```
