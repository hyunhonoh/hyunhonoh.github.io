---
layout: post
title:  "Splunk dashboard multiselec 토큰 변경"
date:   2021-12-20 01:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- token
- dashboard
classes: wide
published: true
---

Splunk 대시보드에 있는 멀티셀렉트는 여러값을 토큰으로 전달하는데 도움을 줍니다.
보통 기본값은 ALL로 되어 있고 그 다음에 다른 값을 선택합니다. 이 때 설정에 따라서 all이 선택이 되어 다른 값을 선택했을 때 지워주어야 정상적인 토큰이 생성되는 경우가 있습니다.

이럴 떄 다른 값이 선택이 되면 ALL 로 된 토큰을 제거해 주는 것을 찾아보니 2가지 정보로 확인이 됩니다.
하나는 javascript를 이용하여 토큰을 조작하는 것이고 하나는 change를 이용하여 토큰을 조작하는 형태입니다.
javascript를 이용하면 다양하게 할 수 있으나 수정이 약간 어렵고 토큰을 이용하는게 좀 더 편리해 보입니다.



[javascript 이용 링크](https://community.splunk.com/t5/Dashboards-Visualizations/How-to-remove-if-ALL-selection-is-selected-in-multiselect-based/m-p/496953#M32527)

[token변경 링크](https://community.splunk.com/t5/Dashboards-Visualizations/Change-amp-Condition-within-a-multiselect-with-token/m-p/553105/highlight/true#M38353)


기록을 위해 남겨둡니다.