---
layout: post
title: 도커의 개요 및 설치2
date: 2025-01-14
categories: CloudStudy
short_description: 클라우드 스터디 1회차
---

## 2.1 Docker 설치
### 2.1.1 사전 준비
도커를 설치하는 방법은 도커 데스크톱(Docker Desktop)을 이용하는 방법과 리눅스 시스템에 직접 설치하는 두가지 방법이 있다.
1. 도커 데스크톱(Docker Desktop) 설치하기
> 언젠가 추가예정..    
   
2. 리눅스에 Docker 직접 설치하기    
[설치 환경]    
OS: Rocky Linux 8.10    
패키지: docker-ce    

[관련 문서]     
[Docker docs](https://docs.docker.com)    
[Install Docker Engine](https://docs.docker.com/engine/install/)    

### 2.1.2 리눅스에 설치하기  

설치에는 두가지 방법 1. 명령행설치 2. 도커 사이트에서 제공되는 스크립트 설치 방법이 잇음.
새로 설치하기 위해선 기존에 깔려잇는 도커를 제거해야함. 

1. 명령어 설치
2. 패키지 db 업데이트 및 확인 진행

3./ # dnf check-update
4. # dnf update /

5.  Docker의 CentOS(Rocky Linux) 전용 저장소(repository)를 추가
    # dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

6. docker-ce 패키지 설치
   # dnf install docker-ce
7. 버전확인
   #docker version



2. 스크립트 설치
3.     # curl -fsSL https://get.docker.com -o get-docker.sh
    # sh get-docker.sh





