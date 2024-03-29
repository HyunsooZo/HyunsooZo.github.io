---
title: Issue Resolvings
category: Et Cetera
order: 12
---
### MariaDB(Mysql)

##### ERROR 2002 (HY000)

`ERROR 2002 (HY000): Can't connect to local server through socket '/tmp/mysql.sock' (2)`
<br>mysql.sock경로 문제로 발생하는 에러.

**해결방법 (1)** <br>

만약 초기 세팅 단계이고 DB를 삭제 후 재설치 해도 상관없는 상황이라면 아래의 방법을 사용!!

```bash
$ brew unlink mysql
$ brew remove mysql
$ brew uninstall mysql
$ brew cleanup
or
$ brew unlink mariadb
$ brew remove mariadb
$ brew uninstall mariadb
$ brew cleanup
# 완전 삭제 (확인차..)
$ sudo rm -rf /usr/local/var/mysql
$ sudo rm -rf /usr/local/bin/mysql*
$ sudo rm -rf /usr/local/Cellar/mysql --/mariadb
$ sudo rm -rf /usr/local/var/homebrew/{mac-user}/mariadb
$ sudo rm -rf /usr/local/etc/my.cnf --설정파일
$ sudo rm -rf /usr/local/etc/my.cnf.d --설정파일
# 이후 재설치 진행! 
$ brew install mariadb
# 재설치 후 확인
$ mysql -u root -p
```
**해결방법 (2)**<br>

혹시 다른 곳에서 3306 포트를 사용하고 있는지 확인 및 종료.

```bash
$ lsof -i :3306
# 혹시 실행중이라면 해당 PID종료
$ kill 41364
# 접속 재 확인
$ mysql -u root -p
```

##### ERROR 1698 (28000)

`ERROR 1698 (28000): Access denied for user 'root'@'localhost'`

<br>root 계정의 비밀번호가 설정되어 있지 않아 발생하는 에러. 

**해결방법**

```bash
# 관리자 계정으로 접속
$ sudo mysql -u root -p
# 비밀번호 재설정
$ ALTER USER 'root'@'localhost' IDENTIFIED BY 'rootroot';
# 변경내용 적용
FLUSH PRIVILEGES;
```

##### UTF 인코딩 설정

**1. 파일복사**

```bash
$ sudo cd /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
```
 
support-files 디렉토리에 my-default.cnf가 존재하지 않을경우 직접 생성

```bash
cd  /usr/local/mysql/support-files
#.cnf생성
touch my-default.cnf
``` 

**2. CharSet추가**

```bash
$ sudo vi /etc/my.cnf 
```

입력모드 전환 후 

```bash
[mysqld]
init_connect=SET collation_connection=utf8_general_ci
init_connect=SET NAMES utf8

[client]
default-character-set=utf8

[mysqldump]
default-character-set=utf8

[mysql]
default-character-set=utf8
```

추가 후 `:wq!` 입력하여 저장

이후 mariadb 재시작!