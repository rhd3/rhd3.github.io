# 1. Docker 이미지 관리
## 주요 명령어
- 이미지 다운로드: #docker pull node:16
  > Node.js 16 이미지를 Docker Hub에서 로컬 환경으로 다운로드

- 이미지 확인: #docker images
  > 로컬에 존재하는 모든 이미지를 확인

- 이미지 삭제: #docker rmi <이미지 ID>
  > 사용하지 않는 이미지를 삭제

# 2. Docker 컨테이너 관리
## 주요 명령어

- 컨테이너 생성: #docker create --name chat-app -v $(pwd):/usr/src/app -w /usr/src/app -p 3000:3000 node:16
  > --name: 컨테이너의 이름을 지정
  > -v: 로컬 디렉토리를 컨테이너 내부 디렉토리와 바인드 마운트
  > -w: 컨테이너 내부의 작업 디렉토리 지정
  > -p: 컨테이너의 3000번 포트를 호스트의 3000번 포트와 매핑

- 컨테이너 실행: #docker run -a chat-app
  > 생성된 컨테이너를 실행


- 컨테이너 중지: #docker stop chat-app
  > 실행 중인 컨테이너를 중지

- 컨테이너 삭제: #docker rm chat-app
  > 중지된 컨테이너를 삭제

- 컨테이너 상태 확인: #docker ps -a
  > 모든 컨테이너(실행 중, 중지 상태 포함)를 확인

# 3. Docker 볼륨 관리
도커에서 컨테이너가 호스트 시스템에 파일을 저장할 수 있는 여러가지 방법이 있고, 대표적으로 볼륨, 바인드 마운트 등이 있다.    

[바인드 마운트와 볼륨 비교]

|------|---------|---------|
|특징              | 바인드 마운트                                | Docker 볼륨                          |   
|설정              | 호스트 경로를 명시적으로 지정                  | Docker가 스토리지 위치 관리            |    
|호스트 파일 공유  | 즉시 가능 (파일 실시간 동기화)                 | Docker 내부 데이터로만 사용              |  
|컨테이너 간 공유  | 파일 공유는 가능하지만, 명시적으로 경로 지정 필요 | 컨테이너 간 데이터 공유가 간단           | 
|데이터 보존       | 컨테이너 삭제 시 파일 보존                      | 컨테이너 삭제 후에도 볼륨 데이터 유지     | 

## 주요 Docker 명령어
- 볼륨 생성: #docker volume create chat-app-volume
  > chat-app-volume이라는 이름의 Docker 볼륨을 생성

- 볼륨 사용: #docker run -it -v chat-app-volume:/usr/src/app -p 3000:3000 node:16
  > Docker 볼륨(/usr/src/app)을 컨테이너에 마운트

- 볼륨 확인: #docker volume ls
  > 생성된 모든 볼륨을 확인

- 볼륨 삭제: #docker volume rm chat-app-volume
  > 사용하지 않는 볼륨을 삭제

- 볼륨 데이터 확인: #docker inspect chat-app-volume
  > 볼륨의 상세 정보와 저장 경로를 확인



