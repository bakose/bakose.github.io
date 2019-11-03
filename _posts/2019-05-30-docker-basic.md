---
title:  "Docker 소개"
excerpt: "docker"
toc: true
toc_sticky: true

categories:
  - docker
tags:
  - docker
last_modified_at: 2019-11-02T21:03:00-09:00
---

# Docker

Build, Ship, and Run Any App, Anywhere

# What is?

---

Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications, whether on laptops, data center VMs, or the cloud.

[Docker](http://docker.com)

# LXC

---

## OS Container

- OS 처럼 여러 서비스를 실행
- Namespace, Cgroup 기술 기반
- ex) LXC, 솔라리스 Zones

## Namespace 격리

네임스페이스를 격리하여 Resource 접근을 제한

[Linux namespaces - Wikipedia](https://en.wikipedia.org/wiki/Linux_namespaces)

Type of Namespace

- Mount Namespace : Storage
- PID Namespace : Process ID
- Network Namespace : network interfaces, routing table, etc..
- IPC Namespace : Inter-process communications (pipes and message queues..)
- UNIX Time-Sharing Namespace (UTS) : hostname, domain name
- User Namespace : user and group ID
- Control group(cgroup) : child Cgroup
- Time namespace (proposed)
- Syslog namespace (proposed)

## Cgroup

- Kernal에 포함
- CPU, memory, disk I/O 등 Resource 제어

## LXC 특징

- 커널 공유
- 리얼타임 프로비저닝과 확장성
- 동일 커널 기반의 서비스만 구성가능
- 네이티브 성능
- 프로세스 수준의 격리로 VM보다 보안 수준이 낮다(Kernal 관점)

# Docker 컨테이너

---

## App Container

- LXC OS Container 기반
- 기본적으로 하나의 서비스를 실행
- ex) Docker, Rocket

## Docker 컨테이너 특징

- Immutable 인프라 환경에적합
- Stateless 애플리케이션에 적합
- 상태 정보는 컨테이너 외부에 구성

## LXC와 다른점

Layered File System

- 이동성과 쉬운 공유
- 버전 제어 가능
- 재사용성

Docker 0.9 버전 이후 LXC 의존성 탈피 및 새로운 커널 인터페이스 API 작성

- libcontainer → runC

# Docker 구성 요소

---

- Docker Hub

    컨테이너 이미지 저장과 배포를 위한 저장소

    [Docker Hub](https://hub.docker.com/)

- Docker 이미지

    Application 실행에 필요한 모든 요소들을 포함하는 파일

- Docker 컨테이너

    Docker 이미지로 실행한 인스턴스

- Docker 명령어

    Docker 컨테이너와 이미지에 대한 작업을 수행하는 주요 명령

- Dockerfile

    이미지 생성시 수행할 작업을 정의한 핵심 파일

# Docker Architecture

---

- Docker 데몬

    이미지와 컨테이너 생성, 실행 모니터링

- Docker 클라이언트

    HTTP를 통해 Docker 데몬과 통신

- Docker 레지스트리

    이미지 저장 및 배포

    ![](/assets/images/2019-05-30-docker-basic-docker-arch.jpeg)

# Docker 설치

---

- 표준 리눅스

        curl -sSL https://get.docker.com/ | sh
        sudo systemctl start docker

- Microsoft Windows, Mac OS
- Container-specific Linux

    Container 전용 경량화 OS

    Nano, RedHat Project Atomic, Mesosphere DCOS, VMware Photon

# Docker 레지스트리

---

Docker 레지스트리

- 컨테이너 이미지 저장소로 이미지 배포를 위한 도구

Docker 이미지 배포를 위한 저장소 유형

- Docker Hub 레지스트리 : Docker가 공식적으로 제공하는 저장소
- Private 레지스트리 : 오픈소스 애플리케이션

Docker 레지스트리 구성요소

- Registry : 이미지를 저장하는 서비스
- Repository : 이미지들의 콜렉션

    {registryAddress}/{namespace}/{repositoryName}:{tag}

    ex. insator.reg.com/dev/lst:v2.5.1

- Index : 이미지 검색, 태깅, 인증

# Docker 이미지

---

이미지

- 읽기 전용 템플릿으로 컨테이너 인스턴스를 저장한 파일
- 호스트 사이의 이동성 제공
- Dockerfile로 자신만의 이미지 생성

    인스트럭션을 하나씩 수행하여 최종 이미지 생성

    각 인스트럭션은 이미지에서 개별 레이어 생성

레이어

- 이미지는 연결된 여러 이미지로 구성
- overlay라는 레이어 파일 시스템을 사용
- 레이어는 읽기 전용이며 이미지 다운로드시 기존 레이어 재활용, 변경 부분만 다운로드
- 여러 레이어로 구성된 이미지 → 단일 레이어

        docker container export > /myImage.tar
        docker import myImage.tar imageName:lates

이미지 검색

- Docker Hub
- docker search 명령

        docker search centos
        docker search --filter=stars=10 fedora

이미지 다운로드

- Docker Hub 사이트에서 애플리케이션 이미지 제공
- Docker save 명령으로 이미지를 tar로 저장

        docker image save -o myImage.tar imageName

- docker pull/run 명령 실행

        docker pull sds.redii.net/sds/ubuntu:16.04
        docker container run ubuntu "cat /etc/os-release"

이미지 저장과 로드

- Docker load 명령으로 tar 파일을 이미지로 로드

        docker image load -i /tmp/myImage.tar

# Docker 컨테이너

---

컨테이너

- 이미지를 인스턴스로 실행한 것
- Read Only 인 이미지 Layer 상단에 임시 R/W Layer 추가

    임시 Layer에 Instance 실행에 필요한 정보 추가

    - 파일 시스템 추가
    - 네트워크 설정
    - 리소스 설정
    - 컨테이너에서 실행할 실행 명령...
- 컨테이너의 수정 사항을 이미지에 저장

        docker container commit <container-id> <image-name>

- 이미지를 사용하여 컨테이너 실행

        docker container run [OPTION] <image-name> [COMMAND][ARG...]

- 컨테이너 중지

        docker container stop [OPTION] <container-id or name> #Graceful
        docker container kill [OPTION] <container-id or name> 

    컨테이너 중단되면 모든 프로세스가 중단되고 메모리가 사라짐

    설정과 파일시스템 변경사항은 임시 레이어에 유지

- 컨테이너 재실행

        docker start <container-id>

런타임 모드

- Foreground

    컨테이너 실행 기본모드

    일번적으로 -t 와 -i 옵션을 사용

        docker run -it ubuntu "/bin/bash"

- Detached

    docker run -d 옵션 사용

    컨테이너를 시작시킨 루트 프로세스가 종료되면 컨테이너도 종료

    docker log 명령을 통해 표준 출력 결과 확인

- 컨테이너 재실행

        docker attach <container-id> #명령어 수행 불가
        docker -it exec <container-id> [COMMAND][ARG..]

## Namespace

PID

컨테이너에 대한 PID 네임스페이스 모드 옵션

    --pid=<container-id> #다른 컨테이너의 PID 네임스페이스에 연결
    --pid=host #Host의 PID 네임스페이스 사용
    
    docker run --pid=host -it ubuntu /bin/bash

UTS

    docker --uts=<container-id>
    docker --uts=host

Volume

Host의 Volume을 컨테이너와 공유

-v <host-path>:<container-path>

    docker run -v /app/tmp/data/:/data/ myApp:latest

## 컨테이너 외부 노출

host의 port와 컨테이너가 사용할 port를 매핑하여 외부로 노출

    docker run -d -p 8080:80 myApp:latest

# DOCKERFILE

- 쉽고 간단하며 명확한 구문을 갖는 이미지 생성을 위한 텍스트 파일
- Dockerfile을 이용한 이미지 building

    .dockerignore 파일 권장

    이미지 레이어 최소화

    한 컨테이너 당 한 Application

        docker build -f <dockerfile-name> 
        #같은 경로에 파일명이 dockerfile인 경우 -f 생략가능
        docker build -t myApp:v1.0.0 -t myApp:latest

- DOCKERFILE Sample

        FROM sds.redii.net/ubuntu:latest
        MAINTAINER Jay Park <jaypark@samsung.com>
        LABEL description="My Sample App"
        
        RUN apt-get-update && \
            mkdir /home/apps &&\
            ...
        
        COPY data/test.conf /home/apps/test.conf
        ADD data/java.tar.gz /home/apps/java/
        ADD http://cloud.com/util.jar /home/apps/utils
        
        SHELL ["/home/apps/init.sh","test.conf"]
        
        ENV JDBC_URL=http://***.***...
        ENV userName=guest
        ARG version=v.1.0.0
        
        EXPOSE 80
        
        ENTRYPOINT ["nginx"]
        CMD ["-g","daemon off;"]

- Instruction
    - FROM : base image
    - MAINTAINER : Author의 meta-data
    - COPY : COPY <원본경로>:<대상경로>
    - ADD

        ADD <원본경로>:<대상경로>

        COPY와 달리 네트워크 (URL)에서 파일 복사 가능

        압축 파일은 대상 경로에 압축 해제

    - CMD
    - ENTRYPOINT
    - LABEL : Image의 Meta-data
    - ENV : 환경변수. 컨테이너에 적용
    - ARG : 변수. Image Bulid 시에만 사용
    - EXPOSE : 컨테이너가 사용하는 Port. Meta-data (실제 listen x)
    - RUN : Image 생성 중 수행할 작업 (Layer 생성)
    - USER
    - VOLUME
    - HEALTHCHECK
    - SHELL

---

# More..

Docker Network, Orchestration Tool : SWARM, Kubernetes..

[Docker Command](/docker/docker-commands)