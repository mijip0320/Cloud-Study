## k8s

#### k8s 에 들어가기 전에..

- 하이퍼바이저 

 [가상 머신(Virtual Machine, VM)](https://www.redhat.com/ko/topics/virtualization/what-is-a-virtual-machine)을 생성하고 구동하는 소프트웨어입니다. 가상 머신 모니터(Virtual Machine Monitor, VMM)라고도 불리는 하이퍼바이저는 하이퍼바이저 운영 체제와 가상 머신의 리소스를 분리해 VM의 생성과 관리를 지원합니다.

 

### 유형 1

네이티브 또는 베어 메탈 하이퍼바이저라고도 불리는 유형 1 하이퍼바이저는 호스트의 하드웨어에서 직접 구동되어 게스트 운영 체제를 관리합니다. 호스트 운영 체제 대신 VM 리소스는 하이퍼바이저에 의해 하드웨어에 직접 예약됩니다. 

이러한 유형의 하이퍼바이저는 엔터프라이즈 데이터 센터와 서버 기반 환경에서 가장 일반적으로 사용됩니다.

KVM, Microsoft Hyper-V, VMware vSphere가 유형 1 하이퍼바이저의 예입니다. KVM은 2007년에 Linux 커널로 통합되었으므로 현대적인 버전의 [Linux](https://www.redhat.com/ko/topics/linux)를 사용하는 경우 이미 KVM 액세스 권한을 갖고 있습니다. 

### 유형 2

호스트 하이퍼바이저라고도 불리는 유형 2 하이퍼바이저는 기존의 운영 체제에서 소프트웨어 레이어 또는 애플리케이션으로서 구동됩니다.

호스트 운영 체제에서 게스트 운영 체제를 추상화하는 방식으로 작동합니다. VM 리소스는 호스트 운영 체제에 따라 예약된 후 하드웨어에 대해 실행됩니다. 

유형 2 하이퍼바이저는 개인 컴퓨터에서 여러 개의 운영 체제를 구동하려는 개인 사용자에게 이상적입니다. 

VMware Workstation과 Oracle VirtualBox는 유형 2 하이퍼바이저의 예입니다. 



VM 과 Container의 차이점. 구동방식이 어떻게 다른가? 



하이퍼바이저는 VM에서 다양한 운영 체제를 구동할 수 있도록 허용하지만, 컨테이너는 단일한 유형의 운영 체제만 구동할 수 있습니다. 예를 들어, Linux 서버를 구동 중인 컨테이너의 경우, Linux 운영 체제만 구동할 수 있습니다. 



VM

VMware, VirtualBox와 같이 Host OS위에 Guest OS를 가상화 하는 방식입니다. 예전에는 Guest OS 전체를 가상화하였습니다. 사용법이 간단하지만 무겁고 느려서 운영환경에 적용하기는 어려웠습니다. 그래서 현재는 Xen과 같은 반가상화 방식으로 구성되고 있습니다.



Container

이와 같이 가상화하는 방식은 무겁고 성능 문제가 발생하기에 프로세스를 격리하는 방안이 등장 하였습니다.



VM은 하이퍼바이저 위에 Guest OS가 올라가는데 그 위에 Binary와 라이브러리 등을 모두 구성해야 하기에 무겁고 성능저하가 발생합니다. (오버헤드 발생) 그런데 도커의 경우에는 하이퍼바이저가 필요 없고 도커엔진만 있으면 그 위에 어플리케이션과 바이너리 및 라이브러리가 포함된 컨테이너만 올리면 되는 구성입니다. 정리하면 OS의 자원인 커널을 공유하지만 어플리케이션 단위로 추상화하여 서로 격리하는 형태입니다



#### 쿠버네티스







## CNCF

## CLOUD NATIVE COMPUTING FOUNDATION

2015 년 7 월에 발표된 2016 년 1 월에 정식 출범 한 Cloud Native Computing Foundation (이하 CNCF)는 혼돈스러운 컨테이너와 관련된 다양한 기술적인 문제들을 오픈소스로 해결하는 하는 것을 목표로하고 있습니다.

Cloud Native Computing Foundation 는 대표적으로 Kubernetes 와 Prometheus 와 같은 클라우드 네이티브 오픈소스 기술들을 추진하고 관리하는 단체입니다.



2019년 7월 현재 Graduated Project는 다음과 같습니다.

- Kubernetes ( Orchestration )
- Prometheus ( Monitoring )
- Envoy (Service Proxy)
- CoreDNS (Service Discovery)
- containerd (Container Runtime)
- Fluentd (Logging)



CNCF에서는 프로젝트를 성숙도에 따라서 샌드박스 단계, 인큐베이팅 단계, 졸업 단계로 나눕니다.



졸업 프로젝트

Kubernetes



Envoy



Prometheus



containerd



Fluentd



Jaeger



Helm



CoreDNS



etcd



Harbor



Rook



TiKV



TUF



Vitess



인큐베이팅 프로젝트

Argo

KubeEdge

Buildpacks

Linkerd

CloudEvents

NATS

CNI

Notary

Contour

Open Policy Agent

Cortex

OpenTracing



CRI-O

CRI-O는 OCI (Open Container Initiative) 호환 런타임을 사용할 수 있도록 Kubernetes CRI (Container Runtime Interface)를 구현 한 것입니다. Docker를 kubernetes의 런타임으로 사용하는 것에 대한 간단한 대안입니다. 이를 통해 Kubernetes는 포드 실행을위한 컨테이너 런타임으로 OCI 호환 런타임을 사용할 수 있습니다. 현재는 컨테이너 런타임으로 runc 및 Kata 컨테이너를 지원하지만 원칙적으로 모든 OCI 준수 런타임을 연결할 수 있습니다.

CRI-O는 OCI 컨테이너 이미지를 지원하며 모든 컨테이너 레지스트리에서 가져올 수 있습니다. Docker, Moby 또는 rkt를 Kubernetes의 런타임으로 사용하는 것보다 가벼운 대안입니다.



CRI-O is made up of several components that are found in different GitHub repositories.

CRI-O는 서로 다른 GitHub 저장소에있는 여러 구성 요소로 구성됩니다.



- [OCI compatible runtime ](https://github.com/opencontainers/runtime-tools) OCI 호환 런타임
- [containers/storage](https://github.com/containers/storage) 
- [containers/image](https://github.com/containers/image)
- [networking (CNI)](https://github.com/containernetworking/cni)
- [container monitoring (conmon)](https://github.com/containers/conmon)
- security is provided by several core Linux capabilities



![CRI-O](https://cri-o.io/assets/images/architecture.png)



Operator Framework



Dragonfly



SPIFFE



Falco



SPIRE



gRPC



Thanos



샌드박스 프로젝트

Artifact Hub

Backstage

BFE

Bridge

cert-manager

Chaos Mesh

ChubaoFS

Cloud Custodian

Cloud Development Kit for Kubernetes

CNI-Genie

Crossplane

Dex

Flux

in-toto

k3s

KEDA

Keptn

Keylime

KubeVirt

KUDO

Kuma

kyverno

LitmusChaos

Longhorn

metal3-io

Network Service Mesh

Open Service Mesh

OpenEBS

OpenKruise

OpenMetrics

OpenTelemetry

OpenYurt

Parsec

Porter

Pravega

SchemaHero

Serverless Workflow Specification

Service Mesh Interface

Strimzi

Telepresence

Thnkerbell

Tremor

Virtual kublet

Volcano





