---
title: Environment Settings
category: Et Cetera
order: 11
---

### Sdkman

##### sdkman 설치
```bash
$curl -s "https://get.sdkman.io" |bash
```
##### sdkman 초기화
```bash
$source ~/.sdkman/bin/sdkman-init.sh
```
##### sdkman를 통한 환경설정
```bash
$sdk version               ## sdkman 버전확인

$sdk list java             ## 자바 버전 리스트조회
$sdk install 11.0.17-zulu  ## 원하는 버전의 자바 설치
$sdk use java 11.0.17-zulu ## 디폴트 버전으로 설정

$sdk list gradle           ## 그래들 버전 리스트 조회
$sdk install gradle 7.3.1  ## 원하는 버전의 그래들 설치
$sdk use gradle 7.3.1      ## 디폴트 버전으로 설정

```

### Docker

Docker는 컨테이너화된 애플리케이션을 관리하기 위한 오픈 소스 플랫폼. 

|Docker의 장점||
|--|--|
|운영 표준화|Docker는 컨테이너화된 애플리케이션을 동일한 방식으로 실행하고 관리할 수 있도록 표준화된 환경을 제공. 이는 개발 환경과 운영 환경 간의 일관성을 유지하고, 애플리케이션을 다른 환경에서도 쉽게 실행할 수 있도록 도움.|
|높은 이식성| Docker 컨테이너는 애플리케이션과 그 종속성을 패키지화하여 독립적으로 실행할 수 있도록 함. 이는 애플리케이션을 다른 시스템 또는 클라우드 환경으로 쉽게 이동할 수 있게 해줌. 또한, 호스트 시스템의 운영체제나 환경 설정에 구애받지 않고 동일한 방식으로 실행될 수 있기 때문에 개발 및 테스트 과정을 단순화할 수 있음|
|비용 절감| Docker는 가상화 기술을 사용하여 하드웨어 자원을 효율적으로 활용할 수 있도록 도와줌. 여러 애플리케이션을 단일 서버에서 실행할 수 있으며, 리소스 공유 및 가상화를 통해 더 효율적으로 작동할 수 있음. 또한, 빠른 배포와 확장을 통해 인프라 관리 비용을 줄일 수 있음|


[컨테이너]
<div class="content-box">
시스템의 다른 어플리케이션이나 다른 부분들에 영향을 주지 않는 격리된 공간.
실행환경,라이브러리, 시스템도구, 코드 등을 포함. 
</div>

[이미지]
<div class="content-box">
컨테이너 생성과 관련된 내용이 포함된 템플릿 (ReadOnly)
한번생성된 이미지는 변경이 불가하다. 
</div>

[도커레지스트리]
<div class="content-box">
도커이미지를 저장하고 다운로드 할 수 있는 저장수 
</div>

##### Docker에서 자주 쓰는 명령어

|Command|Description|
|--|--|
|$docker image pull|Registry에서 이미지 가져오기|
|$docker image rm|이미지 제거|
|$docker image ls<br>$docker images|이미지 조회|
|$docker build|Docker File로부터 도커 이미지 빌드|
|$docker start CONTAINER_ID|컨테이너 실행|
|$docker restart CONTAINER_ID|컨테이너 재시작|
|$docker stop CONTAINER_ID|컨테이너 종료|
|$docker run [OPTIONS] IMAGE|컨테이너 생성+실행|        
|$docker ps|현재 실행되고있는 컨테이너 및 상태 조회|
|$docker exec -it CONTAINER_ID /bin/bash|실행중인 컨테이너에 명령어 실행|
|$docker logs CONTAINER|컨테이너 로그 확인|


##### 사용예시 (mySql)
`docker image pull mysql` : mysql 이미지 불러오기
```bash
hyunsoojo@HYUNSOOui-MacBook-Pro:~$docker image pull mysql
Using default tag: latest
latest: Pulling from library/mysql
90e2fb2facff: Pull complete 
ba60eb20fd5f: Pull complete 
4f509402d469: Pull complete 
496c2cfa6815: Pull complete 
8ec1dfa9522c: Pull complete 
6dec7ba896f8: Pull complete 
dc9ff75362b0: Pull complete 
73e4682f9014: Pull complete 
9ffdeecd6fb6: Pull complete 
a4346ccfb53f: Pull complete 
434c13bc32de: Pull complete 
Digest: sha256:d6164ff4855b9b3f2c7748c6ec564ccff841f79a7023db0f9293143481a44b6e
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```
`$docker images` : 이미지 조회
```bash
hyunsoojo@HYUNSOOui-MacBook-Pro:~$docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
mysql        latest    05db07cd74c0   4 days ago   565MB
```
`docker run` : 컨테이서 생성 + 실행
```bash
hyunsoojo@HYUNSOOui-MacBook-Pro:~$docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=rootroot -d -p 3306:3306 mysql:latest
9aa2706093021cd3c1724cc59387349902b63f4f415956bc72ca8a168d7cde43
```
`docker ps` : 실행중인 컨테이너 조회
```bash
hyunsoojo@HYUNSOOui-MacBook-Pro:~$docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED         STATUS         PORTS                               NAMES
9aa270609302   mysql:latest   "docker-entrypoint.s…"   7 seconds ago   Up 5 seconds   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-container
```

`docker exec -it mysql-container /bin/sh` : 실행중인 컨테이너에 명령어 실행
```bash
hyunsoojo@HYUNSOOui-MacBook-Pro:~$docker exec -it mysql-container /bin/sh
sh-4.4# 
sh-4.4# 
sh-4.4# ls 
bin  boot  dev	docker-entrypoint-initdb.d  entrypoint.sh  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
sh-4.4# cd etc
sh-4.4# ls
GREP_COLORS	   bindresvport.blacklist  dnf		gnupg	  host.conf  issue.net	   libaudit.conf  my.cnf	 nsswitch.conf.bak  passwd-   profile	 rc2.d		 resolv.conf  shadow   subuid		   xattr.conf
X11		   chkconfig.d		   environment	group	  hostname   krb5.conf	   libssh	  my.cnf.d	 openldap	    pkcs11    profile.d  rc3.d		 rpc	      shadow-  swid		   xdg
aliases		   crypto-policies	   ethertypes	group-	  hosts      krb5.conf.d   localtime	  mysql		 opt		    pki       protocols  rc4.d		 rpm	      shells   sysconfig	   xinetd.d
alternatives	   csh.cshrc		   exports	gshadow   init.d     ld.so.cache   login.defs	  netconfig	 oracle-release     pm	      rc.d	 rc5.d		 sasl2	      skel     system-release	   yum.repos.d
bash_completion.d  csh.login		   filesystems	gshadow-  inputrc    ld.so.conf    motd		  networks	 os-release	    popt.d    rc0.d	 rc6.d		 selinux      ssl      system-release-cpe
bashrc		   default		   gcrypt	gss	  issue      ld.so.conf.d  mtab		  nsswitch.conf  passwd		    printcap  rc1.d	 redhat-release  services     subgid   terminfo
sh-4.4# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.33 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql> show databases
    -> ;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> exit
Bye
sh-4.4# exit
exit
hyunsoojo@HYUNSOOui-MacBook-Pro:~$
```

### Feign

Feign은 Java에서 RESTful 웹 서비스 클라이언트를 구현하기 위한 라이브러리. 
Spring Cloud에서 사용되며, Spring 애플리케이션에서 간편하게 원격 HTTP API를 호출할 수 있는 방법을 제공.<br>

Feign은 런타임 시에 인터페이스를 기반으로 REST 클라이언트를 동적으로 생성하는 프록시 기반 접근 방식을 사용. <br>Feign 인터페이스는 실제 원격 서비스의 엔드포인트, HTTP 메서드, 경로, 요청 및 응답 형식 등을 정의하는 어노테이션을 사용하여 구성. *주로 마이크로서비스 아키텍처에서 서비스 간 통신을 위해 활용.*

|Feign 사용 시 이점 ||
|--|--|
|간편한 선언적 API| Feign 인터페이스를 통해 REST API 호출을 선언적으로 정의할 수 있습니다. 메서드에 어노테이션을 추가하여 요청 URL, HTTP 메서드, 요청 및 응답 형식을 지정할 수 있습니다.|
|인터페이스 기반 프록시 생성| Feign은 런타임 시에 Feign 인터페이스를 기반으로 동적으로 REST 클라이언트를 생성합니다. 개발자는 구현 코드를 작성할 필요 없이 인터페이스를 통해 API를 사용할 수 있습니다.|
|통합된 로드 밸런싱| Spring Cloud의 서비스 디스커버리 기능과 통합되어 Feign은 로드 밸런싱을 자동으로 수행할 수 있습니다. 서비스의 여러 인스턴스 중 하나를 선택하여 API 호출을 수행하므로 확장성과 내결함성을 제공합니다.|
|재시도 및 오류 처리| Feign은 기본적으로 재시도 메커니즘과 오류 처리 기능을 제공합니다. 장애가 발생한 경우 일시적인 문제일 수 있으므로 재시도를 통해 요청을 자동으로 다시 시도할 수 있습니다.|


##### Spring에서 feign사용하기

gradle에 아래와 같이 의존성을 추가해주면 feign을 사용할 수 있다. 

```java
dependencies {
    
ext{
    set('springCloudVersion',"2021.0.1")
}

    implementation "org.springframework.cloud:spring-cloud-starter-openfeign"
}


dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
    }
}
```