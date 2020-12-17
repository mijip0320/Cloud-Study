## k8s

#### k8s 에 들어가기 전에..

---



- 하이퍼바이저란?

> [가상 머신(Virtual Machine, VM)](https://www.redhat.com/ko/topics/virtualization/what-is-a-virtual-machine)을 생성하고 구동하는 소프트웨어입니다. 가상 머신 모니터(Virtual Machine Monitor, VMM)라고도 불리는 하이퍼바이저는 하이퍼바이저 운영 체제와 가상 머신의 리소스를 분리해 VM의 생성과 관리를 지원합니다.

 

- 하이퍼바이저 유형 1

>  네이티브 또는 베어 메탈 하이퍼바이저라고도 불리는 유형 1 하이퍼바이저는 호스트의 하드웨어에서 직접 구동되어 게스트 운영 체제를 관리합니다. 호스트 운영 체제 대신 VM 리소스는 하이퍼바이저에 의해 하드웨어에 직접 예약됩니다. 

>  이러한 유형의 하이퍼바이저는 엔터프라이즈 데이터 센터와 서버 기반 환경에서 가장 일반적으로 사용됩니다.

> KVM, Microsoft Hyper-V, VMware vSphere가 유형 1 하이퍼바이저의 예입니다. KVM은 2007년에 Linux 커널로 통합되었으므로 현대적인 버전의 [Linux](https://www.redhat.com/ko/topics/linux)를 사용하는 경우 이미 KVM 액세스 권한을 갖고 있습니다. 



- 하이퍼바이저 유형2

> 호스트 하이퍼바이저라고도 불리는 유형 2 하이퍼바이저는 기존의 운영 체제에서 소프트웨어 레이어 또는 애플리케이션으로서 구동됩니다.

> 호스트 운영 체제에서 게스트 운영 체제를 추상화하는 방식으로 작동합니다. VM 리소스는 호스트 운영 체제에 따라 예약된 후 하드웨어에 대해 실행됩니다. 

> 유형 2 하이퍼바이저는 개인 컴퓨터에서 여러 개의 운영 체제를 구동하려는 개인 사용자에게 이상적입니다. 

> VMware Workstation과 Oracle VirtualBox는 유형 2 하이퍼바이저의 예입니다. 



- VM 과 Container의 차이점. 구동방식이 어떻게 다른가? 

> 하이퍼바이저는 VM에서 다양한 운영 체제를 구동할 수 있도록 허용하지만, 컨테이너는 단일한 유형의 운영 체제만 구동할 수 있습니다. 예를 들어, Linux 서버를 구동 중인 컨테이너의 경우, Linux 운영 체제만 구동할 수 있습니다. 



- VM

> VMware, VirtualBox와 같이 Host OS위에 Guest OS를 가상화 하는 방식입니다. 예전에는 Guest OS 전체를 가상화하였습니다. 사용법이 간단하지만 무겁고 느려서 운영환경에 적용하기는 어려웠습니다. 그래서 현재는 Xen과 같은 반가상화 방식으로 구성되고 있습니다.



- Container

> 이와 같이 가상화하는 방식은 무겁고 성능 문제가 발생하기에 프로세스를 격리하는 방안이 등장 하였습니다.



> VM은 하이퍼바이저 위에 Guest OS가 올라가는데 그 위에 Binary와 라이브러리 등을 모두 구성해야 하기에 무겁고 성능저하가 발생합니다. (오버헤드 발생) 그런데 도커의 경우에는 하이퍼바이저가 필요 없고 도커엔진만 있으면 그 위에 어플리케이션과 바이너리 및 라이브러리가 포함된 컨테이너만 올리면 되는 구성입니다. 정리하면 OS의 자원인 커널을 공유하지만 어플리케이션 단위로 추상화하여 서로 격리하는 형태입니다



- Docker

> 도커란 오픈소스 가상화 플랫폼을 의미한다.
>
> 서버에서 다양한 OS 환경을 운영하는 것이 가능하다.

> VM은 호스트 OS 위에 게스트 OS를 가상화 하여 사용한다. 게스트 OS는 호스트 OS의 자원을 할당받고 하이퍼바이저를 이용하여 가상화를 진행한다.
>
> VM은 이러한 특성 때문에 OS위에 또 OS를 설치하기 때문에 속도가 저하된다.

>컨테이너 기술은 호스트 OS를 공유하며, 여러개의 컨테이너들이 독립적으로 실행되어 가볍다.
>
>즉, 프로세스를 격리하여 실행된다.



![VM Docker](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/vm-vs-docker.png)



> - 도커는 게스트OS가 존재하지 않는다.
> - 실행환경을 컨테이너로 추상화하여 동일한 인터페이스를 제공한다.
> - 프로그램의 배포 관리가 단순화 된다.



> 컨테이너는 애플리케이션을 환경에 구애 받지 않고 실행하는 기술이라고 할 수 있다.



- Dokcer 주요 개념

> - 이미지
>
> 이미지는 컨테이너 실행에 필요한 파일과 설정 값 등을 포함하고 있으며  상태 값을 가지지 않고 변하지 않는다.
>
> 컨테이너란 이미지를 실행한 상태이며 추가되거나 변경된 값은 컨테이너에 저장된다.
>
> 하나의 이미지에서 여러개의 컨테이너를 생성하는 것이 가능하고 컨테이너에서 아무리 변경점이 생겨도 이미지는 변경되지 않는다.
>
> 즉, 원하는 이미지를 이용하거나 만들어서 여러 컨테이너를 운영하는 것이 가능하다.



![이미지](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/docker-image.png)





> ex) ubuntu 이미지는 ubuntu를 실행하기 위한 모든 파일을 가지고 있다.
>
> MySql 이미지는 MySql 을 실행하는데 필요한 파일과 실행 명령어,포트 등을 가지고 있다. 
>
> 이 외에도 Centos를 기반으로 ruby,go,database,redis,gitlab source.nginx 등을 가지고 있다.



> 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 이것저것 컴파일하고 설치할 필요가 없음. 
>
> 1. Docker 이미지는 레이어의 형태
>
> 2. 변경되는 부분만 따로 이미지가 생성된다.
>
> 3. 생성된 이미지는 부모의 이미지를 바라본다.
>
> 4. 상대적으로 경량화가 가능하다.



> - 컨테이너
>
> 도커의 이미지가 실행된 상태이다.
>
> 격리된 공간에서 프로세스가 동작하는 기술이다.



> 기존의 VM의 무겁고 느린 단점을 해소하기 위해 CPU의 가상화 기술(HVM)을 이용한 KVM(Kernel-based Virtual Machine)과 반 가상화 방식의 Xen 이 등장함.
>
> 이러한 방식은 게스트 OS가 필요하긴 하지만 전가상화가 아니였기 때문에 호스트 가상화 방식에 비해 성능이 향상되었으며, AWS,OpenStack 등의 가상컴퓨팅 기술의 기반이 됨.



> 전가상화든 반가상화든 OS를 추가적으로 설치하는 것은 성능문제가 여전히 존재하였고 이를 개선하기 위해 프로세스를 격리하는 방식이 등장함.
>
> "리눅스 컨테이너" 라고 하며 가볍고 빠르게 동작이 가능하다. (프로세스가 필요한 만큼만 CPU나 메모리를 추가로 사용하여 성능적 손실이 거의 없음.)



> - 레이어 저장방식
>
> 도커의 이미지 같은 경우 실행에 필요한 모든 정보를 가지고 있기 떄문에 용량이 수백MB에 이르며, 처음 이미지를 받는 것은 부담이 되지 않지만, 구성요소가 하나 바뀌었다고 다시 이미지를 내려 받는 것은 비효율적이다.

> 이런 문제를 해결하기 위해서 레이어라는 개념을 사용하고 유니온 파일 시스템을 이용하여 여러개의 레이어를 하나의 파일시스템으로 사용할 수 있게 해준다.
>
> 이미지는 여러 개의 읽기 전용 레이어로 구성되고 파일이 추가되거나 수정되면 새로운 레이어가 생성됨.



![레이어 저장방식](https://t1.daumcdn.net/cfile/tistory/99B114445B39EF2D30)









- Docker 명령어 모음



**이미지 관련 **

이미지 목록 보기

```null
$ sudo docker images
```

이미지 보기

```null
$ sudo docker search [이미지 이름]
```

이미지 받기

```null
$ sudo docker pull [이미지 이름]:[버전]
```

버전: **latest** 를 쓰면 최신 버전으로 받을수 있다.

이미지 삭제

```null
$ sudo docker rmi [이미지 id]
```

컨테이너를 삭제하기 전에 이미지를 삭제 할때, **-f** 옵션을 붙어면 컨테이너도 강제 삭제가 가능하다.

```null
$ sudo docker rmi -f [이미지 id]
```



**컨테이너 관련**

다양한 프로그램(nginx, database, WAS 등)을 컨테이너 라는 격리된 환경을 이용하여 실행시킬수 있습니다.

#### 컨테이너 목록 보기

```null
$ sudo docker ps
```

옵션

- -a : 모든 컨테이너 목록 출력

#### 컨테이너 실행

```null
$ sudo docker run [options] image[:TAG|@DIGEST] [COMMAND] [ARG...]
```

| 옵션   | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| -d     | detached mode 흔히 말하는 백그라운드 모드                    |
| -p     | 호스트와 컨테이너의 포트를 연결 (포워딩)                     |
| -v     | 호스트와 컨테이너의 디렉토리를 연결 (마운트)                 |
| -e     | 컨테이너 내에서 사용할 환경변수 설정                         |
| --name | 컨테이너 이름 설정                                           |
| --it   | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 (컨테이너의 표준 입력과 로컬 컴퓨터의 키보드 입력을 연결) |
| --rm   | 프로세스 종료시 컨테이너 자동 제거                           |
| --link | 컨테이너 연결 [컨테이너 명:별칭]                             |

- ex) $ sudo docker run -i -t --name server ubuntu:latest /bin/bash



컨테이너 시작

```null
$ sudo docker start [컨테이너 id 또는 name]
```

컨테이너 재시작

```null
$ sudo docker restart [컨테이너 id 또는 name]
```

컨테이너 접속

```null
$ sudo docker attach [컨테이너 id 또는 name]
```

컨테이너 정지

```null
$ sudo docker stop [컨테이너 id 또는 name]
```

- Bash Shell에서 `exit` 또는 `Ctrl + D`를 입력하면 컨테이너가 정지된다.
- `Ctrl + P`, `Ctrl + Q`를 차례대로 입력하여 컨테이너를 정지하지 않고, 컨테이너에서 빠져나온다.

컨테이너 삭제

```null
$ sudo docker rm [컨테이너 id 또는 name]
// 모든 컨테이너 삭제
$ sudo docker rm `docker ps -a -q`
```



- Dockerfile

Docker File이란 Docker Image를 만들기 위한 설정 파일



> **FROM** 
>
> 기반이 되는 이미지 레이어입니다.
>
> <이미지 이름>:<태그> 형식으로 작성 
>
> ex) ubuntu:14.04
>
> ```
> FROM openjdk:8-jdk-alpine
> ```



> **COPY**
>
> 파일이나 폴더를 이미지에 복사한다. 
>
> ```
> COPY hello.jar hello.jar
> ```



> **ENV**
>
> 이미지에서 사용할 환경 변수 값을 지정한다.
>
> ```
> ENV PROFILE=local
> ```



> **ENTRYPOINT**
>
> 컨테이너를 구동할 때 실행할 명령어를 지정한다. 
>
> ```
> ENTRYPOINT ["./run.sh"]
> ```



> **MAINTAINER** 
>
> 메인테이너 정보입니다.



> **RUN** 
>
> 도커이미지가 생성되기 전에 수행할 쉘 명령어



> **VOLUME** 
>
> VOLUME은 디렉터리의 내용을 컨테이너에 저장하지 않고 호스트에 저장하도록 설정합니다. 
>
> 데이터 볼륨을 호스트의 특정 디렉터리와 연결하려면 docker run 명령에서 -v 옵션을 사용해야 합니다. 
>
> ex) -v /root/data:/data



> **CMD**
>
> 컨테이너가 시작되었을 때 실행할 실행 파일 또는 셸 스크립트입니다. 
>
> 해당 명령어는 DockerFile내 1회만 쓸 수 있습니다.



> **WORKDIR**  
>
> CMD에서 설정한 실행 파일이 실행될 디렉터리입니다.



> **EXPOSE** 
>
> 호스트와 연결할 포트 번호입니다.



- Docker-Hub

이미지를 생성하면 도커 리포지토리를 이용해서 공유할 수 있다. 다양한 도커 리포지토리가 존재하는데 이 중 대표적인 것이 도커 허브이다. 지금까지 예에서 사용한 nginx나 alpine과 같은 이미지가 위치한 곳이기도 하다. [https://hub.docker.com](https://hub.docker.com/) 사이트에 가입하면 도커 허브에 이미지를 푸시해서 공유할 수 있다.



> 먼저 생성한 이미지에 추가로 태그를 붙인다. 추가 태그에는 리포지토리 계정 이름을 사용한다. 예를 들어 도커 허브 계정이 madvirus라면 madvirus/리포지토리이름:태그 형태를 사용한다. 다음은 simplenode:0.1 이미지에 madvirus/simplenode:0.1 태그를 추가하는 예인데 이때 madvirus라 도커 허브 계정이다.



```
$ docker tag simplenode:0.1 madvirus/simplenode:0.1
```

도커 로그인

```
$ docker login
```

이미지 푸시

```
docker push madvirus/simplenode:0.1
```

이미지 가져와서 사용하기

```
 docker run -d --rm -p 5000:5000 madvirus/simplenode:0.1
```





- Docker-compose





### kubernetis (k8s)

---

https://kubernetes.io/ko/docs/concepts/overview/components/ 



**개요**

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼이다. 쿠버네티스는 선언적 구성과 자동화를 모두 용이하게 해준다. 쿠버네티스는 크고, 빠르게 성장하는 생태계를 가지고 있다.



- 이전의 Deplpoy 방법들

![Deployment 변천사](https://d33wubrfki0l68.cloudfront.net/26a177ede4d7b032362289c6fccd448fc4a91174/eb693/images/docs/container_evolution.svg)



> **전통적인 배포 시대:** 초기 조직은 애플리케이션을 물리 서버에서 실행했었다. 한 물리 서버에서 여러 애플리케이션의 리소스 한계를 정의할 방법이 없었기에, 리소스 할당의 문제가 발생했다. 예를 들어 물리 서버 하나에서 여러 애플리케이션을 실행하면, 리소스 전부를 차지하는 애플리케이션 인스턴스가 있을 수 있고, 결과적으로는 다른 애플리케이션의 성능이 저하될 수 있었다. 이에 대한 해결책은 서로 다른 여러 물리 서버에서 각 애플리케이션을 실행하는 것이 있다. 그러나 이는 리소스가 충분히 활용되지 않는다는 점에서 확장 가능하지 않았으므로, 물리 서버를 많이 유지하기 위해서 조직에게 많은 비용이 들었다.



> **가상화된 배포 시대:** 그 해결책으로 가상화가 도입되었다. 이는 단일 물리 서버의 CPU에서 여러 가상 시스템 (VM)을 실행할 수 있게 한다. 가상화를 사용하면 VM간에 애플리케이션을 격리하고 애플리케이션의 정보를 다른 애플리케이션에서 자유롭게 액세스 할 수 없으므로, 일정 수준의 보안성을 제공할 수 있다.
>
> 가상화를 사용하면 물리 서버에서 리소스를 보다 효율적으로 활용할 수 있으며, 쉽게 애플리케이션을 추가하거나 업데이트할 수 있고 하드웨어 비용을 절감할 수 있어 더 나은 확장성을 제공한다. 가상화를 통해 일련의 물리 리소스를 폐기 가능한(disposable) 가상 머신으로 구성된 클러스터로 만들 수 있다.
>
> 각 VM은 가상화된 하드웨어 상에서 자체 운영체제를 포함한 모든 구성 요소를 실행하는 하나의 완전한 머신이다.



> **컨테이너 개발 시대:** 컨테이너는 VM과 유사하지만 격리 속성을 완화하여 애플리케이션 간에 운영체제(OS)를 공유한다. 그러므로 컨테이너는 가볍다고 여겨진다. VM과 마찬가지로 컨테이너에는 자체 파일 시스템, CPU 점유율, 메모리, 프로세스 공간 등이 있다. 기본 인프라와의 종속성을 끊었기 때문에, 클라우드나 OS 배포본에 모두 이식할 수 있다.



- Why 쿠버네티스?

컨테이너는 애플리케이션을 포장하고 실행하는 좋은 방법이다. 프로덕션 환경에서는 애플리케이션을 실행하는 컨테이너를 관리하고 가동 중지 시간이 없는지 확인해야한다. 예를 들어 컨테이너가 다운되면 다른 컨테이너를 다시 시작해야한다. 이 문제를 시스템에 의해 처리한다면 더 쉽지 않을까?

그것이 쿠버네티스가 필요한 이유이다! 쿠버네티스는 분산 시스템을 탄력적으로 실행하기 위한 프레임 워크를 제공한다. 애플리케이션의 확장과 장애 조치를 처리하고, 배포 패턴 등을 제공한다. 예를 들어, 쿠버네티스는 시스템의 카나리아 배포를 쉽게 관리 할 수 있다.



> 쿠버네티스는 다음을 제공한다.

>- **서비스 디스커버리와 로드 밸런싱** 쿠버네티스는 DNS 이름을 사용하거나 자체 IP 주소를 사용하여 컨테이너를 노출할 수 있다. 컨테이너에 대한 트래픽이 많으면, 쿠버네티스는 네트워크 트래픽을 로드밸런싱하고 배포하여 배포가 안정적으로 이루어질 수 있다.
>- **스토리지 오케스트레이션** 쿠버네티스를 사용하면 로컬 저장소, 공용 클라우드 공급자 등과 같이 원하는 저장소 시스템을 자동으로 탑재 할 수 있다.
>- **자동화된 롤아웃과 롤백** 쿠버네티스를 사용하여 배포된 컨테이너의 원하는 상태를 서술할 수 있으며 현재 상태를 원하는 상태로 설정한 속도에 따라 변경할 수 있다. 예를 들어 쿠버네티스를 자동화해서 배포용 새 컨테이너를 만들고, 기존 컨테이너를 제거하고, 모든 리소스를 새 컨테이너에 적용할 수 있다.
>- **자동화된 빈 패킹(bin packing)** 컨테이너화된 작업을 실행하는데 사용할 수 있는 쿠버네티스 클러스터 노드를 제공한다. 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)를 쿠버네티스에게 지시한다. 쿠버네티스는 컨테이너를 노드에 맞추어서 리소스를 가장 잘 사용할 수 있도록 해준다.
>- **자동화된 복구(self-healing)** 쿠버네티스는 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, '사용자 정의 상태 검사'에 응답하지 않는 컨테이너를 죽이고, 서비스 준비가 끝날 때까지 그러한 과정을 클라이언트에 보여주지 않는다.
>- **시크릿과 구성 관리** 쿠버네티스를 사용하면 암호, OAuth 토큰 및 SSH 키와 같은 중요한 정보를 저장하고 관리 할 수 있다. 컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿 및 애플리케이션 구성을 배포 및 업데이트 할 수 있다.



- 기타

쿠버네티스는 전통적인, 모든 것이 포함된 Platform as a Service(PaaS)가 아니다. 쿠버네티스는 하드웨어 수준보다는 컨테이너 수준에서 운영되기 때문에, PaaS가 일반적으로 제공하는 배포, 스케일링, 로드 밸런싱과 같은 기능을 제공하며, 사용자가 로깅, 모니터링 및 알림 솔루션을 통합할 수 있다. 하지만, 쿠버네티스는 모놀리식(monolithic)이 아니어서, 이런 기본 솔루션이 선택적이며 추가나 제거가 용이하다. 쿠버네티스는 개발자 플랫폼을 만드는 구성 요소를 제공하지만, 필요한 경우 사용자의 선택권과 유연성을 지켜준다.



**쿠버네티스 컴포넌트 **

쿠버네티스를 배포하면 클러스터를 얻는다.

> 쿠버네티스 클러스터는 컨테이너화된 애플리케이션을 실행하는 [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)라고 하는 워커 머신의 집합. 
>
> 모든 클러스터는 최소 한 개의 워커 노드를 가진다. 워커 노드는 애플리케이션의 구성요소인 [파드](https://kubernetes.io/ko/docs/concepts/workloads/pods/)를 호스트한다.

>  [컨트롤 플레인](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-control-plane)은 워커 노드와 클러스터 내 파드를 관리한다. 프로덕션 환경에서는 일반적으로 컨트롤 플레인이 여러 컴퓨터에 걸쳐 실행되고, 클러스터는 일반적으로 여러 노드를 실행하므로 내결함성과 고가용성이 제공된다.



![쿠버네티스 클러스터 다이어그램](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)





- 컨트롤 플레인 컴포넌트

컨트롤 플레인 컴포넌트는 클러스터에 관한 전반적인 결정을 수행하고 클러스터 이벤트를 감지하고 반응한다.



>  kube-apiserver
>
> 쿠버네티스의 API를 노출하는 쿠버네티스 컨트롤 플레인 컴포넌트이다. 쿠버네티스 API 서버의 주요 구현은 kube-apiserver이다.
>
> kube-apiserver는 수평적으로 확장되게 디자인 됨. 



>  etcd
>
> 모든 클러스터 데이터를 담는 쿠버네티스 뒷단의 저장소로 사용되는 일관성·고가용성 키-값 저장소.



>  kube-sheduler
>
> [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)가 배정되지 않은 새로 생성된 [파드](https://kubernetes.io/ko/docs/concepts/workloads/pods/) 를 감지하고, 실행할 노드를 선택하는 컨트롤 플레인 컴포넌트.



>  kube-controller-manager
>
> [컨트롤러](https://kubernetes.io/ko/docs/concepts/architecture/controller/)를 구동하는 마스터 상의 컴포넌트.
>
> - 노드 컨트롤러: 노드가 다운되었을 때 통지와 대응에 관한 책임을 가진다.
> - 레플리케이션 컨트롤러: 시스템의 모든 레플리케이션 컨트롤러 오브젝트에 대해 알맞은 수의 파드들을 유지시켜 주는 책임을 가진다.
> - 엔드포인트 컨트롤러: 엔드포인트 오브젝트를 채운다(즉, 서비스와 파드를 연결시킨다.)
> - 서비스 어카운트 & 토큰 컨트롤러: 새로운 네임스페이스에 대한 기본 계정과 API 접근 토큰을 생성한다.



>  cloud-controller-manager
>
> 클라우드별 컨트롤 로직을 포함하는 쿠버네티스 [컨트롤 플레인](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-control-plane) 컴포넌트이다. 클라우트 컨트롤러 매니저를 통해 클러스터를 클라우드 공급자의 API에 연결하고, 해당 클라우드 플랫폼과 상호 작용하는 컴포넌트와 클러스터와 상호 작용하는 컴포넌트를 분리할 수 있다.



- 노드 컴포넌트

노드 컴포넌트는 동작 중인 파드를 유지시키고 쿠버네티스 런타임 환경을 제공하며, 모든 노드 상에서 동작한다.



> kubelet
>
> 클러스터의 각 [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)에서 실행되는 에이전트. Kubelet은 [파드](https://kubernetes.io/ko/docs/concepts/workloads/pods/)에서 [컨테이너](https://kubernetes.io/ko/docs/concepts/containers/)가 확실하게 동작하도록 관리한다.
>
> [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)는 노드의 네트워크 규칙을 유지 관리한다. 이 네트워크 규칙이 내부 네트워크 세션이나 클러스터 바깥에서 파드로 네트워크 통신을 할 수 있도록 해준다.



> kube-proxy
>
> kube-proxy는 클러스터의 각 [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)에서 실행되는 네트워크 프록시로, 쿠버네티스의 [서비스](https://kubernetes.io/docs/concepts/services-networking/service/) 개념의 구현부이다.



> 컨테이너 런타임



- 애드온

애드온은 쿠버네티스 리소스([데몬셋](https://kubernetes.io/ko/docs/concepts/workloads/controllers/daemonset), [디플로이먼트](https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/) 등)를 이용하여 클러스터 기능을 구현한다. 이들은 클러스터 단위의 기능을 제공하기 때문에 애드온에 대한 네임스페이스 리소스는 `kube-system` 네임스페이스에 속한다.



- DNS

클러스터 DNS는 구성환경 내 다른 DNS 서버와 더불어, 쿠버네티스 서비스를 위해 DNS 레코드를 제공해주는 DNS 서버다.



- 학습환경

> kind
>
> minikube



- 운영 환경

> 컨테이너 런타임
>
> 파드가 노드에서 실행될 수 있도록 클러스터의 각 노드에 [컨테이너 런타임](https://kubernetes.io/docs/setup/production-environment/container-runtimes)을 설치해야 한다.
>
> 리눅스 환경의 쿠버네티스에서 여러 공통 컨테이너 런타임을 사용하는 방법



> - [containerd]
>
> 이 섹션에는 `containerd` 를 CRI 런타임으로 사용하는 데 필요한 단계가 포함되어 있다.
>
> 필수 구성 요소를 설치 및 구성한다.
>
> ```
> 
> cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
> overlay
> br_netfilter
> EOF
> 
> sudo modprobe overlay
> sudo modprobe br_netfilter
> 
> # 필요한 sysctl 파라미터를 설정하면 재부팅 후에도 유지된다.
> cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
> net.bridge.bridge-nf-call-iptables  = 1
> net.ipv4.ip_forward                 = 1
> net.bridge.bridge-nf-call-ip6tables = 1
> EOF
> 
> # 재부팅하지 않고 sysctl 파라미터 적용
> sudo sysctl --system
> 
> ```

> ```
> Ubuntu
> 
> # 도커의 공식 GPG 키 추가
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
> # (containerd 설치)
> ## 리포지터리 설정
> ### HTTPS를 통해 리포지터리를 사용할 수 있도록 패키지 설치
> sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
> 
> ## 도커의 공식 GPG 키 추가
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -
> 
> ## 도커 apt 리포지터리 추가
> sudo add-apt-repository \
>     "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
>     $(lsb_release -cs) \
>     stable"
>     
>     ## containerd 설치
> sudo apt-get update && sudo apt-get install -y containerd.io
> 
> # containerd 구성
> sudo mkdir -p /etc/containerd
> sudo containerd config default > /etc/containerd/config.toml
> 
> # containerd 재시작
> sudo systemctl restart containerd
> 
> 
> ```
>
> ```
> Centos
> 
> # (containerd 설치)
> ## 리포지터리 설정
> ### 필요한 패키지 설치
> sudo yum install -y yum-utils device-mapper-persistent-data lvm2
> 
> ## 도커 리포지터리 추가
> sudo yum-config-manager \
>     --add-repo \
>     https://download.docker.com/linux/centos/docker-ce.repo
>     
>     # containerd 설치
> sudo yum update -y && sudo yum install -y containerd.io
> 
> ## containerd 구성
> sudo mkdir -p /etc/containerd
> sudo containerd config default > /etc/containerd/config.toml
> 
> # containerd 재시작
> sudo systemctl restart containerd
> 
> ```
>
> ```
> Window
> 
> # (Install containerd)
> # download containerd
> cmd /c curl -OL https://github.com/containerd/containerd/releases/download/v1.4.0-beta.2/containerd-1.4.0-beta.2-windows-amd64.tar.gz
> cmd /c tar xvf .\containerd-1.4.0-beta.2-windows-amd64.tar.gz
> 
> ​```powershell
> # 추출 및 구성
> Copy-Item -Path ".\bin\" -Destination "$Env:ProgramFiles\containerd" -Recurse -Force
> cd $Env:ProgramFiles\containerd\
> .\containerd.exe config default | Out-File config.toml -Encoding ascii
> 
> # 구성을 검토한다. 설정에 따라 조정할 수 있다.
> # - sandbox_image (쿠버네티스 pause 이미지)
> # - cni bin_dir 및 conf_dir locations
> Get-Content config.toml
> 
> # containerd 시작
> .\containerd.exe --register-service
> Start-Service containerd
> 
> ```
>
> 





> - [CRI-O]
>
> 다음의 Ubuntu 운영 체제에서 CRI-O를 설치하려면, 환경 변수 `OS` 를 아래의 표에서 적절한 필드로 설정한다.
>
> | 운영 체제    | `$OS`           |
> | ------------ | --------------- |
> | Ubuntu 20.04 | `xUbuntu_20.04` |
> | Ubuntu 19.10 | `xUbuntu_19.10` |
> | Ubuntu 19.04 | `xUbuntu_19.04` |
> | Ubuntu 18.04 | `xUbuntu_18.04` |
>
> 
> 그런 다음, `$VERSION` 을 사용자의 쿠버네티스 버전과 일치하는 CRI-O 버전으로 설정한다. 예를 들어, CRI-O 1.18을 설치하려면, `VERSION=1.18` 로 설정한다. 사용자의 설치를 특정 릴리스에 고정할 수 있다. 버전 1.18.3을 설치하려면, `VERSION=1.18:1.18.3` 을 설정한다.
>
> 그런 다음, 아래를 실행한다.
>
> 
>
> ```
> Ubuntu
> 
> cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
> deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /
> EOF
> cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list
> deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /
> EOF
> 
> curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -
> curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers-cri-o.gpg add -
> 
> apt-get update
> apt-get install cri-o cri-o-runc
> 
> sudo systemctl daemon-reload
> sudo systemctl start crio
> ```
>
> 
>
> 다음의 Centos 운영 체제에서 CRI-O를 설치하려면, 환경 변수 `OS` 를 아래의 표에서 적절한 필드로 설정한다.
>
> | 운영 체제       | `$OS`             |
> | --------------- | ----------------- |
> | Centos 8        | `CentOS_8`        |
> | Centos 8 Stream | `CentOS_8_Stream` |
> | Centos 7        | `CentOS_7`        |
>
> 
> 그런 다음, `$VERSION` 을 사용자의 쿠버네티스 버전과 일치하는 CRI-O 버전으로 설정한다. 예를 들어, CRI-O 1.18을 설치하려면, `VERSION=1.18` 로 설정한다. 사용자의 설치를 특정 릴리스에 고정할 수 있다. 버전 1.18.3을 설치하려면, `VERSION=1.18:1.18.3` 을 설정한다.
>
> 그런 다음, 아래를 실행한다.
>
> ```
> Centos
> 
> sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/devel:kubic:libcontainers:stable.repo
> sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo
> sudo yum install cri-o
> 
> sudo systemctl daemon-reload
> sudo systemctl start crio
> ```



> - [도커]
>
> ```
> ubuntu
> 
> # (도커 CE 설치)
> ## 리포지터리 설정
> ### apt가 HTTPS로 리포지터리를 사용하는 것을 허용하기 위한 패키지 설치
> sudo apt-get update && sudo apt-get install -y \
>   apt-transport-https ca-certificates curl software-properties-common gnupg2
> # 도커 공식 GPG 키 추가:
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -
> 
> # 도커 apt 리포지터리 추가:
> sudo add-apt-repository \
>   deb [arch=amd64] https://download.docker.com/linux/ubuntu \
>   $(lsb_release -cs) \
>   stable"
>   
>   # 도커 CE 설치
> sudo apt-get update && sudo apt-get install -y \
>   containerd.io=1.2.13-2 \
>   docker-ce=5:19.03.11~3-0~ubuntu-$(lsb_release -cs) \
>   docker-ce-cli=5:19.03.11~3-0~ubuntu-$(lsb_release -cs)
>   
>   # 도커 데몬 설정
> cat <<EOF | sudo tee /etc/docker/daemon.json
> {
>   "exec-opts": ["native.cgroupdriver=systemd"],
>   "log-driver": "json-file",
>   "log-opts": {
>     "max-size": "100m"
>   },
>   "storage-driver": "overlay2"
> }
> EOF
> 
> # /etc/systemd/system/docker.service.d 생성
> sudo mkdir -p /etc/systemd/system/docker.service.d
> 
> # 도커 재시작
> sudo systemctl daemon-reload
> sudo systemctl restart docker
> 
> # 부팅시 도커 시작
> sudo systemctl enable docker
> 
> ```
>
> ```
> Centos
> 
> # (도커 CE 설치)
> ## 리포지터리 설정
> ### 필요한 패키지 설치
> sudo yum install -y yum-utils device-mapper-persistent-data lvm2
> ## 도커 리포지터리 추가
> sudo yum-config-manager --add-repo \
>   https://download.docker.com/linux/centos/docker-ce.repo
>   # 도커 CE 설치
> sudo yum update -y && sudo yum install -y \
>   containerd.io-1.2.13 \
>   docker-ce-19.03.11 \
>   docker-ce-cli-19.03.11
>   
>   ## /etc/docker 생성
> sudo mkdir /etc/docker
> 
> # 도커 데몬 설정
> cat <<EOF | sudo tee /etc/docker/daemon.json
> {
>   "exec-opts": ["native.cgroupdriver=systemd"],
>   "log-driver": "json-file",
>   "log-opts": {
>     "max-size": "100m"
>   },
>   "storage-driver": "overlay2",
>   "storage-opts": [
>     "overlay2.override_kernel_check=true"
>   ]
> }
> EOF
> # /etc/systemd/system/docker.service.d 생성
> sudo mkdir -p /etc/systemd/system/docker.service.d
> # 도커 재시작
> sudo systemctl daemon-reload
> sudo systemctl restart docker
> # 부팅시 도커 시작
> sudo systemctl enable docker
> ```
>
> 





## CNCF (CLOUD NATIVE COMPUTING FOUNDATION)

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

- Kubernetes



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





