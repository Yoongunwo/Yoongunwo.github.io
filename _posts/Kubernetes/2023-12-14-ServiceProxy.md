---
title: "Service Proxy Mode"
categories:
  - Kubernetes
toc: true
toc_label: "목차"
#toc_icon:
toc_sticky: true
#last_modified_at:
---

## 서비스 프락시 종류

서비스는 애플리케이션을 실행하는 하나 이상의 파드로 요청을 전달하는 추상화 기능을 제공함.

서비스를 만들 때 요청을 전달할 파드를 서비스에 알려주는 셀렉터를 정의함. 요청이 kube-proxy 기능을 통해 서비스에 도달하면 해당 요청은 서비스 셀렉터와 일치하는 파드로 전달됨.

쿠버네티스에는 세 가지 프락시 모드를 사용할 수 있음.

### 1. userspace proxy mode

- 쿠버네티스 버전 1.0부터 사용 가능한 가장 오래된 프락시 모드
- 파드 하나로의 연결 요청이 실패하면 kube-proxy가 자동으로 다른 파드에 연결 재시도
- Round-Robin 방식으로 일치하는 파드에 요청을 보냄.

![image](https://github.com/Yoongunwo/SWF2023-Real3.0/assets/97718735/50d6a01d-2239-4834-92d1-4a473f15c113){: .align-center width="60%"}

### 2. Iptables proxy mode

- 1.1부터 사용 가능하고 1.2부터 기본값
- kube-proxy가 iptables를 관리하는 역할. 따라서 직접 클라이언트에서 트래픽을 받지 않음
- 클라이언트에서 오는 모든 요청은 iptables를 거쳐서 파드로 직접 전달
- userspace 모드와 다르게 파드 하나로의 연결 요청이 실패하면 재시도하지 않고 그냥 요청이 실패함.
- userspace 모드보다 낮은 부담으로 Round-Robin 또는 Random Selection을 사용할 수 있음

![image](https://github.com/Yoongunwo/SWF2023-Real3.0/assets/97718735/9b37829b-3925-4e78-ad4a-00ec091b2f92){: .align-center width="60%"}

### 3. IPVS(IP Virtual Server) 모드

- 1.8부터 사용 가능한 최신 옵션
- Kernel space에서 동작하고 데이터 구조를 해시 테이블로 저장하기 때문에 iptables 모드보다 빠르고 좋은 성능을 가짐
- 더 많은 로드밸런싱 알고리즘이 있어서 이를 이용할 수 있음

  - Round-Robin : 프로세스에 우선순위를 두지 않고 순서와 시간 단위로 CPU 할당
  - Least Connection : 접속 개수가 가장 적은 서버 선택
  - Destination Hashing : 목적지 IP 주소로 해시값을 계산해 분산할 실제 서버 선택
  - Source Hashing : 출발지 IP 주소로 해시값을 계산해 분산할 실제 서버 선택
  - Shorted Expected Delay : 응답 속도가 가장 빠른 서버 선택
  - Never Queue : Shortest Expected Delay 와 비슷하지만 활성화된 접속 개수가 0인 서버를 먼저 선택

![image](https://github.com/Yoongunwo/SWF2023-Real3.0/assets/97718735/7092c69c-69fc-43f3-9f7a-cf81c25df993){: .align-center width="60%"}

## Ref.

- 알렉산더 라울, 클라우드 네이티브 쿠버네티스, 위키북스.
- [[kubernetes]가상 IP 및 서비스 프록시](https://kubernetes.io/ko/docs/reference/networking/virtual-ips/)
- [[김징어] [k8s] kube-porxy가 네트워크를 관리하는 3가지 모드(userspace, iptables, IPVS)](https://kimjingo.tistory.com/152)
