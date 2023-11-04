---
title: Docker & Jenkins
category: TIL
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
|--|--|
|-d	|Detached Mode 백그라운드 모드|
|-p	|호스트와 컨테이너의 포트를 연결 (포워딩)|
|-v	|호스트와 컨테이너의 디렉토리를 연결 (마운트)|
|-e	|컨테이너 내에서 사용할 환경변수 설정|
|–name|	컨테이너 이름 설정|
|–rmg|프로세스 종료시 컨테이너 자동 제거|
|-it|-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|–link	|컨테이너 연결 [컨테이너명:별칭]|


### Jenkins


##### Jenkins를 왜 사용할까?
<div class="content-box">
Jenkins는 오픈 소스 도구로 무료로 사용할 수 있으며, <br>
확장성이 뛰어나서 다양한 플러그인을 활용하여 CI/CD 파이프라인을 개발할 수 있다.<br>
사용자 친화적인 웹 인터페이스와 다양한 설정 옵션을 제공하여 사용자들이<br>
프로세스를 쉽게 관리하고 제어할 수 있다.
Jenkins는 다양한 언어 및 프레임워크와 통합할 수 있는 풍부한 플러그인 생태계를 제공한다.<br>
즉, Jenkins는 지속적 통합 및 지속적인 전달 (CI/CD) 프로세스를 자동화하고 간소화하기 위한 도구 중 하나이다. CI/CD를 간략히 정리하면 아래와 같다.
</div>

|지속적 통합 <br>(Continuous Integration - CI)| 지속적인 전달<br> (Continuous Delivery - CD)|
|--|--|
|CI는 소프트웨어 개발 프로세스의 중요한 부분. <br>여러 개발자가 동시에 작업하는 경우, 그들의 코드가 서로 충돌하지 않고 원할하게 통합되어야 한다.<br>Jenkins는 코드 변경 사항을 자동으로 통합하고 빌드하며, 테스트를 실행하여 코드 변경 사항의 품질을 유지한다.<br>이로써 개발자들은 자주 코드를 통합하고 문제를 빨리 식별할 수 있다.| CD는 소프트웨어를 지속적으로 고객에게 전달할 수 있도록 하는 프로세스입. <br>개발에서 프로덕션 환경까지의 배포를 자동화하고 안정적으로 수행.<br>Jenkins는 CD 파이프라인을 설정하고 배포 과정을 자동화하는 데 사용됨.<br>이로써 변경 사항을 빠르게 고객에게 제공하고 소프트웨어를 신속하게 업데이트할 수 있음.|

##### Docker 설치(Ubuntu)

- **1. EC2서버 접속** 
다음과 같은 명령어를 입력하여 맥북터미널에서 EC2 서버에 접근할 수 있다. 
```bash
 $ssh -i /Users/hyunsoojo/Downloads/lunch_project.pem ubuntu@13.209.169.74
 ```
<br>

- **2. package 업데이트**
```bash
$sudo apt update
```
<br>

- **3. https관련 패키지 설치**
```bash
$sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
<br>

- **4. docker repository 접근을 위한 gpg 키 설정**
```bash
$sudo su
$curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$exit
```
<br>

- **5. docker repository 등록 후 다시 업데이트**
```bash
$echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$sudo apt update
$sudo apt install docker-ce
$docker --version
```

##### Docker 삭제

- **1. Docker 패키지 및 관련 패키지 제거**
```bash
$sudo apt-get purge docker-ce docker-ce-cli containerd.io
```

- **2. Docker 이미지, 컨테이너, 볼륨, 네트워크 등 삭제**
```bash
$sudo rm -rf /var/lib/docker
```

- **3. Docker 관련 설정 파일 제거**
```bash
$sudo rm -rf /etc/docker
```

- **4. Docker 그룹에서 사용자 제거 (선택 사항)**
```bash
$sudo groupdel docker
```

##### Jenkins image pull
- **1. Docker 에 젠킨스 Pull해오기**
```bash
$sudo docker pull jenkins/jenkins
```
jenkins/jenkins의 앞의 내용은 만들고자 하는 계정 뒤의 내용은 Repository의 이름이다.<br>jenkins계정의 jenkins Repository라고 이해하면 된다.

- **2. 도커에서 젠킨스를 실행**
```bash
$sudo mkdir /home/opendocs/jenkins
$sudo docker run \
--name jenkins \
-d \
-p 8888:8080 \
-p 50000:50000 \
-v /home/opendocs/jenkins:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
-u root \
jenkins/jenkins:lts
```
위와같은 명령어로 도커에서 젠킨스를 설치한 뒤 실행한다.</br>
`-u root` 를 주는 이유는 이후 DIND 를 위함임!

|명령어|설명|
|--|--|
|<span class="emphasis">run</span>|도커에서 image를 생성하는 커맨드|
|<span class="emphasis">-p</span>| publish 옵션 컨테이너 내부 포트를 컨테이너 바깥 서버에서 어떻게 접속해서 사용할것인지 나타내는 설정<br>컨테이너 바깥에서 8088(기본값 8080)이라는 포트를 사용하여 접속하면 컨테이너 내부로 8080으로 접속한다.<br>컨테이너 외부에서 50000을 호출하면 컨테이너 내부에서 50000으로 응답한다.|
|<span class="emphasis">--restart=on-failure</span>|fail 했을 경우 restart한다.|
|<span class="emphasis">-v jenkins_home:/var/jenkins_home</span>| Volume 로컬에서 사용하는 Docker의 실행 환경에서 어떠한 디렉토리와 Docker에 있는 디렉토리와 Mount(연결)작업을 할것인지에 대한 설정<br>Mount를 하지 않을 경우 Docker내부에서 발생된 데이터는 Docker내부에 저장되기 때문에 Docker가 삭제되면 해당 데이터가 같이 삭제된다.<br> 따라서 Docker내부에 저장된 데이터값을 삭제되지 않게하기위해 어딘가에 보관하기 위해 설정한다.<br> Docker가 실행되는 외부에 해당하는 폴더 경로(/var/jenkins_home)를 연결해서 Link를 연결하는 작업|
|<span class="emphasis">jenkins/jenkins:lts-jdk11</span>| docker에서 사용하는 jenkins계정 이름<br> / repository 이름|
|<span class="emphasis">lts-jdk11</span>|  사용하려는 tag이름|
|<span class="emphasis">--name jenkins-server</span>| 만들고자 하는 컨테이너에 이름을  jenkins-server로 부여<br>부여하지 않으면 Docker가 랜덤하게 이름을 생성한다.<br>(Random한 이름만 놓고 봤을 때 해당하는 컨테이너가 무엇인지 알수 없다는 뜻이므로 이름을 부여하는것을 권장.)|

- **2.2 jenkins container 접속하여 docker-ce-cli 설치**

```bash
enkins container 접속
docker exec -it jenkins /bin/bash
``` 

linux 버전 확인

```bash
cat /etc/issue

# --------------- OS --------------------------------
# root@DESKTOP-R4P59B3:/home/opendocs# cat /etc/issue
# Ubuntu 20.04.4 LTS \n \l
# --------------- jenkins Container OS --------------------------------
# root@DESKTOP-R4P59B3:/home/opendocs# docker exec -it jenkins /bin/bash
# root@8fc963af71bb:/# cat /etc/issue
# Debian GNU/Linux 11 \n \l
 
# Docker 설치
## - Old Version Remove
apt-get remove docker docker-engine docker.io containerd runc
## - Setup Repo
apt-get update
apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
## - Install Docker Engine
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

- **3. Docker 프로세스 정상동작 확인**
```bash
docker ps 
```

- **4. docker logs를 통해 password를 확인**
```bash
sudo docker logs jenkins-server
```

- **5. jenkins 파이프라인 작성**
```
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HyunsooZo/wanted-pre-onboarding-backend.git'
            }
        }
        stage('application.yml download') {
            steps {
                withCredentials([file(credentialsId: 'application.yml', variable: 'applicationyml')]) {
                    script {
                        sh 'cp $applicationyml ./src/main/resources/application.yml'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x ./gradlew'
                sh './gradlew clean build'
            }
        }
        stage('Dockerize') {
            steps {
                sh '''
                    docker stop my_container_name || true
                    docker rm my_container_name || true
                    docker rmi my_image_name || true
                    docker build -t my_image_name .
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d --name my_container_name -p 8080:8080 my_image_name'
            }
        }
    }
}
```