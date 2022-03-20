---
layout: post
title:  "mecab 설치 후 가상환경에서 libmecab.so.2 파일 에러 조치"
date:   2018-11-26 23:23:35 +0900
categories: 
- Linux
tags:
- python
- conda
- mecab
- TextMining
classes: wide
published: true
---



CentOS에 miniconda로 가상환경을 만들고 mecab를 설치후 libmecab.so.2 파일을 못 찾는 에러가 있어서 조치한 내용입니다.



#### 설치 환경

- CentOS 7.5

- Miniconda 3.7



위 환경을 설치 후 jupyternotebook 가상환경을 만든 후에 mecab를 설치했습니다.  mceab는 konlpy 에서 제공하는 설치 스크립트를 수행하고 마지막 mecab-python 모듈만  추가로 가상환경에서 설치했습니다.  명령어는 가상환경이 아닌 원래의 환경에서 수행했습니다.

```bash
# conda create -n jupyternotebook python==3.6.6
# bash <(curl -s https://raw.githubusercontent.com/konlpy/konlpy/master/scripts/mecab.sh)
```



설치 후 가상환경에서 명령어를 확인시에 아래와 같이 libmecab.so.2 파일을 찾을 수 없다는 메시지를 확인했습니다.

```bash
(jupyternotebooks) [root@ ~]# python
Python 3.6.6 |Anaconda, Inc.| (default, Oct  9 2018, 12:34:16)
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import MeCab
Traceback (most recent call last):
  File "/tmp/mecab-python-0.996/MeCab.py", line 18, in swig_import_helper
    fp, pathname, description = imp.find_module('_MeCab', [dirname(__file__)])
  File "/root/miniconda3/envs/jupyternotebooks/lib/python3.6/imp.py", line 297, in find_module
    raise ImportError(_ERR_MSG.format(name), name=name)
ImportError: No module named '_MeCab'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/tmp/mecab-python-0.996/MeCab.py", line 28, in <module>
    _MeCab = swig_import_helper()
  File "/tmp/mecab-python-0.996/MeCab.py", line 20, in swig_import_helper
    import _MeCab
ImportError: libmecab.so.2: cannot open shared object file: No such file or directory
>>>
```



조치 방법은 다음을 참조 했습니다.

[https://qiita.com/saicologic/items/ab70e14f7e2ec2ee0b4d](https://qiita.com/saicologic/items/ab70e14f7e2ec2ee0b4d)

```bash
# ls /usr/local/lib/libmecab.so.2
# vi /etc/ld.so.conf
```



/etc/ld.so.conf파일에 마지막 한 줄을 추가하고 다시 수행하면 정상적으로 표시가 됩니다.

- ld.so.conf 내용

```
include ld.so.conf.d/*.conf
/usr/local/lib
```