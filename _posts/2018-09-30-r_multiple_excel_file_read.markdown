---
layout: post
title:  "R 여러 파일 읽어서 하나로 묶기"
date:   2018-09-30 10:02:35 +0900
categories: 
- R
tags:
- study
- rbindlist
- analysts
classes: wide
published: true
---



프로젝트를 진행하면서 수많은(?) 엑셀을 읽어 들이는 문제를 가지게 되었습니다. 처음에 테스트를 할 때는 몇개의 파일이 안되어 하나씩 일었으나 기간이 길어지면서 하나씩하는데 어려움을 느끼다가 다음의 코드를 찾았습니다. 여기서는 30개 엑셀(개별 20메가)을 읽었습니다.



```R
# https://stackoverflow.com/questions/32888757/how-can-i-reading-multiple-excel-files-into-r

# 디렉토리를 순환하면서 파일명 가져오기
temp_file <- list.files(pattern='custfile.*\\.xlsx$', recursive = TRUE) 
temp_file # 파일명 리스트 확인

# 모두 한번에 불러오기
df_list <- lapply(temp_file, read_excel) # lapply를 이용해서 엑셀 읽기

# https://stackoverflow.com/questions/2851327/convert-a-list-of-data-frames-into-one-data-frame
df_list_ldply <- ldply(df_list, data.frame)
```



위와 같은 방법으로 여러개의 data.frame을 합치는데 ldply부분에서 약간의 속도 저하가 느껴졌습니다. 그래서 다시 방법을 찾아보니 rbindlist를 이용하면 제일 빠르다는 글을 확인했습니다. (쓰다가 확인해 보니 위 첫 링크에 rbindlist가 있었네요. )

[http://rfriend.tistory.com/225](http://rfriend.tistory.com/225)



```R
# 파일읽기
# install.packages("data.table")
library(data.table)
df_list_rbindlist <- rbindlist(df_list)
```



확실히 이 방법을 이용하니 기존보다는 조금 더 빠른 데이터를 읽어들이는 것으로 파악이 되었습니다. 

느낌상으로는 read_excel부분도 느린거 같은데 확인은 할 수 없었습니다.