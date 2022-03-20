---
layout: post
title:  "월간 차이 계산하기"
date:   2020-08-26 23:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- time
classes: wide
published: true
---

두 날짜가 있고 몇개월인지 구하는 식입니다. 
어떻게 할까 고민하고 있었는데 수식(?)이 있었습니다.

처음에 찾은것은 아래와 같았습니다.
https://stackoverflow.com/questions/2536379/difference-in-months-between-two-dates-in-javascript


```
function monthDiff(d1, d2) {
    var months;
    months = (d2.getFullYear() - d1.getFullYear()) * 12;
    months -= d1.getMonth();
    months += d2.getMonth();
    return months <= 0 ? 0 : months;
}
```

다시 찾아보니 다음이 깔끔하네요.
[https://community.qlik.com/t5/QlikView-Creating-Analytics/How-Do-I-Get-the-Number-of-Months-Between-Two-Dates/td-p/296928](https://community.qlik.com/t5/QlikView-Creating-Analytics/How-Do-I-Get-the-Number-of-Months-Between-Two-Dates/td-p/296928)

```
=((year(today(2))*12)+month(today(2))) - (((year([Set up date])*12)+month([Set up date])))
```

결론은 첫번째에도 두번째와 비슷한게 댓글로 있었네요.

스플렁크에서는 다음과 같이 계산이 됩니다

```
| makeresults
| eval one = "2020-01-01"
| eval two = "2020-07-01"
| eval one_year = strftime(strptime(one, "%Y-%m-%d"), "%Y")
| eval one_month = strftime(strptime(one, "%Y-%m-%d"), "%m")
| eval two_year = strftime(strptime(two, "%Y-%m-%d"), "%Y")
| eval two_month = strftime(strptime(two, "%Y-%m-%d"), "%m")
| eval monthdiff = ((two_year*12)+two_month) - ((one_year*12)+one_month)
```



기록을 위해 남겨둡니다.