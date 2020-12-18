# CNCF:crystal_ball:

> 혼돈스러운 컨테이너와 관련된 다양한 기술적인 문제들을 오픈소스로 해결하는 하는 것을 목표
>
> 대표적으로 Kubernetes 와 Prometheus 와 같은 클라우드 네이티브 오픈소스 기술들을 추진하고 관리하는 단체

- **Cloud Native 기술**: 클라우드 네이티브 기술을 사용하는 조직은 현대적인 퍼블릭, 프라이빗, 그리고 하이브리드 클라우드와 같이 동적인 환경에서 확장성 있는 애플리케이션을 만들고 운영할 수 있다
  - 컨테이너, 서비스 메시, 마이크로서비스, 불변의 인프라스트럭처, 그리고 선언적 API가 전형적인 접근 방식에 해당
  - 회복성이 있고, 관리 편의성을 제공하며, 가시성을 갖는 느슨하게 결합된 시스템을 가능

## Trail Map

![](https://github.com/cncf/trailmap/raw/master/CNCF_TrailMap_latest.png)



## containerd<img src="https://camo.githubusercontent.com/ce52ba4f81cf2a2f218567cb2eaed08a9a1a6968b18ba3618dec889f94018b24/68747470733a2f2f636f6e7461696e6572642e696f2f696d672f6c6f676f732f69636f6e2f626c61636b2f636f6e7461696e6572642d69636f6e2d626c61636b2e706e67" style="zoom:5%;" />

> 업계 표준 컨테이너 런타임, runC를 제어하는 데몬으로 Docker engine에 내장됨
>
> **runC**:  container의 생성/실행 등을 위한 기본적인 기술이며 CLI 도구, 컨테이너의 기본 런타임으로 사용됨

- 전체 컨테이너 life cycle(이미지 전송 및 저장, 컨테이너 실행 및 관리, low-level 저장 및 네트워크 첨부 등)을 관리할 수 있음
- runC를 활용하여 체크 포인트 및 복원, seccomp 및 userspace 지원과 같은 고급 기능을 제공
- ![](https://t1.daumcdn.net/cfile/tistory/257FE535595AF79817)



## Jaeger​​<img src="https://www.jaegertracing.io/img/jaeger-icon-color.png" style="zoom:3%;" />

> 분산 서비스 간 트랜잭션을 추적하는 오픈소스 소프트웨어로 복잡한 마이크로서비스 환경을 모니터링하고 문제를 해결하는 데 사용

  ![그림입니다.  원본 그림의 이름: CLP000045880003.bmp  원본 그림의 크기: 가로 3360pixel, 세로 1859pixel](https://www.jaegertracing.io/img/trace-detail-ss.png)  

### 분산 추적?

> 마이크로서비스 간 복잡한 상호 작용에서 이벤트 전체를 파악하는 방식

- 애플리케이션에서의 단 한번의 호출로 상호 작용하는 수십 개의 서로 다른 서비스가 실행될 수도 있음. 어떤 문제가 발생하거나 요청의 실행 속도가 느릴 경우 개발자와 엔지니어가 문제를 파악하기 위해서는 모든 연결을 추적할 수 있는 방식이 필요 -> **분산 추적이 사용됨**
- Jaeger는 분산 추적을 사용해 다양한 마이크로서비스의 요청 경로를 추적하기 때문에 모호하게 추측하는 것이 아니라 실제 요청 흐름을 *시각적으로 확인 가능*
- 분산 트랜잭션을 모니터링하고, 성능과 대기 시간을 최적화하고, 문제 해결을 위한 근본 원인 분석(Root Cause Analysis, RCA)을 수행할 툴을 갖추고 있음

### 용어 및 구성 요소

- Jaeger는 실행 요청을 **추적**으로 표시
- **추적**: 시스템 전반에서 데이터/실행 경로를 나타냄
- 추적은 1개 이상의 **스팬**(Jaeger의 논리적 작업 단위)으로 이루어짐
  - 각 스팬에는 운영 작업의 이름, 시작 시간, 기간이 포함
  - 스팬은 중첩되거나 순서대로 정리되어 있을 수 있음
- Jaeger는 연동하여 스팬과 추적을 수집, 저장, 시각화하는 여러 구성 요소가 포함되어 있음
- **Jaeger Client**: 분산 추적을 위한 OpenTracing API의 언어별 구현이 포함, 수동 또는 다양한 오픈소스 프레임워크와 함께 사용 가능
- **Jaeger Agent**: 사용자 데이터그램 프로토콜 (User Datagram Protocol)을 통해 전송된 스팬을 수신하는 네트워크 데몬
  - 일반적으로 쿠버네티스와 같은 컨테이너 환경의 사이드카를 통해 구현
- **Jaeger Collector**: 프로세싱을 위해 스팬을 수신하여 대기열에 배치
- **Query**: 스토리지에서 추적을 검색하는 서비스
- **Jaeger Console**: 분산된 추적 데이터를 시각화하는 사용자 인터페이스



## CoreDNS<img src="https://avatars3.githubusercontent.com/u/21110084?s=400&v=4" style="zoom:10%;" />

> 쿠버네티스 클러스터의 DNS 역할을 수행할 수 있는, 유연하고 확장 가능한 DNS 서버

- 사용자는 기존 디플로이먼트인 kube-dns를 교체하거나, 클러스터를 배포하고 업그레이드하는 kubeadm과 같은 툴을 사용하여 클러스터 안의 kube-dns 대신 CoreDNS를 사용할 수 있음



## Kubernetes<img src="https://download.logo.wine/logo/Kubernetes/Kubernetes-Logo.wine.png" style="zoom:5%;" />

