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

[관련 문서]     
[Docker docs](https://docs.docker.com)    
[Install Docker Engine](https://docs.docker.com/engine/install/)(CentOS)    

### 2.1.2 리눅스에 설치하기  

리눅스에 설치하는 두가지 방법이 있다.    
1. 명령행 설치(dnf 사용)   
2. 도커 사이트에서 제공되는 스크립트를 사용한 설치    
    
도커를 새로 설치하기 위해선 기존에 설치된 도커를 제거해야한다.    
    
### 2.1.2.1 명령어 설치
1. 패키지 db 업데이트 및 확인 진행
```
&#35; dnf check-update    
&#35; dnf update    
```

2. Docker의 CentOS(Rocky Linux) 전용 저장소(repository)를 추가
```
&#35; dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
3. docker-ce 패키지 설치
```
&#35; dnf install docker-ce
```
7. 버전확인
```
&#35; docker version
```   

### 2.1.2.2 스크립트를 사용한 설치
```
&#35; curl -fsSL https://get.docker.com -o get-docker.sh
&#35; sh get-docker.sh
```




