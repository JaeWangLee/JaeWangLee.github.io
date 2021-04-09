---
title: "Database 생성"
excerpt: "Database 생성에 대해 araboza"
toc: true
toc_sticky: true
categories:
  - SQL
tags:
  - SQL
  - Mysql
  - Programming
  - DB
  - Database
  - DBMS
last_modified_at: 2021-03-10 20:25:20
---

## Database 생성하기

※ Macbook, MySQL 기준입니다.  
  다운로드 및 설치에 대해서는 다루지 않겠습니다.

Homebrew를 사용해 설치했다면, 실행과 중지가 간편합니다.  
환경 변수 설정 등이 모두 자동으로 이뤄지기 때문입니다.

### **1. Homebrew를 사용하여 MySQL 데몬을 실행하기**  
   아래와 같이 명령을 수행하면  
   간단하게 mysql을 데몬형태로 실행할 수 있습니다.  
   `brew services start mysql`  
   서비스 재시작도 다음과 같이 간단하게 할 수 있습니다.  
   `brew services restart mysql`  
   중지도 마찬가지로 간단하게 할 수 있습니다.  
   `brew services stop mysql`  

### **2. Database 생성하기**
  1. 우선 mysql 관리자 계정인 root로 DBMS에 접속합니다.  
  ```sql
  mysql -uroot -p
  ```
  >u 뒤에는 접속하고자 하는 계정을. p뒤에는 password를 입력
  >여기서는 password가 없으므로 공란!

  2. 접속했다면, 다음과 같은 명령으로 DB를 생성합니다.
   ```sql
  Mysql> create database DB이름;
  ```
  만일 "connectdb"라는 이름의 db를 생성한다면?
  ```sql
  Mysql> create database connectdb;
  ```

### **3. Database 사용자 생성과 권한 주기**
DB를 생성했다면, 해당 DB를 사용하는 계정을 생성해야한다.  
또한, 해당 계정이 DB를 이용할 수 있는 권한을 줘야 한다.  
아래와 같은 명령을 이용하여 사용자 생성과 권한을 줄 수 있다.  
  
※ mysql 8.0 이상인 경우입니다.

  ```sql
  CREATE USER 'connectuser' @'%' IDENTIFIED BY 'connect123!@#';
  GRANT ALL ON connectdb.* TO 'connectuser' @'%';
  flush privileges;
  ```
> 계정이름은 'connectuser', 암호는 'connect123!@#',  
> 사용하는 db는 'connectdb'인 계정을 생성하는 방법이다.

> DB이름 뒤 *는 **모든 권한**을 의미하며,  
> @' %'는 어떤 클라이언트든 접근가능하다는 의미이다.  
> @' localhost'는 해당 컴퓨터에서만 접근가능하다는 의미  

### **4. 생성한 Database에 접속하기**
아래와 같이 명령을 실행하여 원하는 DB에 접속할 수 있다.  
mysql -h*호스트명* -u*DB명* -p*비밀번호* *DB이름*  
db이름이 connectdb, 계정이 connectuser, 암호가 123인 경우  
ex. mysql -h127.0.0.1 -uconnectuser -p123 connectdb  
>127.0.0.1은 localhost의 주소. IPv4에서의 ip주소를 의미

### **5. 연결끊기**
quit 혹은 exit을 하면 된다.

끝-!

