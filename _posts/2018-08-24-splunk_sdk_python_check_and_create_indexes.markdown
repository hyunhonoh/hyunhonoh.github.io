---
layout: post
title:  "Splunk sdk를 이용하여 인덱스 확인 및 생성"
date:   2018-08-24 21:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- sdk
- indexes
classes: wide
published: true
---

지난 포스트에 이어 sdk를 이용해서 데이터를 입력시 특정한 index를 지정 및 생성하는 방법을 잠시 얘기합니다.

인덱스를 지정해 주면 splunk에서는 데이터를 그 곳으로 보낼 수 있으며 인덱스가 없을 경우 생성도 가능합니다.

```python
# 연결
import splunklib.client as client

service = client.connect(host="localhost", port=8089, username="admin", password="shgusgh")
```

-  [참조 주소](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Indexes)



### index 가져오기

```python
userindex = service.indexes[indexname]
```



### index 생성하기

```python
indexesoptions = {'app':'AppName'}
userindex = service.indexes.create(indexname.lower(), **indexesoptions)
```

- 이렇게 생성을 하면 ``AppName``에 index 가 설정이 됩니다.