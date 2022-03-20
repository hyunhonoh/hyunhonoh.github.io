---
layout: post
title:  "Nodejs proxy를 이용하여 Splunk SSO 구현 및 테스트 하기"
date:   2020-01-23 22:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- proxy sso
- nodejs
classes: wide
published: true
---



기존에 작성했던 [Django proxy를 이용하여 Splunk SSO 구현하기](https://hyunhonoh.github.io/splunk/implement_splunk_sso_using_django_proxy/)와 비슷한 내용입니다.


SSO를 설정하고 나오는 에러 메시지를 테스트를 해야 하는데 개발서버에 apache를 추가로 설치할 수 없고, django를 설치하기도 애매하고 성능보다 메시지가 나오게 하는 것이 중요한 상황이었습니다. 예전에 테스트 했던 nodejs 코드를 이용해서 간단하게 구현했던 것이 있었는데 
참고용으로 남겨둡니다. 
Splunk 의 설정은 다른 부분은 없고 앞단만 nodejs 를 이용해서 간단하게 설정하는 내용입니다.


### Splunk 설정
- web.conf

```bash
[settings]
SSOMode = strict
trustedIP = 127.0.0.1
remoteUser = REMOTE_USER
tools.proxy.on = False
```

- server.conf

```
[general]
trustedIP = 127.0.0.1
```


### nodejs proxy server
- git : [nodejs Proxy Server](https://github.com/nodejitsu/node-http-proxy)
- install
```
npm install http-proxy --save
```

- ssoproxy.js
- 아이디를 admin으로 지정해서 보내는 예제
```js
var httpProxy = require('http-proxy');
// Splunk의 서버 ip:port 지정
var proxy = httpProxy.createProxyServer({target:'http://localhost:8000'});
// To modify the proxy connection before data is sent, you can listen
// for the 'proxyReq' event. When the event is fired, you will receive
// the following arguments:
// (http.ClientRequest proxyReq, http.IncomingMessage req,
//  http.ServerResponse res, Object options). This mechanism is useful when
// you need to modify the proxy request before the proxy connection
// is made to the target.
// 
// 자동로그인을 수행할 사용자의 아이디를 X_Remote_User로 보낸다.
proxy.on('proxyReq', function(proxyReq, req, res, options) {
  proxyReq.setHeader('X_Remote_User', 'admin');
});
console.log("listening on port 8008")
proxy.listen(8008);
```

- run
```
node ssoproxy.js
```

이렇게 설정 후 8008 포트로 접속을 하면 splunk가 자동으로 열립니다. 아이디를 다른 이름으로 변경해 주면 스플렁크에 없는 사용자여서 에러가 표시가 됩니다.
