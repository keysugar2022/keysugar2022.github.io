---
layout: post
title: Dev Environment(1) Ubuntu 20.04 설치 및 mysql 설치
published: true
categories: post
---
 
 
#### 인터넷 없이 ubuntu 20.04 LTS live server 설치  

유선 인터넷 연결하고

sudo netplan apply 하면 네트워크 활성화됨



우분투 데스크탑 설치


``` bash

sudo apt-get update

sudo apt-get install ubuntu-desktop

```


재부팅하면
``` bash
sudo system reboot


sudo apt-get upgrade
```


mysql 8 설치
``` bash
sudo apt-get install mysql-server




sudo mysql_secure_installation
```
mysql db 초기 보안설정



sudo /etc/init.d/mysql restart



상태 보기

``` bash
systemctl status mysql.service
```

로컬에선 접속되는데    remote  에서  3306  으로 접속안되서   netstat -tlnp  로 확인해보니

localhost:3306 만 열려있다.


그래서


sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf 에서 수정해준다



bind-address 0.0.0.0 으로 수정했는데 이번엔   접속 호스트 주소가 차단되어 있다.


``` sql
select Host,User,plugin,authentication_string FROM mysql.user
```
하면    user root  가 host 필드는  localhost 값으로 설정되어 있다.


모든 곳에서 접속할 수 있는 새  user 만들기
``` sql
create user 'root'@'%' identified by 'eodlf147';

```
모든 권한 부여 하기
``` sql
grant all on *.* to root@'%';
```

적용하기

``` sql
flush privileges;

```