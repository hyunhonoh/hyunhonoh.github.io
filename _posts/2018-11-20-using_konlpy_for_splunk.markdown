---
layout: post
title:  "Splunk 에서 konlpy 사용해 보기"
date:   2018-11-20 00:23:35 +0900
categories: 
- Splunk
tags:
- Splunk
- nlp
- konlpy
- 형태소분석
- 한글분석
classes: wide
published: true
---





작년 이 맘때쯤 [Splunk에서 신문기사 분석해 보기](https://hyunhonoh.github.io/splunk/splunk-analysis-in-news/)라는 포스팅을 했었습니다. 여기에는 konlpy라는 python 한글 형태소 분석기에 대한 얘기가 나옵니다. 
개인적으로 Splunk에서 테스트해 보기 위해서 만들었던 명령어에 쓰인 konlpy가 요즘에는 확인해 보니 버전업이 되어 0.5.x버전까지 올라가 있었습니다. 여러가지 변경 된 사항이 있고 기존에 있던 Twitter분석기가 이름이 바뀌어서 Open-korean-text로 추가가 되어 있었습니다.

그래서 버전업이 되어서 올라간 버전도 수행이 되는지 확인차 버전업을 해 보았는데 잘 적용이 되었습니다.
다행히 JPype1 패키지 버전 의존도가 낮아 기존에 만들어 두었던 것을 다시 컴파일을 하지 않아도 되었습니다. Splunk는 기본적으로 Python 2.7 버전대를 사용하고 있는데 konlpy 자체는 다행히 2, 3버전대를 무리없이 지원을 합니다. 대신 JPype1가 Splunk에서는 더 문제입니다.
기존에 작성했던 [Splunk Python UCS 확인하기](https://hyunhonoh.github.io/splunk/splunk_python_UCS_check/)를 보면 Splunk의 python Unicode 값이 달라서 일반적으로 build된 JPype1을 바로 사용할 수 없습니다. 그래서 동일한 버전(2.7) 환경에서 다시 UCS에 맞게 설정된 환경에서 build를 해야 제대로 Splunk에서 인식할 수 있습니다.

아래는 Twitter를 Okt로 수정한 Splunk 명령어 python 입니다. 앱으로 만들고 적용을 하면 Splunk에서 한글 형태소를 바로 분류할 수 있습니다.


참조 : 0.5.2버전에서는 기능이 많이 추가가 되어서 다른 패키지의 [의존성](https://github.com/konlpy/konlpy/pull/211)도 있는 형태로 된 것으로 보입니다. 혹시나 이걸 사용하실려면 0.5.1버전에서 테스트가 되었기 때문에 0.5.1버전으로 사용하시면 됩니다.

```python
#-*- coding:utf-8 -*-
import re,sys,time, splunk.Intersplunk
from konlpy.tag import Kkma
from konlpy.tag import Okt
from konlpy.tag import Komoran
from konlpy.tag import Mecab
from konlpy.utils import pprint
import logging, logging.handlers
import sys, os

###
### Splunk logging setup
###
def setup_logging():
    logger = logging.getLogger('splunk.konlpy')
    SPLUNK_HOME = os.environ['SPLUNK_HOME']

    LOGGING_DEFAULT_CONFIG_FILE = os.path.join(SPLUNK_HOME, 'etc', 'log.cfg')
    LOGGING_LOCAL_CONFIG_FILE = os.path.join(SPLUNK_HOME, 'etc', 'log-local.cfg')
    LOGGING_STANZA_NAME = 'python'
    LOGGING_FILE_NAME = "konlpyspl.log"
    BASE_LOG_PATH = os.path.join('var', 'log', 'splunk')
    LOGGING_FORMAT = "%(asctime)s %(levelname)-s\t%(module)s:%(lineno)d - %(message)s"
    splunk_log_handler = logging.handlers.RotatingFileHandler(os.path.join(SPLUNK_HOME, BASE_LOG_PATH, LOGGING_FILE_NAME), mode='a')
    splunk_log_handler.setFormatter(logging.Formatter(LOGGING_FORMAT))
    logger.addHandler(splunk_log_handler)
    splunk.setupSplunkLogger(logger, LOGGING_DEFAULT_CONFIG_FILE, LOGGING_LOCAL_CONFIG_FILE, LOGGING_STANZA_NAME)
    return logger


def konlpyspl(results, settings):

    try:
        fields, argvals = splunk.Intersplunk.getKeywordsAndOptions()
        optionstr       = argvals.get("option", "nouns")
        dedupstr        = argvals.get("dedup", "false")
        typestr         = argvals.get("type", "okt")

        if typestr == "kkma":
            nlp=Kkma()
        if typestr == "okt":
            nlp=Okt()
        if typestr == "komoran":
            nlp=Komoran()
        if typestr == "mecab":
            nlp=Mecab()

        pos=[]
        for r in results:
            for f in fields:
                if f in r:
                    if optionstr == "nouns":
                        r['konlpynouns']=nlp.nouns(r[f])
                        logger.info ("%s : nouns success" % r[f])

                    elif optionstr == "pos":
                        posresult = nlp.pos(r[f])
                        # 중복 옵션을 주면 list안에 있는 내용을 중복 제거해서 하나의 값만 나온다. 단어가 있는지만 파악할때 사용
                        if dedupstr == "true": posresult=set(posresult)
                        for value, key in posresult:
                            # 여러개의 결과값을 여러개의 결과값으로 출력

                            p = {key.encode('utf-8'):value.encode('utf-8'), f:r[f]}
                            pos.append(p)
                            # 한줄에 전체 표시
                            # r[key.encode('utf-8')] = value.encode('utf-8')
                        logger.info ("%s : pos success" % r[f])

                    elif optionstr == "posdetail":
                        posresult = nlp.pos(r[f])
                        r['konlpypos']= ['%s="%s"' % (key.encode('utf-8'), value.encode('utf-8')) for (value, key) in posresult ]
                        logger.info ("%s : pos detail success" % r[f])

                    # 단어에 대한 tag를 알아내기 위한 옵션.
                    elif optionstr == "posresult":
                        posresult = nlp.pos(r[f])
                        r['tag'] = posresult[0][1]
                        logger.info ("%s : pos result detail success" % r[f])
        # pos 옵션일 때 기본 results값이 추가되는걸 방지하기 위해서 사용
        if pos:
            results = pos

        splunk.Intersplunk.outputResults(results)

    except:
        import traceback
        stack =  traceback.format_exc()
        results = splunk.Intersplunk.generateErrorResults("Error : Traceback: " + str(stack))


# main
logger = setup_logging()
results, dummyresults, settings = splunk.Intersplunk.getOrganizedResults()
results = konlpyspl(results, settings)
```

