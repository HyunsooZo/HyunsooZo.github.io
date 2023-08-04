---
title: Docker
category: Projects
order: 3
---
### Docker?

![Docker img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAq1MO%2FbtrH2nr9qlg%2FKcbWROKZroqgebfC2LFkB1%2Fimg.png)

<div class="content-box">
Docker란,  <br>
Go언어로 작성된 리눅스 컨테이너 기반으로 하는 오픈소스 가상화 플랫폼

다시 말해 특정한 서비스를 패키징하고 배포하는데 유용한 오픈소스 프로그램

컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는데 필요한 모든 것이 포함되어 있다.

가상 머신에 비해 꼭 필요한 것만 담겨서 구동되기 때문에 이미지를 만들 경우 용량이 대폭 줄어들게 된다.
</div>

##### Docker를 왜 사용할까?

**1. 애플리케이션 독립성을 가진다.** <br>
호스트 OS, 다른 컨테이너와도 독립된 공간을 보장받아 충돌이 발생하지 않는다. <br>

**2. 배포가 용이 하다.**  <br>
컨테이너 내부에 작업 후 배포하려 한다면 도커 이미지로 만들어서 운영서버에 전달만 하면 된다. <br>

**3. 마이크로 서비스 구조로 변화가 쉽다.**  <br>
컨테이너 하나당 하나의 기능을 제공하는 모듈로 만드는 등 조정이 가능하다.

정리하자면, 
Docker을 사용하면 환경에 구애받지 않고 애플리케이션을 신속하게 배포, 확장할 수 있다.

### Docker 구성

##### Docker Image

<div class="content-box">
Docker Image란 컨테이너를 실행할 수 있는 실행파일, 설정 값들을 가지고 있는 것으로,
더 이상 의존성 파일을 컴파일하거나 이것저것 설치할 필요가 없는 상태의 파일.

Image를 컨테이너에 담고 실행시키면 해당 프로세스가 동작한다
</div>

**[Docker Image 생성]**  <br>

도커 이미지는 기존 이미지에 추가적인 구성이 필요할 때 다시 다운로드하는 방법이 아닌 기존이미지에 레이어를 추가하여 구성을 올려주는 방식으로 생성.

한 마디로, 이미지는 여러 개의 읽기 전용 레이어로 구성되고 파일 추가되면 새로운 레이어가 생성되어 추가된다.

도커는 여러 개의 레이어를 묶어 하나의 파일시스템으로 사용할 수 있게 해 준다.

##### Docker Container
<div class="content-box">
도커 컨테이너는 이미지를 실행한 상태를 나타냄. 

컨테이너는 이미지를 실행하여 프로세스가 동작하는 격리된 환경을 제공.

컨테이너는 호스트 운영체제의 커널을 공유하지만, 다른 컨테이너와 격리되어 독립적으로 실행됨. 

이러한 격리된 환경을 통해 애플리케이션과 그 의존성들이 다른 시스템에 영향을 미치지 않고 실행될 수 있다.
</div>

##### Image vs Container

![Docker img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPMq6k%2FbtrUlSuw1Ht%2FTqPlJYbau6e5TPqZHO2Uak%2Fimg.png)

### Docker 설치

##### Mac & Window

**1.**`homebrew-cask` 사용  <br>
Mac의 (GUI를 제공하는) 응용프로그램을 커맨드로 설치해주는 편리한 기능이다. <br>

cask 옵션을 통해 설치한 프로그램들은 기본적으로 Applications 폴더에 들어가게 된다. <br>

이를 이용해 Docker 설치를 진행하면
Desktop on Mac을 설치하고 docker-compose, docker-machine 또한 같이 설치할 수 있다. <br>

```bash
brew install --cask docker
```
**2. 직접 다운로드** <br>

https://www.docker.com/products/docker-desktop/ 에서 직접 다운로드 할 수도 있다.

### Docker 사용

##### Docker 명령어

|명령어| 설명|
|--|--|
|docker -v / docker --version |버전 확인|
|docker images|다운로드된 이미지들 확인	|
|docker pull [이미지]|이미지 다운로드	|
|docker create [옵션]|[이미지]컨테이너 생성|
|docker start [컨테이너]|컨테이너 실행|
|docker restart [컨테이너]|컨테이너 재실행|
|docker attach [컨테이너]|컨테이너 접속|
|docker stop [컨테이너]|컨테이너 정지|
|docker ps|실행중인 컨테이너들|
|docker ps -a|모든 컨테이너들|
|docker rename [기존 컨테이너][새로운 이름]|컨테이너 이름 변경|
|docker rm [컨테이너]컨테이너 삭제|


##### Docker 생성/실행 옵션
```bash
docker run [옵션] [이미지] [커맨드] [다른 사항]
```
|옵션|	설명|
|-d	|Detached Mode 백그라운드 모드|
|-p	|호스트와 컨테이너의 포트를 연결 (포워딩)|
|-v	|호스트와 컨테이너의 디렉토리를 연결 (마운트)|
|-e	|컨테이너 내에서 사용할 환경변수 설정|
|–name|	컨테이너 이름 설정|
|–rm	|프로세스 종료시 컨테이너 자동 제거|
|-it	|-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|–link	|컨테이너 연결 [컨테이너명:별칭]|

### Docker 사용예시

##### 이미지 설치
```bash
# ubuntu 이미지 설치
docker pull ubuntu:20.04
# mysql 이미지 설치
docker pull mysql
# 설치된 이미지 확인
docker images
```
ubuntu 버전 확인 : https://hub.docker.com/_/ubuntu  <br> 
mysql 버전 확인 : https://hub.docker.com/_/mysql/?tab=tags 

##### 컨테이너 생성

```bash
# mysql을 컨테이너 생성 / 서버 실행
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=[password] -d -p 3306:3306 mysql
# ubuntu 컨테이너 생성 / 서버 실행
docker run --name ubuntu-container -it ubuntu
```

##### 상호작용

```bash

# mysql container 접속
docker exec -it mysql-container bash
# ubuntu container 접속
docker exec -it ubuntu-container /bin/bash
```






