---
layout: post
title:  "Django proxy를 이용하여 Splunk SSO 구현하기"
date:   2018-07-20 22:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- proxy sso
- django
classes: wide
published: true
---


Splunk는 SSO 로그인을 지원합니다. 기본적으로 네이티브 로그인 방식이 있고 다음과 같은 방식으로 SSO를 제공합니다.

- http://docs.splunk.com/Documentation/Splunk/7.1.1/Security/Hardeningstandards#Set_up_authenticated_users_and_manage_user_access

```
Use one of the following to to create secure one-step login for users:
- Single Sign on with SAML
- Multi-factor authentication
- ProxySSO
- Reverse-proxy SSO with Apache
```

여기서 얘기하는 내용은 마지막 부분의 Reverse-proxy SSO with Apache부분입니다. 물론 Apache를 이용하진 않고 기존에 Django로 만들어진 app 환경에서 다른 proxy전용의 app을 구동하고 연동하는 방법입니다.

기본 Reverse-Proxy를 이용한 방법은 아래와 같이 구성이 된다고 합니다.

![](http://docs.splunk.com/images/7/77/SSOProcess.png)

아파치를 이용해서 하는 방법이 기본으로 설정되어 있는데 이 경우 Splunk 에 Header를 전달해 주기 위해서는 별도의 프로그램 과정이 필요합니다. 이부분에서 많은 어려움이 있어서 실제로 적용하는데는 문제점이 있습니다.

예제에서는 Python WebFramework 인 Django를 이용해서 reverse proxy를 구현하고 sso를 수행하는 내용에 대해서 설명하려고 합니다.

## 순서

### 가상환경 설정
- 여기서는 django 1.9를 설치

```bash
conda create --name splproxy django=1.9
source activate splproxy
```

### Django app 만들기

```bash
django-admin startproject splproxy
```

### Django reverse proxy 설치
- http://django-revproxy.readthedocs.io/en/latest

```bash
pip install django-revproxy
```

- views.py 예

```python
from revproxy.views import ProxyView
from django.contrib import auth

class CustomProxyView(ProxyView):
    upstream = 'http://example.com:8000'

    def get_request_headers(self):
        # Call super to get default headers
        headers = super(CustomProxyView, self).get_request_headers()
        # Add new header. Splunk에 전달해 주기 위한 헤더값을 지정. 로그인한 사용자의 정보를 가져와서 전달.
        # headers['REMOTE_USER'] = str(auth.get_user(self.request))
        headers['REMOTE_USER'] = 'admin'
        return headers
```

- urls.py 예

```python
from django.conf.urls import url
from .views import CustomProxyView

urlpatterns = [
    url(r'^(?P<path>.*)$', CustomProxyView.as_view(add_remote_user=True)),

]
```


### session 공유 설정
- 이 예제는 django앱으로 접속 로그인 후 splunk 로 접근이라 login session 공유를 위해 다음을 추가

```python
SESSION_COOKIE_DOMAIN = ".<example.com>"
SESSION_COOKIE_NAME = "hyunhonohproxykey"
```

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

이렇게 설정 후 django app을 임의의 포트 (ex:9000)으로 설정 후 접속을 하면 splunk가 자동으로 열립니다.
