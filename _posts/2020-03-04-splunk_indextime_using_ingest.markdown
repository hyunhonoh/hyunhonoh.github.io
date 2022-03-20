---
layout: post
title:  "source 에서 _time 시간 추출하기"
date:   2020-03-04 17:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- time
classes: wide
published: true
---

소스에서 날짜를 찾아주는 방법을 하는데 조건이 특이 했습니다. 년월일시까지만 있는 폴더인 상태에서 추출하는 방법이었는데 기존에 했던 방법으로 datetime.xml을 설정하고 입력을 했는데 계속 실패를 했습니다.

이것 저것 다 참조해서 변경하고 해 보았는데 계속 timepatterns를 못 찾는다는 에러를 내고 입력이 되지 않고 있었습니다. 시간을 계속 소비하다가 7.2 이후에 나온 INGEST라는 방법으로 처리하는 것을 발견하고 적용했더니 바로 적용이 되네요. 

props.conf
```
[mysourcetype]
TRANSFORMS=timestampeval
```

transforms.conf
```
[timestampeval]
INGEST_EVAL = _time=strptime(replace(source,".*(?=/)/",""),"Log_I15_%d%m%Y%H%M%S.txt")
```

[https://answers.splunk.com/answers/320978/how-to-extract-the-timestamp-from-a-filename-at-in.html](https://answers.splunk.com/answers/320978/how-to-extract-the-timestamp-from-a-filename-at-in.html)

[https://docs.splunk.com/Documentation/Splunk/latest/Data/IngestEval](https://docs.splunk.com/Documentation/Splunk/latest/Data/IngestEval)

기록을 위해 남겨둡니다.