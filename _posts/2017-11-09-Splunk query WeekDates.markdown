---
layout: post
title:  "Splunk 에서 쿼리를 이용한 주차에서 날짜 구하기"
date:   2017-11-09 22:55:35 +0900
categories: 
- Splunk
tags:
- Splunk
- 스플렁크
- 주차
- ISO8601
- 목요일
classes: wide
published: true
---


Splunk에서 내용을 검색할 때 보통은 기간을 지정해서 검색을 하지만 가끔 주차를 이용해서 기간을 정해야 할 때가 있습니다. 이럴 때 현재 날짜를 기준으로 계산해서 주차에 대한 월요일부터 일요일의 날짜를 구해서 전달해 줘야할 필요가 생기는데 이때 사용하려고 만든게 었었네요.
그런데 오랜만에 다시 찾아보니 ISO8601이라고 하는 국제 기준이 있고 첫 주의 시작은 1/1일이 목요일까지여야지 1주라고 합니다. 금, 토, 일에 1/1일이 있으면 그전년도의 마지막 주차라고 하는 내용을 보고 다시 쿼리를 수정해야 할 듯 한대 오래만에 보는 쿼리라 우선 기존 쿼리만 남겨 놓고 나중에 수정을 해야겠습니다.

[Wikipedia ISO8601 Week Dates](https://ko.wikipedia.org/wiki/ISO_8601#Week_Dates)

### ISO8601을 생각하지 않고 작성한 구문

```
index=_internal 
| head 1
| eval WEEKS=1 
| eval dYear=if((WEEKS=="01" AND date_month=="december"), date_year+1, date_year) 
| strcat dYear "0101" fDay 
| eval fDay=strptime(fDay, "%Y%m%d") 
| eval wDay=(WEEKS-1)*7*86400 
| eval fwDay=(tonumber(strftime(fDay,"%w")))*86400 
| eval earliest=strftime(fDay+wDay-fwDay, "%m/%d/%Y:00:00:00") 
| eval latest=strftime(fDay+wDay-fwDay+518400, "%m/%d/%Y:23:59:59") 
| stats count by earliest, latest 
| fields - count 
| return earliest, latest
```

### ISO8601 을 적용한 구문
  - Splunk의 strftime의 %V가 주차를 알 수 있음.

```
index=_internal
| head 1
| eval WEEKS=20
| strcat date_year "0101" fDay
| eval fDay = strptime(fDay, "%Y%m%d")
| eval fwDay = tonumber(strftime(fDay, "%u"))
| eval fwDayWeek = if(tonumber(strftime(fDay, "%V"))==1, 1, 0)
| eval fwDays = (fwDay - 1 ) * 86400
| eval wDay = (WEEKS - fwDayWeek) * 7 * 86400
| eval earliest = strftime(fDay + wDay - fwDays, "%m/%d/%Y:00:00:00")
| eval latest = strftime(fDay + wDay - fwDays + 604800, "%m/%d/%Y:00:00:00")
| table WEEKS, fDay, fwDay, fwDayWeek, fwDays, wDay, earliest, latest
```


```
WEEKS : 출력을 원하는 주차
fDay : 첫날의 시간을 구하기
fwDay : 첫날의 요일의 값 구하기(1-7) 7: 일요일
fwDayWeek : 첫날의 주가 1주인지 5x인지에 따라 변동값 구하기
fwDays : 현재 날짜부터 월요일 아침까지의 시간을 빼주기 위해서 처리
wDay : 0101부터 현재까지의 시간
```

![20171130_weekday.png](/images/20171130_weekday.png)

latest 는 21일까지 검색하기 위해서 22일 새벽 00시까지로 나오게 계산 되었습니다.

- 비교주차
  - 출처 : [https://kalender-365.de/calendar-ko.php](https://kalender-365.de/calendar-ko.php)

![20171130_weekday1.png](/images/20171130_weekday1.png)
