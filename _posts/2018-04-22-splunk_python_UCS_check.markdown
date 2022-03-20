---
layout: post
title:  "Splunk Python UCS 확인하기"
date:   2018-04-22 13:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- python
- UCS2
- UCS4
classes: wide
published: true
---

Splunk에서 커스텀 명령어를 만들 수 있습니다. splunk python sdk를 이용하거나 splunk search에 있는 명령어를 참고해서 만들 수 있습니다. 

명령어에 대한 예제는 http://dev.splunk.com/view/python-sdk/SP-CAAAEU2 에서 더 많은 내용을 볼 수 있습니다.

이 때 추가적인 모듈이 필요한 경우가 있는데 이럴 때 pip 등을 이용해서 추가적으로 받은 모듈을 사용할 수 있습니다. 예를 들어 DB에 연동을 하거나 할 때 oracle 연결 모듈이나 mysql 에 연동할 때 필요한 모듈을 받아서 사용할 수 있습니다.



하지만 아래와 같은 문제를 만날 수 있습니다.

```shell
root@ubuntu:/opt/splunk/etc/apps/decissiontree/bin# /opt/splunk/bin/splunk cmd python export.py 
Traceback (most recent call last):
  File "export.py", line 5, in <module>
    from sklearn.tree import _tree
  File "/opt/splunk/etc/apps/decissiontree/bin/sklearn/__init__.py", line 31, in <module>
    from . import __check_build
  File "/opt/splunk/etc/apps/decissiontree/bin/sklearn/__check_build/__init__.py", line 46, in <module>
    raise_build_error(e)
  File "/opt/splunk/etc/apps/decissiontree/bin/sklearn/__check_build/__init__.py", line 41, in raise_build_error
    %s""" % (e, local_dir, ''.join(dir_content).strip(), msg))
ImportError: /opt/splunk/etc/apps/decissiontree/bin/sklearn/__check_build/_check_build.so: undefined symbol: PyUnicodeUCS4_DecodeUTF8
```



이는 Splunk에 설치된 python과 다운 받은 python의 UCS 값이 달라서 나오는 문제입니다.

- Splunk python - UCS2

```python
$ ./splunk cmd python
Python 2.7.13 (default, Nov 14 2017, 11:43:23)
[GCC 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.maxunicode)
65535
>>> exit()
```

- System python - UCS4

```python
$ python
Python 3.6.3 |Anaconda custom (64-bit)| (default, Oct  6 2017, 12:04:38)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.maxunicode)
1114111
>>> exit()
```



위 두 개를 비교해 보면 maxunicode의 값이 다릅니다. UCS가 달라서 발생하는 문제입니다. 그래서 Splunk에서 사용할 모듈을 추가로 받았을 때 이 문제로 실행이 안되는 경우가 발생합니다. 대표적으로 Jpype1등의 패키지가 그렇습니다.

Python을 소스로 받아서 설치 할 때 ```--enable-unicode``` 옵션을 이용하여 원하는 형태로 설치가 가능합니다.

- ucs4

```bash
$ ./configure --enable-unicode=ucs4 --with-ssl
$ make
$ make install
```



- ucs2

```bash
$ ./configure --enable-unicode=ucs2 --with-ssl
$ make
$ make install
```

