---
layout: post
title:  "Splunk Operator for Kubernetes 연습하기"
date:   2025-11-18 01:23:35 +0900
categories: 
- Splunk
tags:
- splunk
- kubernetes
- sok
- operator
classes: wide
published: true
---


Splunk는 초기에는 Docker 이미지를 제공하는 정도였기 때문에 “컨테이너 기반 배포”까지만 고려할 수 있었습니다.  
하지만 Kubernetes 환경이 대세가 되면서 Splunk도 공식적으로 **Splunk Operator for Kubernetes(SOK)** 를 제공하고 있습니다.

기존에는 가상 머신(VM) 기반 배포를 주로 권장했지만, 최근 공식 문서에서도 Kubernetes 기반 배포에 대한 명확한 지원을 확인할 수 있습니다.

- 공식 문서:  
  https://help.splunk.com/en/splunk-cloud-platform/get-started/splunk-validated-architectures/applied-svas/splunk-operator-for-kubernetes
- GitHub 저장소:  
  https://github.com/splunk/splunk-operator

SOK(Splunk Operator for Kubernetes)를 활용하면 **Splunk 환경 구성과 확장, 운영이 훨씬 편리해집니다.**  
아래는 테스트를 진행하면서 정리한 내용입니다. (일부는 틀릴 수도 있습니다.)

---

## 테스트 중 확인한 내용

### 1. Operator 업그레이드 시 자동 버전 업그레이드
Splunk Operator를 업그레이드하면, 해당 Operator가 지원하는 버전으로 Splunk 인스턴스들이 자동 업그레이드됩니다.

### 2. 기본 설치 시 자동 버전 설정
버전을 지정하지 않고 배포하면 Operator가 **기본 지원 Splunk 버전(default supported version)** 을 자동으로 설정합니다.

### 3. YAML에서 Splunk 이미지 버전 지정 가능
YAML에서 이미지를 직접 지정할 수 있습니다.

```yaml
spec:
  image: splunk/splunk:9.1.4
```

SOK와 localstack을 이용하면 로컬에서 smartstore연동 테스트가 가능한 부분도 흥미로웠습니다.


기록을 위해 남겨둡니다.