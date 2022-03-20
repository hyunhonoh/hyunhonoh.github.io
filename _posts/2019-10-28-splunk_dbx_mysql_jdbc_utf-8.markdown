---
layout: post
title:  "Splunk DBX 3.1.x mysql 출력시 한글 깨짐"
date:   2019-10-28 23:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- mysql
- dbx
- utf-8
classes: wide
published: true
---

DBX 를 이용해서 dbxoutput을 테스트하게 되었습니다. 데이터를 그냥 출력만 하는 것이고 mysql의 설정도 utf-8로 맞추었다고 해서 출력을 했는데 뭔가 이상합니다.
자세히 보니 한글이 깨져있는 상태였습니다. 다시 utf-8설정인지 확인했으나 정상이라고 하고 한글은 깨지고 해서 찾아보니 여러가지 내용이 나오긴 했습니다.
하지만 간단하게 아래의 내용을 보고  수동으로 jdbc 설정을 변경하고 적용할 수 있었습니다.

[참고링크](https://tost.tistory.com/132)

다음과 같이 연결에 옵션을 주고 넣으니 정상적으로 한글이 입력되었습니다.

```
jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8
```


- 수정전
![mysql_dbx_utf-8_1.png](/images/mysql_dbx_utf-8_1.png)

- 수정후
![mysql_dbx_utf-8_2.png](/images/mysql_dbx_utf-8_2.png)



- 추가 

BatchInsert를 위해서 아래를 추가하면 됩니다.

```
rewriteBatchedStatements=true 
```