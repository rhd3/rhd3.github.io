# 3.6 리소스 제한 및 모니터링하기

## -- 개요
- Docker는 컨테이너가 CPU, 메모리, Block I/O 등 다양한 리소스를 제한 없이 사용할 수 있으므로, 필요에 따라 명령어 옵션 또는 오케스트레이션 도구(Kubernetes, Docker Swarm 등)로 자원 사용을 제한해야 한다.
- `docker stats`와 `docker events`를 이용해 실시간 모니터링이 가능하다.

---

## -- 1. 리소스 제한

### -- 1.1 Memory 제한
- `--memory (-m)`: 최대 메모리 사용량 지정 (예: `-m 512m`)
- `--memory-swap`: 메모리와 스왑의 총 사용량 지정 (예: `--memory-swap 1g`)
- `--memory-reservation`: 소프트 메모리 제한 설정
- `--oom-kill-disable`: OOM Killer 비활성화

#### -- 사용 예:
```bash
docker run -d -m 512m nginx
docker run -d -m 1g --memory-reservation 500m ubuntu
docker run -d -m 200m --memory-swap 300m ubuntu
docker run -d -m 200m --oom-kill-disable=true ubuntu
```

---

### -- 1.2 CPU 제한
- `--cpus`: 사용 가능한 CPU 코어 수 지정 (예: `--cpus="1.5"`)
- `--cpuset-cpus`: 특정 CPU 코어 지정 (예: `--cpuset-cpus=0-3`)
- `--cpu-shares`: CPU 우선순위 설정 (기본값 1024, 예: `--cpu-shares=2048`)

#### -- 사용 예:
```bash
docker run -d --cpus="0.5" ubuntu
docker run -d --cpu-shares=2048 ubuntu
docker run -d --cpu-shares=512 ubuntu
docker run -d --cpuset-cpus=0-3 ubuntu
```

---

### -- 1.3 Block I/O 제한
- `--blkio-weight`: I/O 가중치 설정 (10~1000)
- `--blkio-weight-device`: 특정 장치의 I/O 가중치 지정 (예: `/dev/sda:200`)
- `--device-write-bps` / `--device-read-bps`: 초당 I/O 속도 제한 (단위: kb, mb, gb)
- `--device-write-iops` / `--device-read-iops`: 초당 IOPS 제한

#### -- 사용 예:
```bash
docker run -it --rm --blkio-weight 100 ubuntu /bin/bash
docker run -it --rm --device-write-bps /dev/sda:1mb ubuntu /bin/bash
docker run -it --rm --device-write-iops /dev/sda:10 ubuntu /bin/bash
```

---

## -- 2. 리소스 모니터링

### -- 2.1 docker stats
- 실행 중인 컨테이너의 CPU, 메모리, 네트워크, I/O 사용 현황을 실시간으로 표시한다.

#### -- 사용법:
```bash
docker stats
docker stats <컨테이너_ID 또는 이름>
```

---

### -- 2.2 docker events
- Docker 호스트에서 발생하는 이벤트(컨테이너 시작/종료, 이미지 pull 등)를 실시간으로 출력한다.

#### -- 사용법:
```bash
docker events
docker events -f container=<컨테이너_이름>
```

---

### -- 2.3 cAdvisor
- [cAdvisor](https://github.com/google/cadvisor)는 컨테이너의 리소스 사용량과 성능을 웹 UI로 시각적으로 모니터링할 수 있는 도구이다.

---

## -- 3.7 네트워크 관리

## -- 개요
- Docker는 컨테이너 간 통신 및 외부 네트워크와의 연결을 효율적으로 관리하기 위해 다양한 네트워크 드라이버를 제공한다.
- 주요 네트워크 모드는 **브릿지(Bridge) 네트워크**, **오버레이(Overlay) 네트워크**이며, 성능 최적화를 위해 네트워크 설정을 조정할 수도 있다.

---

## -- 1. 브릿지 & 오버레이 네트워크

### -- 1.1 브릿지 네트워크 (Bridge Network)
- 기본적으로 Docker는 `bridge` 네트워크를 사용하여 컨테이너 간 통신을 가능하게 한다.
- 같은 호스트 내에서 실행되는 컨테이너들은 `bridge` 네트워크를 통해 연결된다.
- 각 컨테이너는 `docker0` 인터페이스를 통해 IP를 할당받으며, 필요에 따라 **사용자 정의 브릿지 네트워크**를 만들 수 있다.

#### -- 사용 예:
```bash
# 새로운 브릿지 네트워크 생성
docker network create --driver bridge my_bridge_network

# 컨테이너 실행 시 특정 네트워크에 연결
docker run -d --network my_bridge_network nginx
```

---

### -- 1.2 오버레이 네트워크 (Overlay Network)
- 오버레이 네트워크는 여러 호스트에서 실행되는 컨테이너가 하나의 논리적 네트워크를 공유하도록 한다.
- Docker Swarm을 사용하여 다중 호스트 간의 네트워크를 형성할 때 주로 사용된다.
- VXLAN(Virtual Extensible LAN) 기술을 활용하여 가상 네트워크를 구축한다.

#### -- 사용 예:
```bash
# Swarm 모드 활성화
docker swarm init

# 오버레이 네트워크 생성
docker network create --driver overlay my_overlay_network

# 서비스 실행 시 네트워크 지정
docker service create --network my_overlay_network --name my_service nginx
```

---

## -- 2. 네트워크 성능 최적화

### -- 2.1 MTU 설정 최적화
- MTU(Maximum Transmission Unit)는 한 번에 전송할 수 있는 최대 패킷 크기이다.
- Docker 오버레이 네트워크(VXLAN 등)를 사용할 경우, 추가적인 헤더가 붙어 MTU 값이 줄어들 수 있다.
- 적절한 MTU 값을 설정하여 패킷 단편화(Fragmentation)를 줄이면 네트워크 성능을 개선할 수 있다.

#### -- 사용 예:
```bash
# 현재 MTU 확인
ip link show docker0

# 브릿지 네트워크 생성 시 MTU 값 조정
docker network create --driver bridge --opt com.docker.network.driver.mtu=1450 my_bridge_network
```

---

### -- 2.2 네트워크 드라이버 선택
- Docker는 다양한 네트워크 드라이버를 제공하며, 성능 최적화를 위해 적절한 드라이버를 선택하는 것이 중요하다.
- `host` 네트워크나 `macvlan`을 사용하면 네트워크 오버헤드를 줄일 수 있다.

#### -- 사용 예:
```bash
# host 네트워크 사용 (네트워크 가상화 오버헤드 최소화)
docker run --network host nginx

# macvlan 네트워크 사용 (컨테이너가 물리 네트워크 인터페이스처럼 동작)
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 my_macvlan_network
```

---

### -- 2.3 컨테이너 간 네트워크 경로 최적화
- 불필요한 중간 노드 또는 NAT(Network Address Translation) 단계를 줄이면 컨테이너 간 통신 속도를 개선할 수 있다.
- 직접적인 IP 통신을 활용하고, 필요한 경우 로드 밸런서를 적절히 설정한다.

#### -- 사용 예:
```bash
# 컨테이너의 네트워크 정보 확인
docker network inspect my_bridge_network
```

---

### -- 2.4 네트워크 버퍼 및 시스템 파라미터 튜닝
- 리눅스 커널의 `sysctl` 설정을 조정하여 네트워크 성능을 향상시킬 수 있다.
- TCP 버퍼 크기를 조정하면 네트워크 트래픽 처리량이 증가할 수 있다.

#### -- 사용 예:
```bash
# TCP 버퍼 크기 조정
sysctl -w net.core.rmem_max=26214400
sysctl -w net.core.wmem_max=26214400
```

---

