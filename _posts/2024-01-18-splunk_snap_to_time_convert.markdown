---
layout: post
title:  "Splunk snap_to time 기간 변경"
date:   2024-01-18 01:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- time
- token
- dashboard
classes: wide
published: true
---

Splunk 대시보드에서 시간선택기를 이용해서 원하는 시간의 범위를 지정할 수 있습니다.
해당 값을 변수로 받아 검색 범위(earliest, latest)에 사용을 할 때는 별 문제가 없습니다.

하지만, 그 시간값의 토큰으로 다른 곳에서 보여주려면 문제가 발생합니다.

일반적일 때는 epoch time으로 올 때는 strftime으로 변경을 할 수 있는데 그렇지 않는 경우 인 ```@y``` 형태로 오면 문자여서 인식을 할 수 없습니다.

그럴 때 활용하는 방법이 있어 링크 남깁니다.

```@y``` 형태를 snap-to time이라고 하는 것 같습니다.

[How to format "snap to time" to string in dashboard panel title.]([https://community.splunk.com/t5/Dashboards-Visualizations/How-to-remove-if-ALL-selection-is-selected-in-multiselect-based/m-p/496953#M32527](2021-12-17-splunk_dashboard_multiselect_token_all.markdown))


relative_time은 상대시간으로 문자형태를 받을 수 있어서 토큰으로 전달해 주고 변경을 한 다음에 원하는 형태로 사용을 하게 하는 방식입니다.

```xml
        <eval token="allCommandsEarliestLabel">
          if(isnum($command_token.earliest$),strftime($command_token.earliest$,"%d/%m/%Y %H:%M:%S"),strftime(relative_time(now(), $command_token.earliest$),"%m/%d/%Y %H:%M:%S"))
        </eval>
```


기록을 위해 남겨둡니다.