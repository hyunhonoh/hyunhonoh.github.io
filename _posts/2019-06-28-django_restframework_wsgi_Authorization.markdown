---
layout: post
title:  "django rest framework에서 인증 설정 시 apache추가 사항 "
date:   2019-06-28 20:23:35 +0900
categories: 
- web
tags:
- django
- python
classes: wide
published: true
---



django 의 rest framework를 이용해서 restapi를 작성하고 있습니다.
TokenAuthentication 으로 설정 후 계정 토큰도 발행하고 로컬 개발환경에서 설정시에도 잘 되어서 apache로 구성된 환경에 옯겼더니 다음과 같은 에러가 나는 걸 확인했습니다.

```bash
$ curl -X GET -H 'Authorization: Token 5a12a3c79827fe7c0df3231fbe49ccf19a308200' -i 'http://127.0.0.1/api/v1/apidemo/?format=json&searchword=%ED%95%9C%EA%B8%80'
HTTP/1.1 401 Unauthorized
Date: Fri, 28 Jun 2019 00:13:45 GMT
Server: Apache/2.4.29 (Ubuntu)
WWW-Authenticate: Basic realm="api"
Content-Length: 96
Vary: Accept,Cookie
Allow: GET, HEAD, OPTIONS
X-Frame-Options: SAMEORIGIN
Content-Type: application/json

{"detail":"자격 인증데이터(authentication credentials)가 제공되지 않았습니다."}
$
```

찾아보니 메뉴얼을 제대로 안봐서 못 보고 지나친 부분이었네요.

[https://www.django-rest-framework.org/api-guide/authentication/#apache-mod_wsgi-specific-configuration](https://www.django-rest-framework.org/api-guide/authentication/#apache-mod_wsgi-specific-configuration)

아래와 같은 옵션을 추가해 주어야 합니다.

```
# this can go in either server config, virtual host, directory or .htaccess
WSGIPassAuthorization On
```

