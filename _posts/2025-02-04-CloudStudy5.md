---
layout: post
title: 리소스 제한 및 모니터링하기
date: 2025-01-23
categories: CloudStudy
short_description: 클라우드 스터디 3회차
---
   

# 리소스 제한 및 모니터링하기

## 개요
- Docker는 컨테이너가 CPU, 메모리, Block I/O 등 다양한 리소스를 제한 없이 사용할 수 있으므로, 필요에 따라 명령어 옵션 또는 오케스트레이션 도구(Kubernetes, Docker Swarm 등)로 자원 사용을 제한해야 한다.
- `docker stats`와 `docker events`를 이용해 실시간 모니터링이 가능하다.

---

## 1. 리소스 제한

### 1.1 Memory 제한
- `--memory (-m)`: 최대 메모리 사용량 지정 (예: `-m 512m`)
- `--memory-swap`: 메모리와 스왑의 총 사용량 지정 (예: `--memory-swap 1g`)
- `--memory-reservation`: 소프트 메모리 제한 설정
- `--oom-kill-disable`: OOM Killer 비활성화

#### 사용 예:
```bash
docker run -d -m 512m nginx
docker run -d -m 1g --memory-reservation 500m ubuntu
docker run -d -m 200m --memory-swap 300m ubuntu
docker run -d -m 200m --oom-kill-disable=true ubuntu
```

---

### 1.2 CPU 제한
- `--cpus`: 사용 가능한 CPU 코어 수 지정 (예: `--cpus="1.5"`)
- `--cpuset-cpus`: 특정 CPU 코어 지정 (예: `--cpuset-cpus=0-3`)
- `--cpu-shares`: CPU 우선순위 설정 (기본값 1024, 예: `--cpu-shares=2048`)

#### 사용 예:
```bash
docker run -d --cpus="0.5" ubuntu
docker run -d --cpu-shares=2048 ubuntu
docker run -d --cpu-shares=512 ubuntu
docker run -d --cpuset-cpus=0-3 ubuntu
```

---

### 1.3 Block I/O 제한
- `--blkio-weight`: I/O 가중치 설정 (10~1000)
- `--blkio-weight-device`: 특정 장치의 I/O 가중치 지정 (예: `/dev/sda:200`)
- `--device-write-bps` / `--device-read-bps`: 초당 I/O 속도 제한 (단위: kb, mb, gb)
- `--device-write-iops` / `--device-read-iops`: 초당 IOPS 제한

#### 사용 예:
```bash
docker run -it --rm --blkio-weight 100 ubuntu /bin/bash
docker run -it --rm --device-write-bps /dev/sda:1mb ubuntu /bin/bash
docker run -it --rm --device-write-iops /dev/sda:10 ubuntu /bin/bash
```

---

## 2. 리소스 모니터링

### 2.1 docker stats
- 실행 중인 컨테이너의 CPU, 메모리, 네트워크, I/O 사용 현황을 실시간으로 표시한다.

#### 사용법:
```bash
docker stats
docker stats <컨테이너_ID 또는 이름>
```

---

### 2.2 docker events
- Docker 호스트에서 발생하는 이벤트(컨테이너 시작/종료, 이미지 pull 등)를 실시간으로 출력한다.

#### 사용법:
```bash
docker events
docker events -f container=<컨테이너_이름>
```


### -- 2.3 cAdvisor
- [cAdvisor](https://github.com/google/cadvisor)는 컨테이너의 리소스 사용량과 성능을 웹 UI로 시각적으로 모니터링할 수 있는 도구이다.

