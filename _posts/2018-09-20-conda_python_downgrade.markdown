---
layout: post
title:  "conda 환경에서 python 3.7 to 3.6 downgrade"
date:   2018-09-20 13:23:35 +0900
categories: 
- Linux
tags:
- python
- conda
- tensorflow
classes: wide
published: true
---



시스템 문제로 초기화를 한 다음에 conda 가상환경에서 tensorflow를 새로 설치하려고 하는데 다음과 같은 메시지가 나옵니다.

기존에는 잘 되었는데 안되서 약간 멘붕이 오네요....
그래서 python 버전을 보니 3.7입니다.  miniconda를 설치하고 작업을 하다보니 에러가 떠서 확인을 해보니 아직 3.7.0에는 tensorflow가 안나온거 같습니다. 그래서 다음으로 3.6으로 다시 설치하고 작업을 이어 나갔습니다.



- 추가 20181117 : 명령어만 따로 적어봅니다.

```shell
(jupyternotebook) [hhnoh@localhost notebooks]$ conda install python==3.6
```

그런데 이걸 하면 일부 패키지는 삭제되는지 다시 설치를 해야 하네요.





- 전체 내용





```bash
(jupyternotebook) [hhnoh@localhost notebooks]$ pip install tensorflow
Collecting tensorflow
  Could not find a version that satisfies the requirement tensorflow (from versions: )
No matching distribution found for tensorflow
(jupyternotebook) [hhnoh@localhost notebooks]$ python -V
Python 3.7.0

(jupyternotebook) [hhnoh@localhost notebooks]$ conda install python==3.6
Fetching package metadata ...........
Solving package specifications: .

Package plan for installation in environment /home/hhnoh/miniconda3/envs/jupyternotebook:

The following NEW packages will be INSTALLED:

    libgcc:             7.2.0-h69d50b8_2
    libiconv:           1.14-0

The following packages will be UPDATED:

    appdirs:            1.4.3-py37h28b3542_0   --> 1.4.3-py36h28b3542_0
    asn1crypto:         0.24.0-py37_0          --> 0.24.0-py36_0
    attrs:              18.2.0-py37h28b3542_0  --> 18.2.0-py36h28b3542_0
    automat:            0.7.0-py37_0           --> 0.7.0-py36_0
    backcall:           0.1.0-py37_0           --> 0.1.0-py36_0
    bleach:             2.1.4-py37_0           --> 2.1.4-py36_0
    certifi:            2018.8.24-py37_1       --> 2018.8.24-py36_1
    cffi:               1.11.5-py37he75722e_1  --> 1.11.5-py36he75722e_1
    constantly:         15.1.0-py37h28b3542_0  --> 15.1.0-py36h28b3542_0
    cryptography:       2.3.1-py37hc365091_0   --> 2.3.1-py36hc365091_0
    decorator:          4.3.0-py37_0           --> 4.3.0-py36_0
    entrypoints:        0.2.3-py37_2           --> 0.2.3-py36_2
    html5lib:           1.0.1-py37_0           --> 1.0.1-py36_0
    hyperlink:          18.0.0-py37_0          --> 18.0.0-py36_0
    idna:               2.7-py37_0             --> 2.7-py36_0
    incremental:        17.5.0-py37_0          --> 17.5.0-py36_0
    ipykernel:          4.9.0-py37_1           --> 4.9.0-py36_1
    ipython:            6.5.0-py37_0           --> 6.5.0-py36_0
    ipython_genutils:   0.2.0-py37_0           --> 0.2.0-py36_0
    ipywidgets:         7.4.1-py37_0           --> 7.4.1-py36_0
    jedi:               0.12.1-py37_0          --> 0.12.1-py36_0
    jinja2:             2.10-py37_0            --> 2.10-py36_0
    jsonschema:         2.6.0-py37_0           --> 2.6.0-py36_0
    jupyter:            1.0.0-py37_6           --> 1.0.0-py36_6
    jupyter_client:     5.2.3-py37_0           --> 5.2.3-py36_0
    jupyter_console:    5.2.0-py37_1           --> 5.2.0-py36_1
    jupyter_core:       4.4.0-py37_0           --> 4.4.0-py36_0
    markupsafe:         1.0-py37h14c3975_1     --> 1.0-py36h14c3975_1
    mistune:            0.8.3-py37h14c3975_1   --> 0.8.3-py36h14c3975_1
    nbconvert:          5.3.1-py37_0           --> 5.3.1-py36_0
    nbformat:           4.4.0-py37_0           --> 4.4.0-py36_0
    notebook:           5.6.0-py37_0           --> 5.6.0-py36_0
    pandocfilters:      1.4.2-py37_1           --> 1.4.2-py36_1
    parso:              0.3.1-py37_0           --> 0.3.1-py36_0
    pexpect:            4.6.0-py37_0           --> 4.6.0-py36_0
    pickleshare:        0.7.4-py37_0           --> 0.7.4-py36_0
    pip:                10.0.1-py37_0          --> 10.0.1-py36_0
    prometheus_client:  0.3.1-py37h28b3542_0   --> 0.3.1-py36h28b3542_0
    prompt_toolkit:     1.0.15-py37_0          --> 1.0.15-py36_0
    ptyprocess:         0.6.0-py37_0           --> 0.6.0-py36_0
    pyasn1:             0.4.4-py37h28b3542_0   --> 0.4.4-py36h28b3542_0
    pyasn1-modules:     0.2.2-py37_0           --> 0.2.2-py36_0
    pycparser:          2.18-py37_1            --> 2.18-py36_1
    pygments:           2.2.0-py37_0           --> 2.2.0-py36_0
    pyopenssl:          18.0.0-py37_0          --> 18.0.0-py36_0
    python-dateutil:    2.7.3-py37_0           --> 2.7.3-py36_0
    pyzmq:              17.1.2-py37h14c3975_0  --> 17.1.2-py36h14c3975_0
    send2trash:         1.5.0-py37_0           --> 1.5.0-py36_0
    service_identity:   17.0.0-py37h28b3542_0  --> 17.0.0-py36h28b3542_0
    setuptools:         40.2.0-py37_0          --> 40.2.0-py36_0
    simplegeneric:      0.8.1-py37_2           --> 0.8.1-py36_2
    six:                1.11.0-py37_1          --> 1.11.0-py36_1
    terminado:          0.8.1-py37_1           --> 0.8.1-py36_1
    testpath:           0.3.1-py37_0           --> 0.3.1-py36_0
    tornado:            5.1-py37h14c3975_0     --> 5.1-py36h14c3975_0
    traitlets:          4.3.2-py37_0           --> 4.3.2-py36_0
    twisted:            18.7.0-py37h14c3975_1  --> 18.7.0-py36h14c3975_1
    wcwidth:            0.1.7-py37_0           --> 0.1.7-py36_0
    webencodings:       0.5.1-py37_1           --> 0.5.1-py36_1
    wheel:              0.31.1-py37_0          --> 0.31.1-py36_0
    widgetsnbextension: 3.4.1-py37_0           --> 3.4.1-py36_0
    zope:               1.0-py37_1             --> 1.0-py36_1
    zope.interface:     4.5.0-py37h14c3975_0   --> 4.5.0-py36h14c3975_0

The following packages will be DOWNGRADED:

    fontconfig:         2.13.0-h9420a91_0      --> 2.12.1-3
    freetype:           2.9.1-h8a8886c_1       --> 2.5.5-2
    icu:                58.2-h9c2bf20_1        --> 54.1-0
    libxml2:            2.9.8-h26e45fe_1       --> 2.9.4-0
    pyqt:               5.9.2-py37h22d08a2_1   --> 5.6.0-py36h22d08a2_6
    python:             3.7.0-hc3d631a_0       --> 3.6.0-0
    qt:                 5.9.6-h8703b6f_2       --> 5.6.2-5
    qtconsole:          4.4.1-py37_0           --> 4.3.1-py36_0
    readline:           7.0-h7b6447c_5         --> 6.2-2
    sip:                4.19.12-py37he6710b0_0 --> 4.18.1-py36hf484d3e_2
    sqlite:             3.24.0-h84994c4_0      --> 3.13.0-0
    tk:                 8.6.8-hbc83047_0       --> 8.5.18-0

Proceed ([y]/n)?
```


정상적으로 설치가 되네요.

```bash
(jupyternotebook) [hhnoh@localhost notebooks]$ pip install tensorflow
Collecting tensorflow
  Downloading https://files.pythonhosted.org/packages/04/7e/a484776c73b1431f2b077e13801531e966113492552194fe721e6ef88d5d/tensorflow-1.10.1-cp36-cp36m-manylinux1_x86_64.whl (58.4MB)
    100% |████████████████████████████████| 58.4MB 626kB/s
Collecting astor>=0.6.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/35/6b/11530768cac581a12952a2aad00e1526b89d242d0b9f59534ef6e6a1752f/astor-0.7.1-py2.py3-none-any.whl
Collecting absl-py>=0.1.6 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/16/db/cce5331638138c178dd1d5fb69f3f55eb3787a12efd9177177ae203e847f/absl-py-0.5.0.tar.gz (90kB)
    100% |████████████████████████████████| 92kB 13.0MB/s
Collecting grpcio>=1.8.6 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/a7/9c/523fec4e50cd4de5effeade9fab6c1da32e7e1d72372e8e514274ffb6509/grpcio-1.15.0-cp36-cp36m-manylinux1_x86_64.whl (9.5MB)
    100% |████████████████████████████████| 9.5MB 2.2MB/s
Requirement already satisfied: wheel>=0.26 in /home/hhnoh/miniconda3/envs/jupyternotebook/lib/python3.6/site-packages (from tensorflow) (0.31.1)
Collecting tensorboard<1.11.0,>=1.10.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/c6/17/ecd918a004f297955c30b4fffbea100b1606c225dbf0443264012773c3ff/tensorboard-1.10.0-py3-none-any.whl (3.3MB)
    100% |████████████████████████████████| 3.3MB 5.7MB/s
Requirement already satisfied: six>=1.10.0 in /home/hhnoh/miniconda3/envs/jupyternotebook/lib/python3.6/site-packages (from tensorflow) (1.11.0)
Collecting setuptools<=39.1.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/8c/10/79282747f9169f21c053c562a0baa21815a8c7879be97abd930dbcf862e8/setuptools-39.1.0-py2.py3-none-any.whl (566kB)
    100% |████████████████████████████████| 573kB 7.4MB/s
Collecting protobuf>=3.6.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/c2/f9/28787754923612ca9bfdffc588daa05580ed70698add063a5629d1a4209d/protobuf-3.6.1-cp36-cp36m-manylinux1_x86_64.whl (1.1MB)
    100% |████████████████████████████████| 1.1MB 4.3MB/s
Collecting termcolor>=1.1.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/8a/48/a76be51647d0eb9f10e2a4511bf3ffb8cc1e6b14e9e4fab46173aa79f981/termcolor-1.1.0.tar.gz
Collecting numpy<=1.14.5,>=1.13.3 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/68/1e/116ad560de97694e2d0c1843a7a0075cc9f49e922454d32f49a80eb6f1f2/numpy-1.14.5-cp36-cp36m-manylinux1_x86_64.whl (12.2MB)
    100% |████████████████████████████████| 12.2MB 2.9MB/s
Collecting gast>=0.2.0 (from tensorflow)
  Downloading https://files.pythonhosted.org/packages/5c/78/ff794fcae2ce8aa6323e789d1f8b3b7765f601e7702726f430e814822b96/gast-0.2.0.tar.gz
Collecting markdown>=2.6.8 (from tensorboard<1.11.0,>=1.10.0->tensorflow)
  Downloading https://files.pythonhosted.org/packages/6d/7d/488b90f470b96531a3f5788cf12a93332f543dbab13c423a5e7ce96a0493/Markdown-2.6.11-py2.py3-none-any.whl (78kB)
    100% |████████████████████████████████| 81kB 4.7MB/s
Collecting werkzeug>=0.11.10 (from tensorboard<1.11.0,>=1.10.0->tensorflow)
  Downloading https://files.pythonhosted.org/packages/20/c4/12e3e56473e52375aa29c4764e70d1b8f3efa6682bef8d0aae04fe335243/Werkzeug-0.14.1-py2.py3-none-any.whl (322kB)
    100% |████████████████████████████████| 327kB 5.0MB/s
Building wheels for collected packages: absl-py, termcolor, gast
  Running setup.py bdist_wheel for absl-py ... done
  Stored in directory: /home/hhnoh/.cache/pip/wheels/3c/33/ae/db8cd618e62f87594c13a5483f96e618044f9b01596efd013f
  Running setup.py bdist_wheel for termcolor ... done
  Stored in directory: /home/hhnoh/.cache/pip/wheels/7c/06/54/bc84598ba1daf8f970247f550b175aaaee85f68b4b0c5ab2c6
  Running setup.py bdist_wheel for gast ... done
  Stored in directory: /home/hhnoh/.cache/pip/wheels/9a/1f/0e/3cde98113222b853e98fc0a8e9924480a3e25f1b4008cedb4f
Successfully built absl-py termcolor gast
twisted 18.7.0 requires PyHamcrest>=1.9.0, which is not installed.
Installing collected packages: astor, absl-py, grpcio, markdown, numpy, werkzeug, setuptools, protobuf, tensorboard, termcolor, gast, tensorflow
  Found existing installation: setuptools 40.2.0
    Uninstalling setuptools-40.2.0:
      Successfully uninstalled setuptools-40.2.0
Successfully installed absl-py-0.5.0 astor-0.7.1 gast-0.2.0 grpcio-1.15.0 markdown-2.6.11 numpy-1.14.5 protobuf-3.6.1 setuptools-39.1.0 tensorboard-1.10.0 tensorflow-1.10.1 termcolor-1.1.0
You are using pip version 10.0.1, however version 18.0 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
(jupyternotebook) [hhnoh@localhost notebooks]$
```
