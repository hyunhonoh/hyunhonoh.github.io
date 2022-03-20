---
layout: post
title:  "Splunk 검색 결과 내려받기 한글 깨짐 수정"
date:   2021-01-26 23:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- csv
- 한글
classes: wide
published: true
---

Splunk 검색 결과 후에 내려받기를 눌러서 저장한 csv를 바로 엑셀에서 읽으면 한글이 깨집니다.
이는 한글 excel에서는 바로 utf-8형식의 글을 읽을 때 인식을 제대로 못하는 것으로 알고 있습니다.

그래서 다운 받는 csv 데이터에 BOM을 추가해 주면 utf-8문서로 인식하고 정상적으로 읽어지면서 한글이 깨지지 않게 됩니다.
물론 일반 텍스트 에디터로 보면 앞부분에 BOM문자가 표시되긴 하지만 바로 엑셀에서 읽으면 문제는 없습니다.

[https://community.splunk.com/t5/Getting-Data-In/how-to-export-csv-with-BOM/m-p/198357](https://community.splunk.com/t5/Getting-Data-In/how-to-export-csv-with-BOM/m-p/198357)

- ```splunkhome/lib/python2.7/site-packages/splunk/rest/__init__.py``` 의 파일을 수정합니다.

- 수정 후

```python
    def readall(self, blocksize=32768):
        """
        Returns a generator reading blocks of data from the response
        until all data has been read
        """
        response = self.response
        import codecs
        counter = 0;
        while 1:
            data = response.read(blocksize)
            if not data:
                break
            if counter == 0:
                data = "".join((codecs.BOM_UTF8, data))
                counter += 1
            yield data
```


해당 코멘트에 추가로 변경된 파이선 3버전의 수정 사항도 있습니다.

- 3 버전 코드 (앞부분에 b를 추가)
```python
    def readall(self, blocksize=32768):
        """
        Returns a generator reading blocks of data from the response
        until all data has been read
        """
        response = self.response
        import codecs
        counter = 0;
        while 1:
            data = response.read(blocksize)
            if not data:
                break
            if counter == 0:
                data = b"".join((codecs.BOM_UTF8, data))
                counter += 1
            yield data
```

기록을 위해 남겨둡니다.