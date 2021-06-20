---
layout: post
title:  "사양 낮은 서버에 torch 설치 시 자동 killed 될 때 설치 "
date:   2021-06-03 19:23:35 +0900
categories: 
- Linux
tags:
- python
- torch
- pororo
classes: wide
published: true
---

이번달에 리뷰를 위해 받은 도서의 뒷부분에 [pororo](https://github.com/kakaobrain/pororo)라는 NLP 관련 모듈이 소개 되어 있습니다. NLP 에 대한 것이라 테스트 하려고 기존에 설치해서 사용중인 오라클 클라우드에 설치를 하려는데 사양이 낮아서 그런지 torch를 설치시에 계속 killed 메시지를 내면서 종료가 되었습니다.

```
Collecting torch==1.6.0
  Downloading torch-1.6.0-cp36-cp36m-manylinux1_x86_64.whl (748.8 MB)
     |█████████████████████████████   | 676.8 MB 5.8 MB/s eta 0:00:13Killed
```

찾아보니 [옵션](https://askubuntu.com/questions/1271858/killed-during-installation-of-torch)을 주고 설치하면 된다 하여 설치 시 무리없이 설치가 되었습니다.

```
$ pip install torch==1.6.0 --no-cache-dir
Collecting torch==1.6.0
  Downloading torch-1.6.0-cp36-cp36m-manylinux1_x86_64.whl (748.8 MB)
     |████████████████████████████████| 748.8 MB 4.9 MB/s
Requirement already satisfied: numpy in /home/ubuntu/miniconda3/envs/torchenv/lib/python3.6/site-packages (from torch==1.6.0) (1.19.4)
Collecting future
  Downloading future-0.18.2.tar.gz (829 kB)
     |████████████████████████████████| 829 kB 6.4 MB/s
Building wheels for collected packages: future
  Building wheel for future (setup.py) ... done
  Created wheel for future: filename=future-0.18.2-py3-none-any.whl size=491059 sha256=f1d4254c9c31b43816e51156e796c82b23fcf95e9e34e7c339315aa49465cb46
  Stored in directory: /tmp/pip-ephem-wheel-cache-mv3ukb3b/wheels/6e/9c/ed/4499c9865ac1002697793e0ae05ba6be33553d098f3347fb94
Successfully built future
WARNING: Error parsing requirements for plotly: [Errno 2] No such file or directory: '/home/ubuntu/miniconda3/envs/torchenv/lib/python3.6/site-packages/plotly-4.14.1.dist-info/METADATA'
Installing collected packages: future, torch
Successfully installed future-0.18.2 torch-1.6.0
```

하지만, 결론은 실제 수행은 안되어 서버에 swap 을 설정하고 구동을 하였습니다.

[https://extrememanual.net/12975](https://extrememanual.net/12975)

기록을 위해 남겨둡니다.
