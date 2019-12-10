---
title: "Docker와 Docker Portainer"
tags: ["Docker"]
---



## Docker

<hr>

- 컨테이너 기반 오픈소스 **가상화 플랫폼**

<br>

### 1. 개념

#### Container

- 격리된 공간에서 프로세스가 동작하게 하는 기술
- **프로세스를 격리** 시켜 작동하는 방식 사용
- 프로그램을 독립된 프로세스간 영향을 미치지 않고 독립적으로 실행

<br>

#### 기존 가상화 OS와 다른 점

- 가상화는 OS 전체를 가상화 하기 때문에 무겁고 용량이 큰 반면, 도커는 프로세스 자체를 격리하기 때문에 추가적인 부하가 적다.
- 가상화는 OS를 생성시점에 필요한 자원을 할당해 놓고 사용하지만, 도커는 필요한 만큼의 자원을 사용하고 반납하기 때문에 자원 이용이 효율적

<br>

> 작동구조(가상환경과 비교). [출처](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

![가상머신과 도커](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/vm-vs-docker.png)

<br>

#### Image

- 컨테이너 생성 및 실행에 필요한 파일과 설정값을 갖고있는 파일
  - Ubuntu 이미지, Mysql 이미지, Nginx 이미지...
- 컨테이너 실행에 필요한 모든 파일과 설정을 갖고 있기 때문에 추가 설치 및 의존성 관리가 필요 없다.
  - 독립된 프로세스 공간에서 Python을 사용하고 싶다면 Python 이미지를 받아서 컨테이너화 해서 사용
- 이미지는 `hub.docker.com` 등의 사이트에서 받아서 사용할 수 있으며, 자신이 만든 이미지를 공유하는 것도 가능

<br>

#### volume

- Docker 내부에서 사용하는 데이터를 컨테이너가 아닌 호스트에 저장하는 방식
- 컨테이너 내부의 폴더와 호스트의 폴더를 동기화 시켜 도커 내부 데이터를 관리

<br>

>작동 구조. [출처](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter06/04)

![img](http://pyrasis.com/assets/images/DockerForTheReallyImpatientChapter06/5.png)

<br>

### 2. 기본 명령어

`docker ps`: 컨테이너 출력. `-a` 를 붙이면 실행을 멈춘 컨테이너도 출력

`docker exec -it <컨테이너 이름> bash`: bash창으로 컨테이너에 접속

<br>

#### 추가 명령어 모음

[도커 명령어 모음](https://velog.io/@wlsdud2194/-Docker-%EB%8F%84%EC%BB%A4-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4-%EB%AA%A8%EC%9D%8C)

<br>

### 3. Docker Portainer

- Docker Container를 GUI환경에서 편하게 관리할 수 있는 도구
- image화 되어 있으며, docker를 통해 설치하고 사용한다.

<br>

> 설치 방법

```bash
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

- image를 받아 호스트의 9000번 port와 게스트의 9000번 포트를 연결
- 게스트와 호스트의 볼륨을 연결 후 작동

<br>





