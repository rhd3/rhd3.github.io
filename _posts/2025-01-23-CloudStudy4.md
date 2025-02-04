---
layout: post
title: 도커 프로젝트
date: 2025-01-23
categories: CloudStudy
short_description: 클라우드 스터디 2회차
---
   
OS: Rocky Linux 8   
Docker 이미지: node:16   
 
1. node 이미지를 검색 후 다운로드       
![](https://raw.githubusercontent.com/rhd3/rhd3.github.io/main/_posts/img/docker_search.png)

3. Docker 컨테이너 생성      
![](https://raw.githubusercontent.com/rhd3/rhd3.github.io/main/_posts/img/docker_create.png)

```
--name: 컨테이너의 이름을 지정         
-v: 로컬 디렉토리를 컨테이너 내부 디렉토리와 바인드 마운트         
-w: 컨테이너 내부의 작업 디렉토리 지정         
-p: 컨테이너의 3000번 포트를 호스트의 3000번 포트와 매핑     
```


3. 컨테이너 실행      
![](https://raw.githubusercontent.com/rhd3/rhd3.github.io/main/_posts/img/docker_run.png)      
Docker 볼륨을 컨테이너에 마운트 후 실행

4. ps 확인      
![](https://raw.githubusercontent.com/rhd3/rhd3.github.io/main/_posts/img/docker_ps_after_run.png)


5. 실행 결과     
![](https://raw.githubusercontent.com/rhd3/rhd3.github.io/main/_posts/img/tlfgod.png)
