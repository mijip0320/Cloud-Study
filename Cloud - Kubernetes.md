# Kubernetes ( k8s )

![](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/kubernetes-logo.png)

### 쿠버네티스란 ? 

- 컨테이너를 쉽고 빠르게 배포 / 확장하고 관리를 자동화해주는 오픈소스 플랫폼



## 쿠버네티스 기본 개념

 ![](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/desired-state.png)

  쿠버네티스에서 가장 중요한 것은 **desired state - 원하는 상태**라는 개념이다. **원하는 상태**라 함은 관리자가 바라는 환경을 의미하고 좀 더 구체적으로는 얼마나 많은 웹서버가 떠있으면 좋은지, 몇 번 포트로 서비스하기를 원하는지 등을 말한다.

  쿠버네티스는 복잡하고 다양한 작업을 하지만 자세히 들여다보면 **current state - 현재 상태**를 모니터링하면서 관리자가 설정한 **원하는 상태**를 유지하려고 내부적으로 여러 작업을 하는 로직을 가지고 있다.

  이러한 개념 때문에 관리자가 서버를 배포할 때 직접적인 동작을 명령하지 않고 상태를 선언하는 방식을 사용한다. 예를 들어 "nginx 컨테이너를 실행해줘. 그리고 80 포트로 오픈해줘."는 현재 상태를 원하는 상태로 바꾸기 위한 **명령 ( imperative )**이고 "80 포트를 오픈한 nginx 컨테이너 1개를 유지해줘"는 원하는 상태를 **선언 ( declarative )**한 것이다.

```bash
docker run # 명령
kubectl create # 상태 생성
```

  쿠버네티스의 핵심은 **상태**이며 쿠버네티스를 사용하려면 어떤 상태가 있고 어떻게 상태를 선언하는지를 알아야 한다.





### Kubernetes Object

  쿠버네티스는 상태를 관리하기 위한 대상을 오브젝트로 정의한다.



#### 1. Pod

<img src="https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/pod.png" style="zoom:50%;" />

  쿠버네티스에서 배포할 수 있는 가장 작은 단위로, 한 개 이상의 컨테이너와 스토리지, 네트워크 속성을 갖는다. **Pod**에 속한 컨테이너는 스토리지와 네트워크를 공유하고 서로 localhost로 접근할 수 있다. 컨테이너를 하나만 사용하는 경우도 반드시 **Pod**로 감싸서 관리한다.



#### 2. ReplicaSet

<img src="https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/replicaset.png" style="zoom:50%;" />

  **Pod**를 여러 개(한 개 이상) 복제하여 관리하는 오브젝트다. **Pod**를 생성하고 개수를 유지하려면 반드시 **ReplicaSet**을 사용해야 한다. **ReplicaSet**은 복제할 개수, 개수를 체크할 라벨 선택자, 생성할 **Pod**의 설정값(템플릿) 등을 가지고 있다. 직접적으로 **ReplicaSet**을 사용하기보다는 **Deployment** 등 다른 오브젝트에 의해서 사용되는 경우가 많다



#### 3. Service

  네트워크와 관련된 오브젝트다. **Pod**를 외부 네트워크와 연결해주고 여러 개의 **Pod**를 바라보는 내부 로드 밸런서를 생성할 때 사용한다. 내부 DNS에 서비스 이름을 도메인으로 등록하기 때문에 **서비스 디스커버리** 역할도 한다.



#### 4. Volume

  저장소와 관련된 오브젝트다. 호스트 디렉토리를 그대로 사용할 수 있고 EBS 같은 스토리지를 동적으로 생성하여 사용할 수 있다. 사실상 인기있는 대부분의 저장 방식을 지원한다.



#### 5. Deployment

  **ReplicaSet**과 **Pod**의 배포를 관리하는 오브젝트다. **Deployment**는 ReplicaSet의 상위 오브젝트이기 때문에 **Deployment**를 생성하면 해당 Deployment에 대응하는 ReplicaSet도 함께 생성된다. 따라서 Deployment를 사용하면  Pod와 ReplicaSet을 직접 생성할 필요가 없다.

```yaml
apiVersion: apps/vq
kind: Deployment
metadata:
	name: my-nginx-deployment
spec:
	replicas: 3
	selector:
		matchLabels:
			app: my-nginx
		template:
			metadata:
				name: my-nginx-pod
				labels:
					app: my-nginx
			spec:
				containers:
					- name: nginx
					  image: nginx:1.10
					  ports:
					  - containerPort: 80
```



### Object Spec - YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: example
spec:
	containers:
		- name: busybox
			image: busybox:1.25
```

  오브젝트의 명세는 YAML 파일로 정의하고 여기에 오브젝트의 종류와 원하는 상태를 입력한다. 이러한 명세는 생성, 조회, 삭제로 관리할 수 있기 때문에 REST API로 쉽게 노출할 수 있다. 접근 권한 설정도 같은 개념을 적용하여 누가 어떤 오브젝트에 어떤 요청을 할 수 있는지 정의할 수 있다.



### 쿠버네티스 배포방식

  쿠버네티스는 애플리케이션을 배포하기 위해 **원하는 상태(desired state )**를 다양한 오브젝트에 라벨을 붙여 정의(yaml)하고 API 서버에 전달하는 방식을 사용한다.

   "컨테이너를 2개 배포하고 80 포트로 오픈해줘"

라는 간단한 작업을 위해 다음과 같은 구체적인 명령을 전달해야 한다.

> "컨테이너를 Pod로 감싸고 type=app, app=web이라는 라벨을 달아줘. type=app, app=web이라는 라벨이 달린 Pod가 2개 있는지 체크하고 없으면 Deployment Spec에 정의된 템플릿을 참고해서 Pod를 생성해줘. 그리고 해당 라벨을 가진 Pod를 바라보는 가상의 서비스 IP를 만들고 외부의 80 포트를 방금 만든 서비스 IP랑 연결해줘."





## 쿠버네티스 아키텍처

 

### 서버 - 클라이언트 구조

: 컨테이너를 관리하는 에이전트를 만들고 중앙에서 API를 이용하여 원격으로 관리하는 구조

<img src="https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/server-agent.png" style="zoom:50%;" />

### 마스터 - 노드 구조

<img src="https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/master-node.png" style="zoom:50%;" />

  쿠버네티스는 전체 클러스터를 관리하는 **마스터**와 컨테이너가 배포되는 **노드**로 구성되어 있다. 모든 명령은 **API Server**를 호출하고 노드는 마스터와 통신하면서 필요한 작업을 수행한다. 



#### Master

  마스터 서버는 다양한 모듈이 확장성을 고려하여 기능별로 쪼개져 있는 것이 특징이다. 관리자만 접속할 수 있도록 보안 설정을 해야 하고 마스터 서버가 죽으면 클러스터를 관리할 수 없기 때문에 보통 3대를 구성하여 안정성을 높인다. AWS EKS같은 경우 마스터를 AWS에서 자체관리하여 안정성을 높였고 개발 환경이나 소규모 환경에선 마스터와 노드를 분리하지 않고 같은 서버에 구성하기도 한다.



##### Master 구성요소

<img src="https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/kubernetes-master.png" style="zoom:33%;" />

###### 1. API 서버 kube-apiserver

  API 서버는 모든 요청을 처리하는 마스터의 핵심 모듈이다. kubectl의 오청뿐만 아니라 내부 모듈의 요청도 처리하며 권한을 체크하여 요처을 거부할 수 있다. 실제로 하는 일은 **원하는 상태**를 **key-value** 저장소에 저장하고 저장된 상태를 조회하는 일이다. Pod를 노드에 할당하고 상태를 체크하는 일은 다른 모듈로 분리되어 있다. 노드에서 실행 중인 컨테이너의 로그를 보여주고 명령을 보내는 등 디버거 역할도 수행한다.



###### 2. 분산 데이터 저장소 etcd

  **RAFT 알고리즘**을 이용한 **key-value** 저장소다. 여러 개로 분산하여 복제할 수 있기 때문에 안정성이 높고 속도도 빠르다. 단순히 값을 저장하고 읽는 기능뿐 아니라 **watch** 기능이 있어 어떤 상태가 변경되면 바로 체크하여 로직을 실행할 수 있다.

  클러스터의 모든 설정, 상태 데이터는 여기 저장되고 나머지 모듈은 **stateless**하게 동작하기 때문에 **etcd**만 잘 백업해두면 언제든지 클러스터를 복구할 수 있다. **etcd**는 오직 **API 서버**와 통신하고 다른 모듈은 **API 서버**를 거쳐 **etcd** 데이터에 접근한다.

> RAFT 알고리즘 : 뗏목처럼 운용중인 여러 서버들 중 일부에 장애가 발생하더라도 제 기능을 유지하도록 하는 **합의 알고리즘 (합의 프로토콜)**이다. Raft와 같은 합의 알고리즘이 적용된 시스템은 특정 서버에 장애가 발생하더라도 전체 서비스를 중단하지 않고 서버를 복구할 수 있다.



**스케쥴러, 컨트롤러** : API 서버는 요청을 받으면 etcd 저장소와 통신할 뿐 실제로 상태를 바꾸는 건 스케쥴러와 컨트롤러다. 현재 상태를 모니터링하다가 원하는 상태와 다르면 각자 맡은 작업을 수행하고 상태를 갱신한다.



###### 3. 스케쥴러 kube-scheduler

  할당되지 않은 Pod를 여러가지 조건(필요한 자원, 라벨)에 따라 적절한 노드 서버에 할당해주는 모듈이다.



###### 4. 큐브 컨트롤러 kube-controller-manager

  

###### 5. 클라우드 컨트롤러 cloud-controller-manager





#### Node

  노드 서버는 마스터 서버와 통신하면서 필요한 Pod를 생성하고 네트워크와 볼륨을 설정한다. 실제 컨테이너들이 생성되는 곳으로 수백, 수천대로 확장할 수 있다. 각각의 서버에 라벨을 붙여 사용목적을 정의할 수 있다.