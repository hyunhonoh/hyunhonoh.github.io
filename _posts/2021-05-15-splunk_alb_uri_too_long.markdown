---
layout: post
title:  "Splunk Search Head Clustering 구축 시 ALB 사용 시 주의"
date:   2021-05-15 01:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- aws
- alb
classes: wide
published: true
---

Splunk SHC 구축 시 [매뉴얼](https://docs.splunk.com/Documentation/Splunk/8.1.3/DistSearch/UseSHCwithloadbalancers)에는 load balancer를 이용하라고 나옵니다.
Layer 7급의 lb를 구축하라고 되어 있어 alb를 가지고 구현을 하였는데 하나의 문제가 발생합니다.

검색문이 긴 쿼리는 alb의 헤더 사이즈에 의해서 쿼리가 제한되어 414 uri too long 에러가 발생하면서 검색이 안되는 상태로 됩니다. AWS 관련 문서를 찾아보니 alb 에 헤더 길이 제한이 되어 있고 변경을 할 수 없다고 합니다.

https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html

```
HTTP header limits

The following size limits for Application Load Balancers are hard limits that cannot be changed.

HTTP/1.x headers

Request line: 16 K
Single header: 16 K
Whole header: 64 K
HTTP/2 headers

Request line: 16 K
Single header: 16 K
Whole header: 64 K
```

다른 [문의](https://community.splunk.com/t5/Archive/Large-Splunk-queries-causing-quot-Request-URI-too-long-error/m-p/437368) 등을 보면 savedsearch로 변경후에 조치했다고 하는데 이건 재개발 할때나 필요한 것이고 만들어진 수많은 대시보드를 모두 사용할 수 없으면 무의미하여 clb나 nlb 로 구현을 하여야 합니다.

기록을 위해 남겨둡니다.