---
layout: post
title:  "R 에서 시간 문자열을 시간 형식으로 변경하기"
date:   2018-09-29 23:25:35 +0900
categories: 
- R
tags:
- datetime
- data
- analysts
classes: wide
published: true
---



RFM 작업을 하면서 문자열로 된 날짜를 날짜 형식으로 변경하는 방법을 하다가 찾은 내용입니다. 기본적으로는 다음의 방법으로 날짜를 변경했습니다.



```R
cust_data$date <- as.POSIXct(as.character(cust_data$date), 
                                   format="%Y%m%d") # 시간형으로 변환
```

이 방법으로 하면 원 데이터가 ```20180923```형태의 데이터를 날짜형인 POSIXct 로 변경을 해 줍니다. 그 다음에 년, 월, 일을 따로 저장하려면 다음과 같은 개별 작업을 또다시 수행합니다.



```R
## 연변수 추가 
cust_data$year <- format(cust_data$date, '%Y') 
# 
## 월변수 추가 
cust_data$month <- format(cust_data$date, '%m') 
# 
## 일변수 추가 
cust_data$day <- format(cust_data$date, '%d') 
```



비효율적이라 다른 방법이 있는지 찾아보니 lubridate라는 패키지가 있었습니다. 아래와 같이 한번에 날짜 형식을 인식 할 수도 있고 년, 월, 일을 나눠서 처리할 수 있었습니다.

요일은 기본으로 한글인 "일", "월",.. 형식으로 저장되는데 로케일을 변경해 주면 "Sun", "Mon",…형식으로 저장 할 수 있습니다. 

```R
# 영어로 로케일 설정
# Sys.setlocale("LC_TIME", "English_United States.1252") # Windows
Sys.setlocale("LC_TIME", "en_US.UTF-8") # Mac
# 날짜를 한번에 년월일 나누기 # https://cran.r-project.org/web/packages/lubridate/vignettes/lubridate.html
# cust_data$data 20180929
cust_data = cust_data %>%
  dplyr::mutate(date = lubridate::ymd(date)) %>% 
  dplyr::mutate(year = lubridate::year(date), 
                month = lubridate::month(date), 
                day = lubridate::day(date),
                weekday = lubridate::wday(date, label = TRUE))
glimpse(cust_data)

# 한국어로 재설정
# Sys.setlocale("LC_TIME", "Korean_Korea.949") # Windows
Sys.setlocale("LC_TIME", "ko_KR.UTF-8") # Mac
```
