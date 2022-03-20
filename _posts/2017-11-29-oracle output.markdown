---
layout: post
title:  "Custom command를 이용하여 oracle output 하기"
date:   2017-11-30 21:55:00 +0900
categories: 
- Splunk
tags:
- splunk
- 스플렁크
- oracle
- dboutput
- db
classes: wide
published: true
---

# Python module을 이용한 oracle insert 명령어를 만들기

## 결과

```
index=_internal | head 10
| eval TIME=strftime(_time, "%Y/%m/%d %H:%M:%S") | table TIME, action, method
| dbout type="sql" "insert into DBX (DTM, ACTIONS, METHODS) values(TO_TIMESTAMP($TIME$, 'YYYY-MM-DD HH24:MI:SS'), $action$, $method$)"
```

![명령어 예제](/images/20160519_001.png)

![입력후 결과 확인](/images/20160519_002.png)



- [github link](https://github.com/hyunhonoh/dbout-custom-command)
