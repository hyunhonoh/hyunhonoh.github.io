---
layout: post
title:  "Splunk Modular Input의 데이터입력을 sdk로 추가하기"
date:   2018-08-17 20:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- sdk
- modular input
classes: wide
published: true
---

Splunk는 Modular Input을 지원합니다. Modular Input에 대한 설명은 다음 링크에서 확인을 해 볼 수 있습니다.

http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsIntro#About_modular_inputs



간단히 이해하는 내용은 기본 입력 방식 외에 사용자가 추가로 새로운 시스템의 입력방식을 추가할 수 있는 것입니다.  그래서 새로운 로그 입력 도구들이 나왔는데 Splunk에 없으면 이 기능을 이용해서 추가 데이터 입력을 만들 수 있습니다.

Splunk python sdk를 이용해서 만드는 방법은 다음 링크에서 설명이 되어 있습니다.

http://dev.splunk.com/view/python-sdk/SP-CAAAER3




여기서는 이렇게 만들어진 앱에 데이터 입력 및 추가를 sdk를 이용해서 하는 방법에 대해서 간단히 알아보려고 합니다.

가상환경을 만들고 splunk sdk for python 를 설치합니다.

```
conda create -n splunksdk python=3.6
source activate splunksdk
pip install splunk-sdk
```


개발시 python은 [Python sdk의 api reference](http://docs.splunk.com/Documentation/PythonSDK)를 참조하면 됩니다. 이번에는 inputs에 관련된 내용이기 때문에 http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Inputs 를 참조하면 됩니다.


#### 연결 및 리스트 조회

- 기본 restapi의 주소는 다음과 같습니다. 

```
https://127.0.0.1:8089/services/data/inputs
```

접속을 하면 inputs의 목록이 나오는 화면입니다.

![](/images/splunk_modular_input_01.png)

하위 주소로 각각의 입력을 따라가면 각 입력에 대한 목록이 추가로 나옵니다. 여기서는 snmp앱의 데이터 입력을 확인해 보고 추가해 보려고 합니다.


- 소스 

```python
# https://127.0.0.1:8089/services/data/inputs/snmp : restapi address

import splunklib.client as client

s = client.connect(host="localhost", port=8089, username="admin", password="shgusgh")

# inputs list 
inputlists = s.inputs

for n in inputlists.list("snmp"):
    print (n.name)
```

위와 같이 하면 목록이 30개 보입니다. 리스트를 볼 때 ``list("snmp")``가 없어도 화면에 표시가 되지만 이는 전체를 보여주기 때문에 다른 필요 없는 것들도 보여지게 ㄷ되어 list를 활용하는게 좋습니다.

#### 추가

새로운 데이터 입력을 만들려면 create를 이용해서 추가할 수 있습니다.

```python
%%time
# create
kwargs = {'activation_key':'activation_key', 'app':'snmp_ta', 
          'snmp_mode':'attributes', 'snmp_version':'2C', 'snmpinterval':60, 'communitystring':'public',
          'destination':'127.0.0.1', 'port':'1024', 'do_bulk_get':1, 'do_get_subtree':0,
          'ipv6':0, 'sourcetype':'snmp', 'object_names':'1.3.6.1.2.1.4.31.1.1.0, 1.3.6.1.2.1.6.15.0', 'split_bulk_output':1, 'trap_rdns':0}
for i in range(1,10):
    inputlists.create("local_2015_{}".format(i),kind="snmp", **kwargs)
```

위 예제는 splunk modular input앱을 설치하고 그 앱에 추가적인 데이터 입력을 한번에 하려고 합니다. 앱이 새로 바뀌면서 activation_key를 요구하고 있어서 홈페이지에서 요청을 해야 합니다. 입력 테스트라서 activation_key를 임의로 넣었습니다.

kwargs에 사용한 값들은 앱에서 요구하는 기본 입력 값 및 추가 입력값의 변수를 확인해서 필요한 부분을 넣어주었습니다.
여기서는 미리 ``local_2015_1``값을 넣고 확인하였습니다.

```
https://localhost:8089/servicesNS/nobody/snmp_ta/data/inputs/snmp/local_2015_1
```

![](/images/splunk_modular_input_02.png)


create문을 이용해서 값을 넘어주면 splunk에 추가가 되는 것을 볼 수 있습니다. create를 사용할 때는 kind를 주어서 어디 입력에 넣으려고 하는지를 적용해야 합니다.

입력이 정상적으로 되면 아래와 같이 추가된 화면을 확인 할 수 있습니다. 추가를 하면서 느낀게 데이터 입력을 하면 내부적으로 스케줄링이 다시 수행이 되기 때문에 전체 refresh가 되면서 양이 많아지면 엄청 늦어진다는 것을 알게 되었습니다. bulk로 한번에 추가를 하는 방법이 있는지 확인을 해 봐야 할 듯 합니다.

![](/images/splunk_modular_input_03.png)


#### 삭제

```python
%%time
# delete

for i in range(1, 2):
    try:
        inputlists.delete("local_2015_{}".format(i),kind="snmp")
        print("delete ok")
    except:
        pass
```

삭제는 이름만 자세히 넣어주면 삭제가 됩니다.
