# CNCF Graudated.



## Prometheus

  Kubernetes 환경에 들어오면서, 전체 인프라 환경과 Application에 대한 모니터링을 하는 것이 중요해졌다. 인프라에 대한 가시성을 확보해야 Kubernetes 내에 돌아가는 Application에 대한 가용성과 안정성을 확보할 수 있으며 더 나아가 이러한 모니터링 결과를 통해 고도화된 Cloud Native 시스템을 구축할 때 사용하는 지표로 활용할 수 있다.



### 모니터링 아키텍쳐의 변화

#### Legacy 환경

<img src="https://miro.medium.com/max/700/1*bJc3GSRxcD90mZUsboaemg.png" style="zoom:50%;" />  

- 기존 레거시 환경은 고사양의 서버에 application을 크게 운영하는 방식
- 따라서 고사양의 서버에 Monitoring Agent를 서버에 설치한 후, Agent가 OS의 메트릭을 수집하여 backend에 직접 전송하는 방식을 사용하였다.



#### Cloud Native 환경

<img src="https://miro.medium.com/max/700/1*uWBRLUx52hINqY3CECFnLw.png" style="zoom:50%;" /> 

- Cloud Native 환경에서는 Container 기반의 Application을 작게 운영하고 필요할 때마다 확장, 파괴하는 방식을 선택한다.
- 동적으로 확장하는 서버에 agent를 일일이 설치하는 것이 불가능하므로 kubernetes api를 통해 동적으로 확장된 서버에 endpoint를 discovery하는 방식으로 운영된다.
- 이후에 Monitoring Backend에서 discovery한 endpoint를 통해  metric을 수집한다.



### Kubernetes Monitoring Pipeline

  Kubernetes의 모니터링 파이프라인은 2가지로 나눌 수 있는데, Kubernetes 관련 구성 요소를 관리하는 Core Metric Pipeline과 Monitoring으로 나눌 수 있다.



#### Core Metric Pipeline

<img src="https://miro.medium.com/max/700/1*mR3yw0bZQzwoxTVWqlKP6g.png" style="zoom:100%;" /> 

- **Core Metric Pipeline**은 쿠버네티스 관련 구성요소를 직접 관리하는 파이프라인이다.

- Kubelet, metrics-server, metric API 등으로 구성되어 있고, 쿠버네티스의 핵심 동작 요소에 대한 모니터링을 담당하고 현재 상태가 Desired State가 되는 것을 모니터링한다.

- Kubelet에 저장된 cadvisor를 통해 노드/포드/컨테이너의 사용량 정보를 수집하며 동적으로 확장된 노드에 관련된 정보도 같이 수집한다.

  > **cadvisor** : 쿠버네티스에서 사용하는 기본적인 모니터링 에이전트로 모든 노드에 설치되서 노드에 대한 정보와 포드(컨테이너)에 대한 지표를 수집한다.
  >
  > 이렇게 수집된 내용은 kubelet에게 전달되는데 이 후 전달된 내용은 모니터링 툴(메트릭 서버, 프로메테우스)에서 다시 수집해 간다.
  >
  > **cadvisor의** 경우 리소스 데이터만 수집한다. 이 리소스에는 CPU, Memory, Filesystem, Network Used와 같은 통계를 수집한다.

- Kubelet에 저장된 정보는 metric server가 수집하여 메모리에 저장한다. 이때, 메모리에 저장하기 때문에 짧은 기간의 데이터만 저장한다.

- 이렇게 저장된 정보는 Kubernetes Master Metric API를 통해 다른 시스템 component가 조회 가능하게 한다.



### Monitoring Pipeline

![](https://miro.medium.com/max/700/1*W83y72_MD-Kd89fBvMS69A.png) 

- 위 그림에서 파란색으로 써져 있는 부분이 **Monitoring Pipeline**이며 Monitoring Pipeline은 Kubernetes에서 제공하는 기본 메트릭 외에 다양한 메트릭을 수집한다. 특히 클러스터 사용자들이 필요한 모니터링을 하는데 사용된다.
- 이 모니터링 파이프라인은 시스템 메트릭과 서비스 메트릭을 동시에 수집 가능하도록 설계되었다.
- 쿠버네티스에서는 모니터링 파이프라인은 직접 관리하지 않기 떄문에 사용자가 직접 설치하여 모니터링 파이프라인을 구축해야한다. **cAdvisor과 Prometheus** 조합이 가장 많이 쓰인다.



### what is Promethues?

![](https://miro.medium.com/max/320/0*Qat4flC5IwoCqgZx.png) 

- **Prometheus**는 현재  **Kubernetes**상에서 가장 많이 사용하는 오픈 소스 기반 모니터링 시스템이다.
- **CNCF**에 소속되어 있으며 Kubernetes 클러스터 및 Docker 컨테이너들을 손쉽게 모니터링 가능하다.
- 간단한 텍스트 형식으로 메트릭을 쉽게 노출 가능하며 데이터 모델은 key-value 형태로 레이블 집계한 후, **Grafana** 같은 대시보드 시스템에서 그래프로 쉽고 간단하게 **Dashboard**를 만들 수 있다.
- 이해하기 쉬운 PromQL 쿼리 언어를 사용하여 간단하게 경고와 **Ruleset**을 정의 가능하다.



### Prometheus Architecture

  대부분의 모니터링 도구가 **Push** 방식 즉, 각 서버에 클라이언트를 설치하고 이 클라이언트가 메트릭 데이터를 수집해서 서버로 보내면 서버가 모니터링 상태를 보여주는 방식을 취한다.

 하지만 **Prometheus**는 **Pull** 방식을 사용하여, 서버에 클라이언트가 떠있으면 서버가 주기적으로 클라이언트에 접속해서 데이터를 가져오는 방식이다.

<img src="https://miro.medium.com/max/700/1*tHxrGpEUte5iz8IVKAhyfQ.png" style="zoom:150%;" />

- **Service Discoery** : **Prometheus**는 기본적으로 모니터링 대상 목록을 유지하고 있으며, 대상에 대한 ip나 기타 접속 정보를 설정 파일에 주어서 모니터링 정보를 가져오는 방식을 사용한다.

  > 이러한 환경을 대처하기 위해 서비스 디스커버리를 사용하지만 오토스케일링을 하는 환경에서는 ip가 동적으로 변경되는 경우가 많기 때문에 모니터링 대상이 등록되어 있는 저장소에서 목록을 받아서 그 대상을 모니터링하는 형태를 취한다 ex) **kubelet**

- **Push gateway** : **Push gateway** 는 쉽게 말해 **Proxy Forwarding** 을 해서 접근할 수 없는 곳에 데이터가 존재하는 경우에 사용할 수 있는 대안이다. **application이** **push gateway**에 메트릭을 **push한** 후 **prometheus server**가 **push gateway**에 접근해 **metric을** **pull** 해서 오는 방식으로 동작한다.

- **Exporter & job** : **Exporter**는  **Prometheus** 에게 메트릭을 가져가도록 특정 Service에 메트릭을 노추하게 하는 Agent라고 이해하면 편하다.

  > Exporter는 서버 상태를 나타내는 Node exporter, SQL Exporter 등 다양한 커스텀 exporter이 개발되어 사용되고 있으며, 이러한 Exporter를 사용하여메트릭을 Prometheus에 긁어가도록 할 수 있다.

- **Alert Manager** : **Alert Manager**는 **metric**에 대한 어떠한 지표를 걸어놓고 그 규칙을 위반하는 사항에 대해 알람을 전송하는 역할을 한다.

- **Data visulization** : **Data visualization**은 다양한 모니터링 **Dashboard**를 위한 **visualization**을 제공한다. 주로 **Prometheus**가 수집한 데이터에 대한 외부 시각화 툴 및 api를 제공하는 역할을 한다.





## Rook

### Kubernetes 용 스토리지 운영자

![](https://danawalab.github.io/images/2020-01-28-kubernetes-rook-ceph/Untitled.png)

- **Rook**은 분산 스토리지 시스템을 자체 관리, 자체 확장, 자체 복구 스토리지 서비스로 전환한다. deployment, bootstrapping, configuration, provisioning, scaling, upgrading, migration, disaster recovery, monitoring, and resource management와 같은 스토리지 관리자의 작업을 자동화 한다.

- **Rook**은 쿠버네티스 **Pod**에서 실행되며, Ceph, EdgeFS 등 가상 솔루션을 POD로 배포하여 관리하는 도구다. agent를 통해 Ceph, EdgeFS 등 가상 솔루션을 관리하고 OSD를 통해 데이터를 영구저장한다.

  > - **EdgeFS** : 데이터베이스처럼 대용량 스토리지가 필요할때 사용된다
  > - **Ceph** : 공유 확장에 특화되어 있는 스토리지가 필요할때 사용된다.



#### Rook의 특징

![](../images/image-20201221085609199.png)







## TiKV

  **TiKV**는 Google Spanner 및 HBase의 설계를 기반으로 하는 오픈 소스 분산 key-value 데이터베이스지만, 분산 파일 시스템에 의존하지 않고 간단하다.

- TiKV는 수조 개의 행에 걸쳐 페타바이트 규모의 배포를 지원하여 대규모 데이터 작업에 탁월하다.
- 지리적 복제, 수평확장성, 일관된 분산 트랜잭션, Coprocessor 지원을 포함한 주요 기능이 있다.



### 아키텍쳐

![](https://tikv.org/img/basic-architecture.png)





## TUF(The Update Framework)

<img src="https://theupdateframework.io/img/logos/tuf-stacked-color.png" style="zoom:50%;" />

**소프트웨어 업데이트 시스템 보안을 위한 프레임워크**

- malware나 키 노출 등 기타 공격에 대해 system resilience를 가지도록 설계되었다.

- 소프트웨어 업데이트를 자동으로 식별하고 다운로드하는 메커니즘을 보호하도록 설계된 소프트웨어 프레임 워크다.

- 개발자가 소프트웨어 업데이트 시스템의 보안을 유지하도록 하여, 저장소 또는 signing keys를 손상시키는 공격자로부터 보호를 제공한다.
- 개발자가 모든 소프트웨어 업데이트 시스템에 채택할 수 있는 유연한 프레임 워크 및 사양을 제공한다.



### Security Design Principles

#### 1. Trust

  다운로드한 파일을 신뢰한다는 것은 그 파일이 **악의적인 설계가 없는** 그룹에서 제공되었다고 가정하는 것을 의미한다. 소프트웨어 업데이트 시스템에서 자주 간과되는 **두가지 측면**은 다음과 같다.

- Trust는 영원히 부여되서는 안된다. 갱신되지 않으면 소멸되어야 한다.
- Trust는 균등하게 부여되서는 안된다. 이로 하여금 Trust를 가지고 있는 root 역할이 제공하는 file만을 신뢰할 수 있게 된다.



#### 2. Mitigating Key Rsit(Compromise-Resilience)

  암호화 서명은 secure한 소프트웨어 업데이트 시스템에서 필수적인 요소다. 서명을 작성하는데 사용되는 키의 보안은 클라이언트의 보안에 직접적으로 영향을 끼치게 된다. 그래서 개인 키가 노출되지 않았다고 가정하는것보다, 키가 노출되었을 때 클라이언트를 가능한 안전하게 유지시키는 방법을 고려해야 한다. 

- 빠르고 안전하게 키 교체 및 폐기
- 위험도가 높은 키에 대한 신뢰를 최소화
- 여러 키 사용 및 서명의 임계값/정족수(threshold / quorum)

> quorum : 의사결정하는데 필요한 최소한도의 인원수



#### 3. Integrity

  **TUF**에서 **Integrity**를 보장한다는 것은 단일 파일에만 적용되는 것이 아니라 **전체 저장소에 적용**된다는 것을 의미한다. 클라이언트 시스템에서 개별적으로 다운로드한 단일 파일이 맞는지 확인하는 것도 중요하지만 그들이 다운로드했던 장소인 저장소가 무결하지 확인하는 것도 중요하다.

  예를 들어, 신뢰할 수 있는 그룹에서 제공한 두 파일을 받을 경우 소프트웨어 업데이트 시스템은 두 파일 모두의 최신 버전을 확인하여야 한다.



#### 4.  Freshness

  보안에 관련된 bug fix 업데이트가 잦기 때문에 소프트웨어 업데이트 시스템은 파일의 최신 버전을 얻는 것이 중요하다. 그렇지 않으면 공격자가 클라이언트 시스템을 속여 오래된 버전의 소프트웨어를 설치하거나 업데이트를 사용할 수 없다고 할 수도 있다.

  **Freshness**를 보장한다는 것은 다음과 같다.

- 이전에 본 파일보다 오래된 파일은 절대 허용 안함
- 업데이트를 가져오는데 문제가 있음을 인식



## Vitess

<img src="https://vitess.io/img/logos/vitess.png" style="zoom:25%;" />

  대규모 오픈소스 데이터베이스 인스턴스 클러스터를 구축, 확장 및 관리하기 위한 데이터베이스 솔루션이다. MySQL과 MariaDB를 지원한다. 전용 하드웨어에서와 마찬가지로 퍼블릭 또는 프라이빗 클라우드 아키텍처에서 효과적으로 실행되도록 설계되었다. 중요 SQL 기능을 NoSQL 데이터베이스의 확장성과 결합한다.

> 전용 하드웨어 : 회사가 단독으로 사용할 수 있도록 보유하고 있는 컴퓨팅 리소스 풀

  **Vitess**에는 기본 쿼리 프로토콜을 사용하는 JDBC 및 Go 데이터베이스 드라이버가 포함되어 있다. 또한 거의 모든 다른 언어와 호환되는 MySQL 서버 프로토콜을 구현한다.



### Features

- Performance
  - Connection Pooling : 성능을 최적화하기 위해 여러개의 front-end 응용 프로그램 쿼리를 MySQL 연결 풀에 연결
  - Query de-duping : 실행 중인 쿼리가 실행되는 동안 수신된 동일한 요청에 대해 실행 중인 쿼리 결과를 재사용
  - Transaction manager : 동시 트랜잭션 수를 제한하고 마감 시간을 관리하여 전체 처리량을 최적화

  

- Protection

  - Query rewriting and sanitization(쿼리 다시 쓰기 및 삭제) : 제한을 추가하고 결정적이지 않은 업데이트를 방지
    - 비결정적(non-deterministic) : 같은 입력을 넣어도 경우에 따라 다른 결과가 나오는 기계나 프로그램의 성질
  - Query blacklisting : 규칙을 사용자 지정하여 문제가 발생할 수 있는 쿼리가 데이터베이스에 충돌하지 않도록 방지
  - Query Killer : 데이터를 반환하는데 너무 오래 걸리는 쿼리를 종료
  - Table ACLs : 연결된 사용자를 기준으로 테이블에 대한 ACL(엑세스 제어 목록)을 지정

  

- Monitoring : 성능 분석 도구를 사용하여 데이터 베이스 성능을 모니터링, 진단 및 분석할 수 있다.



- Topology Management Tools

  - 마스터 관리 도구(재분리 핸들)
  - 웹 기반 관리 GUI
  - 여러 데이터 센터 / 지역에서 작동하도록 설계됨

  

- Sharding

  - 사실상 완벽한 동적 재공유
  - 수직 및 수평 sharding 제공
  - 사용자 정의 스키마를 연결할 수 있는 멀티 샤딩 스키마

> sharding : 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법을 의미



### Architecture

  **Vitess** 플랫폼은 일관된 메타데이터 저장소에 의해 지원되는 다수의 서버 프로세스, 명령줄 유틸리티 및 웹 기반 유틸리티로 구성된다. 

![](https://vitess.io/docs/overview/img/architecture.svg)

