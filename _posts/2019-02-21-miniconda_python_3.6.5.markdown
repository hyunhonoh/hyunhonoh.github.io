---
layout: post
title:  "Miniconda 버전 확인(python 3.6 버전 설치하기)"
date:   2019-02-21 19:23:35 +0900
categories: 
- Linux
- Windows
- python
tags:
- version
- linux
- python
- conda
classes: wide
published: true
---

miniconda를 재설치 하기 위해서 매번 홈페이지에서 새 버전을 다운 받습니다. 
요즘에는 3.7.x버전이 기본으로 들어간 miniconda를 받는데 가상환경은 매번 3.6버전대를 다운을 받아 실행합니다.

크게 문제는 없지만 처음 base도 3.6.x대를 맞추고 싶은 마음에 몇 버전이 3.6.x인지 확인해 보았습니다.

- [Miniconda3-4.5.4-Windows-x86_64.exe](https://repo.continuum.io/miniconda/Miniconda3-4.5.4-Windows-x86_64.exe)
- [Miniconda3-4.5.4-Linux-x86_64.sh](https://repo.continuum.io/miniconda/Miniconda3-4.5.4-Linux-x86_64.sh)

이 버전을 받아야 안에 들어있는 python이 3.6.5버전으로 설치가 됩니다. 물론 가상환경을 만들 때 내가 원하는 python버전을 설치할 수 있지만 그냥 3.6으로 맞추고 싶네요.

![Miniconda3-4.5.4-Windows-x86_64.exe](/images/conda_4.5.4_python_3.6.5.png)





기존 파일 목록 : [https://repo.continuum.io/miniconda/](https://repo.continuum.io/miniconda/)

---

위 파일을 다 설치 후 문제가 발생하였습니다. (메시지는 조금 다르지만 이 문제는 4.5.12 버전에서도 동일하게 나왔었습니다. )

추가로 윈도우에서 가상환경을 만들려고 하는데 다음과 같은 에러가 나면서 실행이 안되는 현상이 발생했습니다.

```bash
CondaHTTPError: HTTP 404 NOT FOUND for url <https://pypi.python.org/pypi/noarch/repodata.json>
Elapsed: 00:00.244046

The remote server could not find the noarch directory for the
requested channel with url: https://pypi.python.org/pypi

As of conda 4.3, a valid channel must contain a `noarch/repodata.json` and
associated `noarch/repodata.json.bz2` file, even if `noarch/repodata.json` is
empty. please request that the channel administrator create
`noarch/repodata.json` and associated `noarch/repodata.json.bz2` files.
$ mkdir noarch
$ echo '{}' > noarch/repodata.json
$ bzip2 -k noarch/repodata.json

You will need to adjust your conda configuration to proceed.
Use `conda config --show channels` to view your configuration's current state.
Further configuration help can be found at <https://conda.io/docs/config.html>.

A reportable application error has occurred. Conda has prepared the above report.
If submitted, this report will be used by core maintainers to improve
future releases of conda.
Would you like conda to send this report to the core maintainers?
[y/N]:
```

기존에는 예전버전을 설치하여도 없었던거 같은데 에러가 계속 나서 찾아보니 conda channel이 아니어서 그런거 같다는 글이 있었습니다.

- [https://github.com/conda/conda/issues/7095](https://github.com/conda/conda/issues/7095)

해결은 아래와 같이 삭제를 해 주고 난 후에 정상적으로 실행이 되었습니다.

```bash
conda config --remove channels https://pypi.python.org/pypi/noarch
conda config --remove channels https://pypi.python.org/pypi/win-64
```