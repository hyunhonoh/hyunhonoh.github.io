---
layout: post
title:  "오라클 자율운영DB 연결 및 데이터 저장"
date:   2022-03-20 01:23:35 +0900
categories: 
- Linux
tags:
- oracle
- db
- cx_Oracle
classes: wide
published: true
---

Oracle Cloud를 이용해서 간단한 서버를 구성하여 연습하고 있습니다. 이번에 데이터베이스를 활용하고 싶어서 확인하니 MySql은 유료 플랜에서만 가능하고 무료에서는 자율운영데이터베이스라는 것이 사용이 가능했습니다.

Oracle Cloud 메뉴에서 확인이 가능합니다.

![](/images/oracle_autonomous_0.png){: width="640" }


신규 생성을 하면 다음과 같이 작업 로드 유형으로 4가지를 선택할 수 있습니다.

![](/images/oracle_autonomous_1.png){: width="640" }

이번 테스트에서는 JSON 형식으로 설정을 하고 JSON 타입의 데이터를 입력하려고 테스트를 생성했습니다.

### SQL Developer 연결

데이터베이스를 연결하기 위해서 우선 SQL Developer로 연결하려고 했습니다. 데이터베이스에서 제공하는 wallet을 받아서 적용하려고 압축을 해제하고 sql developer에서 tnsnames.ora를 선택하고 수행을 하니 에러가 발생했습니다.

```
요청한 작업을 수행하는 중 오류 발생:

IO 오류: Got minus one from a read call, connect lapse 8 ms., Authentication lapse 0 ms.

업체 코드 17002
```

원인을 찾기 위해 여러 곳을 돌아다녔는데 결론은 추가가 잘 못 된 것이었습니다. 감지된 데이터베이스에서 추가를 하는 게 아니라 신규를 이용하는 것이었습니다.

![](/images/oracle_autonomous_2.png){: width="640" }

접속 유형을 "클라우드 전자 지갑"으로 변경 후에 구성 파일을 압축파일 그대로 추가했어야 했습니다. 이걸 모르고 계속 추가를 하니 에러가 발생해서 접속하는 것부터 힘이 들었네요.


### cx_Oracle 연결

로컬에서는 SQL Developer를 이용하고 서버에서는 cx_Oracle로 연결을 하려고 하여 동일하게 찾아보고 설치했습니다.

[cx_Oracle Install](https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html#installing-cx-oracle-on-linux)

매뉴얼을 보고 설치를 하고 샘플 코드로 접근을 수행하는 데 에러가 발생했습니다.


```bash
cx_Oracle.DatabaseError: ORA-28759: failure to open file
```

원인을 찾아보니 환경 파일에 경로를 변경하지 않아서 발생한다고 합니다.

```python
import cx_Oracle
cx_Oracle.init_oracle_client(lib_dir=r"/opt/oracle/instantclient_21_5", config_dir="/home/ubuntu/Wallet")
```

위와 같이 config_dir로 정의해 주었는데 해당 위치에 sqlnet.ora 파일의 내용을 수정해 주어야 합니다. DIRECTORY를 파일 위치로 변경해 주어야 합니다.

#### 수정전 (기본값)

```
WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY="?/network/admin")))
SSL_SERVER_DN_MATCH=yes
```

#### 수정후

```
WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY="/home/ubuntu/Wallet")))
SSL_SERVER_DN_MATCH=yes
```

파일 내용을 변경 해 주고 다시 수행을 하면 정상적으로 데이터베이스에 접근이 가능합니다.

참조 : [https://www.linkedin.com/pulse/let-your-python-applications-connect-oracle-database-michal-soszynski](https://www.linkedin.com/pulse/let-your-python-applications-connect-oracle-database-michal-soszynski)


기록을 위해 남겨둡니다.