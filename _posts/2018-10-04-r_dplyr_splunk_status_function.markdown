---
layout: post
title:  "R의 dplyr 과 Splunk Stats function 사용하기"
date:   2018-10-04 10:02:35 +0900
categories: 
- R
- Splunk
tags:
- study
- dplyr
- stats
classes: wide
published: true
---



R에는 dplyr이라는 멋진 도구가 있습니다. Splunk에는 stats, eventstats, streamstats라는 함수가 있습니다. 아래 내용은 제가 학습하면서 Splunk와 같이 설명 할 수 있을거 같아 작성한 내용입니다. 다른 부분이 있으면 댓글로 알려주시면 고맙겠습니다.





요즘 R을 하면서 dplyr의 mutate와 summrize가 Splunk의 eventstats와 stats랑 비슷하다는 것을 알게 되어서 너무 기분이 좋았습니다.

간단한 CSV가 있습니다. 이 예제를 이용해서 R로 예제를 보여드리고 Splun에서의 쿼리도 같이 보여드리겠습니다.

```
custid, date, qty, amt
c000001,2018-01-01,1,10000
c000001,2018-02-01,10,100000
c000002,2018-02-03,7,130000
c000003,2018-02-01,5,50000
c000003,2018-03-11,3,30000
c000004,2018-02-01,2,30000
c000001,2018-04-01,1,10000
```



위 자료를 가지고 다음과 같이 검색 및 활용할 수 있습니다. 먼저 tidyverse를 불러옵니다.

```R
library(tidyverse)
```



## mutate를 이용하여 eventstats와 비슷한 결과 보이기

```R
cust<-read_csv("custinfo.csv")

## Parsed with column specification:
## cols(
##   custid = col_character(),
##   date = col_date(format = ""),
##   qty = col_integer(),
##   amt = col_integer()
## )

cust

## # A tibble: 7 x 4
##   custid  date         qty    amt
##   <chr>   <date>     <int>  <int>
## 1 c000001 2018-01-01     1  10000
## 2 c000001 2018-02-01    10 100000
## 3 c000002 2018-02-03     7 130000
## 4 c000003 2018-02-01     5  50000
## 5 c000003 2018-03-11     3  30000
## 6 c000004 2018-02-01     2  30000
## 7 c000001 2018-04-01     1  10000

## 고객별 방문 수를 붙이기
cust <- cust %>%
  group_by(custid) %>%
  mutate(distinct_date = n_distinct(date))

### 고객 아이디마다 총 방문 회수가 붙습니다. 
glimpse(cust)

## Observations: 7
## Variables: 5
## $ custid        <chr> "c000001", "c000001", "c000002", "c000003", "c00...
## $ date          <date> 2018-01-01, 2018-02-01, 2018-02-03, 2018-02-01,...
## $ qty           <int> 1, 10, 7, 5, 3, 2, 1
## $ amt           <int> 10000, 100000, 130000, 50000, 30000, 30000, 10000
## $ distinct_date <int> 3, 3, 1, 2, 2, 1, 3
```



- Splunk로 하면 다음과 같이 사용할 수 있습니다.

    ```sql
    index=cust
    | eventstats dc(date) as distinct_date by custid
    | table custid, date, qty, amt
    ```



## summarise를 이용하여 stats와 비슷한 결과 보이기

```R
## 고객별 전체 구매(amt) 합계 구하기
cust <- cust %>%
  group_by(custid) %>%
  summarise(amt_total=sum(amt))

### 고객 아이디별 총합이 붙습니다.
glimpse(cust)

## Observations: 4
## Variables: 2
## $ custid    <chr> "c000001", "c000002", "c000003", "c000004"
## $ amt_total <int> 120000, 130000, 80000, 30000
```


- Splunk로 하면 다음과 같이 사용할 수 있습니다.

  ```sql
  index=cust
  | stats sum(amt) as amt_total by custid
  | table custid, amt_total
  ```



두개를 같이 활용해 보면서 결과값을 도출하는 방법을 이해하는데 많은 도움이 되었습니다. Splunk의 stats 함수를 처음에 이해하는데 어려움이 있고 힘이 들었었는데, R에서 비슷한 개념을 있고 쉽게 이해가 되는게 도움이 되네요.

처음에 R을 배울 때는 통계학적인 내용(검정에 대한)이 주를 이루어서 이해하기 어렵고 했는데 RFM을 수행하면서 데이터를 정제하는 부분은 어렵지 않게 이해가 되어서 진행하는데 어려움이 없었습니다. 