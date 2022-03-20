---
layout: post
title:  "Splunk REST API is EASY to use code 변경"
date:   2018-04-22 16:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- python
- restapi
- python3
classes: wide
published: true
---



기존에 Splunk의 데이터를 외부에서 가져오기 위해서 참조하는 예제는 python 예제 중에서 이 예제가 기본 이었습니다. 

[Splunk REST API is EASY to use](https://www.splunk.com/blog/2011/08/02/splunk-rest-api-is-easy-to-use.html)

2011년도에 나온 버전이나 python 2.7버전에서는 아직도 무리없이 작동을 합니다. 하지만 세월이 흐르고 지금은 python은 모두 3버전으로 작업을 하고 있어서 이걸 하기 위해서 따로 2.7버전을 설치해야하는 번거로움을 조금이라도 줄일 수 있을까 하고 수정해 보았습니다.

아래는 jupyter notebook에서 작업한 내용이며  소스는 [여기](https://github.com/hyunhonoh/Splunk-Tips/blob/master/restapi_getdata.py)에서 참조 할 수 있습니다.





```python
#Step 1: Get a session key

import urllib
import lxml.html
import requests
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

baseurl = 'https://localhost:8089'
username = 'admin'
password = '1'

url = baseurl + '/services/auth/login'
data = urllib.parse.urlencode({'username': username, 'password': password})
# data = data.encode('ascii')

with requests.post(url, data=data, verify=False) as req:
    sessionkey = lxml.html.fromstring(req.content)[0].text
print("sessionkey {}".format(sessionkey))
```

    sessionkey NXi4R88NyLi2jYTXCyuTfS8woSsXYD1u_eKCF079Zy_XuvmxKqE5s1KE1sjE^xWNLItzwPbrsPkAQzI_ugj2bLWbbI93pTv5CpCwC8A

```python
#Step 2: Create a search job
searchquery = 'index="_internal" | head 10'
if not searchquery.startswith('search'):
    searchquery = 'search ' + searchquery

searchjoburl = baseurl + '/services/search/jobs'
with requests.post(searchjoburl, verify=False, 
              data=urllib.parse.urlencode({'search': searchquery}),
              headers={'Authorization': 'Splunk {}'.format(sessionkey)}) as req:
    sid = lxml.html.fromstring(req.content)[0].text
print("sid {}".format(sid))
```

    sid 1524381007.72

```python
#Step 3: Get the search status

servicessearchstatusstr = '/services/search/jobs/%s/' % sid

isnotdone = True
while isnotdone:
    with requests.post(baseurl + servicessearchstatusstr, 
                             headers={'Authorization': 'Splunk {}'.format(sessionkey)}, verify=False) as searchstatus:
            
        isDone=lxml.html.fromstring(searchstatus.content)
        
        isdonestatus=isDone.cssselect('key[name=isDone]')[0].text
        if(isdonestatus == '1'):
            isnotdone = False
print ("search status : {}".format(isdonestatus))

```

    search status : 1

```python
#Step 4: Get the search results
services_search_results_str = "/services/search/jobs/{}/results?output_mode=json&count=0".format(sid)
print(services_search_results_str)

with requests.get(baseurl + services_search_results_str, 
                 headers={'Authorization': 'Splunk {}'.format(sessionkey)}, verify=False) as searchresults:
    print(searchresults.content)
            
print ("====>search result:  [%s]  <====" % searchresults.content)
```

```shell
/services/search/jobs/1524381007.72/results?output_mode=json&count=0
b'{"preview":false,"init_offset":0,"messages":[],"fields":[{"name":"_bkt"},{"name":"_cd"},{"name":"_indextime"},{"name":"_raw"},{"name":"_serial"},{"name":"_si"},{"name":"_sourcetype"},{"name":"_subsecond"},{"name":"_time"},{"name":"host"},{"name":"index"},{"name":"linecount"},{"name":"source"},{"name":"sourcetype"},{"name":"splunk_server"}],
......
```







