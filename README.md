
<img src="https://capsule-render.vercel.app/api?type=waving&color=999999&height=300&section=header&text=DB-Backup-Automization&fontSize=50&fontColor=FFFFFF&animation=fadeIn&width=1200" width="1200" />


<br>

|<img src="https://github.com/DoomchitYJ.png" width="220" />|<img src="https://github.com/imhaeunim.png" width="220" />|<img src="https://github.com/jinhyunpark929.png" width="220" />|<img src="https://github.com/letmeloveyou82.png" width="220" />|
|:-:|:-:|:-:|:-:|
|박영진<br/>[@DoomchitYJ](https://github.com/DoomchitYJ)|임하은<br/>[@imhaeunim](https://github.com/imhaeunim)|박진현<br/>[@jinhyunpark929](https://github.com/jinhyunpark929)|최윤정<br/>[@letmeloveyou82](https://github.com/letmeloveyou82)|

<br>

## 📍 Contents
- [1️⃣ Goals](#1%EF%B8%8F⃣-goals)
- [2️⃣ Architecture](#2%EF%B8%8F⃣-architecture)
- [3️⃣ Skills](#3%EF%B8%8F⃣-skills)
- [4️⃣ Expectations](#4%EF%B8%8F⃣-expectations)
- [5️⃣ How to do](#5%EF%B8%8F⃣-how-to-do)
- [6️⃣ Trouble Shooting](#6%EF%B8%8F%E2%83%A3-trouble-shooting)
- [7️⃣ Retrospective](#7%EF%B8%8F%E2%83%A3-retrospective)

<br>

## 1️⃣ Goals

### 🚀 1. 컨테이너 기반의 데이터베이스 배포 및 관리 최적화

- **Docker Compose**를 활용하여 Spring Boot & MySQL 환경을 쉽게 배포

- MySQL 데이터를 영구 저장하여 **컨테이너 재시작 시에도 데이터 유지**

### 💾 2. MySQL 데이터 백업 자동화

- cron + bash script를 활용한 **자동 백업**으로 데이터 유실 방지

- mysqldump를 사용한 데이터베이스 스냅샷 저장 및 **무결성** 유지

- 7일 이상 된 백업 자동 삭제를 통해 **스토리지 관리 최적화**

### 🌎 3. 확장성과 운영 환경 고려

- 다중 인스턴스 배포 가능 (Spring Boot 컨테이너 여러 개 사용)

- MySQL 컨테이너 **healthcheck** 설정으로 안정적인 서비스 보장

- Docker **Volume과 네트워크 설정**으로 데이터 일관성 및 보안 유지

<br>

## 2️⃣ Architecture
<br>

![docker-compose](https://github.com/user-attachments/assets/93a9b7dd-957b-4e4a-85f3-75de29aaded5)


## 3️⃣ Skills

### **Tech Stack**

<div>
  <img src="https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/docker--compose-1488C6?style=for-the-badge&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/cron-333333?style=for-the-badge&logo=linux&logoColor=white">
  <img src="https://img.shields.io/badge/ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white">
  <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
  <img src="https://img.shields.io/badge/springboot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white">
</div>

<br>

### **Co-work Stack**

<div>
<img src="https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white">
<img src="https://img.shields.io/badge/github-303a50?style=for-the-badge&logo=github&logoColor=white">
<img src="https://img.shields.io/badge/slack-e01e5a?style=for-the-badge&logo=slack&logoColor=white">
</div>

<br>


## 📁 **Project File Structure (`ex.. 07.dockerComposeMission`)**

```
07.dockerComposeMission/
├── db-backup/                   # 📂 MySQL 백업 파일 저장 폴더
│   ├── fisa_backup_20250321_000001.sql  # 📄 MySQL 백업 파일 (Timestamp 포함)
│   ├── fisa_backup_20250322_000001.sql
│   ├── fisa_backup_20250323_000001.sql
|
├── backup.sh                     # 📜 MySQL 자동 백업 스크립트
├── docker-compose.yml             # ⚙️ Docker Compose 설정 파일
├── Dockerfile                     # 🛠️ Spring Boot 앱 Docker 빌드 파일
└── step06_SpringDataJPA-0.0.1-SNAPSHOT.jar  # 🚀 Spring Boot 애플리케이션 실행 파일 (JAR)

```
### 🔹 **설명**

- `db-backup/`
    - MySQL 데이터베이스 백업 `.sql` 파일이 저장되는 폴더
  
    - `backup.sh` 실행 시, 새로운 `.sql` 파일이 생성됨
    
- `backup.sh`
    - MySQL DB를 백업하는 스크립트
      
    - 특정 주기(`cron`)로 실행 가능 (예: 자정 시간마다)
- `backup.log`
    - 백업 파일이 잘 수행되었는지 관리자가 확인하기 위한 로그 파일
- `docker-compose.yml`
    - Docker 컨테이너(앱, DB) 구성 및 실행 설정
- `Dockerfile`
    - Spring Boot 애플리케이션의 컨테이너 이미지를 빌드하는 설정
- `step06_SpringDataJPA-0.0.1-SNAPSHOT.jar`
    - Spring Boot 애플리케이션 실행 JAR 파일

## 4️⃣ Main Flow

### 📌 docker-compose

💡 **데이터 영속성과 DB 백업을 위한 볼륨 설정**

### ✅ Dockerfile

```docker
# Java 17 경량 JRE 기반 이미지 사용
FROM eclipse-temurin:17-jre-alpine

# 작업 디렉토리 설정
WORKDIR /app

# 애플리케이션 JAR 파일 복사
COPY step06_SpringDataJPA-0.0.1-SNAPSHOT.jar app.jar

# 환경 변수 설정 (포트 변경이 용이하도록)
ENV SERVER_PORT=8080

# ✅ INIT_MODE 에 따라 ddl-auto 설정 분기
ENTRYPOINT ["sh", "-c", "java -jar app.jar --spring.jpa.hibernate.ddl-auto=${INIT_MODE}"]
```

### ✅ docker-compose.yml

```yaml
services:
  db:
    container_name: mysqldb
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: [root 비밀번호]
      MYSQL_DATABASE: [데이터베이스]
      MYSQL_USER: [사용자]
      MYSQL_PASSWORD: [비밀번호]
    volumes:
      - mysql-data:/var/lib/mysql       # 🟢 MySQL 데이터 볼륨(Named Volume)
      - ./db-backup:/backup             # 🟢 백업 파일 공유 볼륨(호스트 마운트(Bind Mount))
    networks:
      - spring-mysql-net
    healthcheck:
      test: ['CMD-SHELL', 'mysqladmin ping -h 127.0.0.1 -u root --password=$${MYSQL_ROOT_PASSWORD} || exit 1']
      interval: 10s
      timeout: 2s
      retries: 100

  app1:
    container_name: springbootapp01
    build:
      context: .
      dockerfile: ./Dockerfile
    ports:
      - "8081:8080"  # ✅ 새로운 포트 매핑
    environment:
      MYSQL_HOST: db
      MYSQL_PORT: 3306
      MYSQL_DATABASE: [데이터베이스]
      MYSQL_USER: [사용자]
      MYSQL_PASSWORD: [비밀번호]
      INIT_MODE: create # ✅ 데이터 베이스 생성 시 table도 함께 생성
    depends_on:
      db:
        condition: service_healthy
    networks:
      - spring-mysql-net

  app2:
    container_name: springbootapp02
    build:
      context: .
      dockerfile: ./Dockerfile
    ports:
      - "8082:8080"  # ✅ 새로운 포트 매핑
    environment:
      MYSQL_HOST: db
      MYSQL_PORT: 3306
      MYSQL_DATABASE: [데이터베이스]
      MYSQL_USER: [사용자]
      MYSQL_PASSWORD: [비밀번호]
      INIT_MODE: create # ✅ 데이터 베이스 생성 시 table도 함께 생성
    depends_on:
      db:
        condition: service_healthy
    networks:
      - spring-mysql-net
      
networks:
  spring-mysql-net:
    driver: bridge

volumes:
  mysql-data:  # 🟢 Docker named volume 선언

```

### ⚙️ Docker Compose 핵심 설정 및 기능 설명

1️⃣ **데이터 영속성 및 백업 설정**

- `mysql-data:/var/lib/mysql` → **MySQL 데이터를 유지하는 Named Volume** (컨테이너 삭제 후에도 데이터 보존)
  
- `./db-backup:/backup` → **백업 파일을 호스트 디렉토리와 공유**하여 접근 가능
- `healthcheck` → **DB가 정상 실행될 때까지 대기하여 안정적인 연결 보장**

2️⃣ **Spring Boot 앱 컨테이너 (`app01`, `app02`)**

- `depends_on: service_healthy` → **DB가 실행된 후 앱 실행**
  
- `ports` → **앱 별로 다른 포트 설정 (`8081:8080`, `8082:8080`)**
- `INIT_MODE=create` → **초기 테이블 자동 생성, 이후 유지하여 데이터 보존**

3️⃣ **네트워크 & 볼륨 설정**

- `networks: bridge` → **Spring Boot와 MySQL이 같은 네트워크에서 통신 가능하도록 설정**
  
- `mysql-data` → **DB 데이터를 안전하게 유지하여 컨테이너 재시작 시에도 정보 보존**

### ✅ docker compose 실행

```bash
docker-compose up -d
```

![image](https://github.com/user-attachments/assets/5af4f229-01e9-48c9-b840-de41838ec413)


curl 명령어를 통해 두 개의 Spring Boot 애플리케이션이 같은 DB를 사용하고 있음을 확인할 수 있음.

---

### 📌 DB 백업 자동화

💡 백업 스크립트(`backup.sh`)를 생성 후 `crontab`에 등록하여 **자동화 구성**

### 1. `backup.sh` 파일 생성

```bash
nano backup.sh
```

**📄 `backup.sh` (실행 스크립트)**

```bash
#!/bin/bash

# 절대 경로로 설정 (호스트 기준)
BACKUP_DIR="/home/ubuntu/06.dockerCompose/db-backup"

# 백업 파일 이름 (날짜/시간 포함)
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="fisa_backup_${TIMESTAMP}.sql"

# 디렉토리가 없으면 생성
mkdir -p "${BACKUP_DIR}"

# 백업 실행
echo "📦 MySQL 백업 중... (${BACKUP_FILE})"
docker exec mysqldb /usr/bin/mysqldump -u root -proot fisa > "${BACKUP_DIR}/${BACKUP_FILE}"

# 결과 확인
if [ $? -eq 0 ]; then
  echo "✅ 백업 완료: ${BACKUP_DIR}/${BACKUP_FILE}"
  
  # 🔥 7일 이상된 백업 파일 자동 삭제
  find ${BACKUP_DIR} -name "*.sql" -type f -mtime +7 -exec rm {} \;
  echo "🧹 7일 이상된 백업 파일 정리 완료"
else
  echo "❌ 백업 실패"
fi
```

- **풀 백업을 주기적으로 하고, 오래된 백업은 자동 삭제**

### 2. 권한 부여 (중요❗)

- 실행 권한

```bash
sudo chmod +x backup.sh
```

- 편집 권한

```bash
sudo chmod 744 /db-backup
```

### 3. 수동 실행 테스트

```bash
./backup.sh
```

> ./db-backup 폴더에 .sql 파일이 생기면 성공!
> 

![image](https://github.com/user-attachments/assets/602d1469-981f-4727-86d9-ea9170acdd5b)

#### 실행 결과 : `./db-backup` 폴더에 생긴 파일

```sql
-- MySQL dump 10.13  Distrib 8.0.41, for Linux (x86_64)
--
-- Host: localhost    Database: fisa
-- ------------------------------------------------------
-- Server version	8.0.41

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!50503 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Table structure for table `people`
--

DROP TABLE IF EXISTS `people`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `people` (
  `age` int NOT NULL,
  `no` bigint NOT NULL,
  `people_name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`no`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `people`
--

LOCK TABLES `people` WRITE;
/*!40000 ALTER TABLE `people` DISABLE KEYS */;
INSERT INTO `people` VALUES (26,1,'김연아');
/*!40000 ALTER TABLE `people` ENABLE KEYS */;
UNLOCK TABLES;

--
-- Table structure for table `people_seq`
--

DROP TABLE IF EXISTS `people_seq`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `people_seq` (
  `next_val` bigint DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `people_seq`
--

LOCK TABLES `people_seq` WRITE;
/*!40000 ALTER TABLE `people_seq` DISABLE KEYS */;
INSERT INTO `people_seq` VALUES (101);
/*!40000 ALTER TABLE `people_seq` ENABLE KEYS */;
UNLOCK TABLES;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2025-03-21  5:21:56

```

### 4. Crontab 설정

**매일 자정에 DB 백업 파일 생성**

```bash
crontab -e
```

```bash
0 0 * * * /home/ubuntu/06.dockerCompose/backup.sh >> /home/ubuntu/06.dockerCompose/backup.log 2>&1
```


## 5️⃣ Trouble Shooting

### **1.** `fisa.people` 테이블이 없어 실행 중 SQL 에러 발생

![image](https://github.com/user-attachments/assets/5ab8f1d7-c8f5-4c61-b900-6e02cb828ef8)

**❗ 원인 :** 

1. `application.properties`에서 `spring.jpa.hibernate.ddl-auto=none` 설정

2. `spring.sql.init.mode=always` 설정

**⇒ Spring Boot 앱 실행 시 `fisa.people` 테이블이 없다는 오류 발생**

```bash
SQLSyntaxErrorException: Table 'fisa.people' doesn't exist
```

**💡 해결 방안 :**

- Docker에서는 MySQL 컨테이너가 따로 실행되므로, **최초 한 번만 `create` 적용 후 `none`으로 변경하면 해결 가능하다.**
  
  1. **초기 실행 시 `ddl-auto=create` 적용**
      - `docker-compose.yml`에서 환경 변수 `INIT_MODE: create` 추가
  2. **그 이후 실행 시 `ddl-auto=none` 적용**
      - `docker-compose.yml`에서 `INIT_MODE: none` 적용
  3. **Dockerfile에서 ENTRYPOINT로 실행 시 `INIT_MODE` 값을 반영**
        
     ```docker
     ENTRYPOINT ["sh", "-c", "java -jar app.jar --spring.jpa.hibernate.ddl-auto=${INIT_MODE}"]
     ```
        
      - 실행 명령어가 가장 높은 우선순위를 가지므로 `application.properties`의 `ddl-auto=none` 설정을 덮어씌울 수 있음.

### 2. 볼륨 내부 폴더에 대한 권한 문제

**❗ 원인 :** 

- 설계 초기에 영속성을 위한 .sql 파일과 백업 파일 모두 볼륨에 저장되도록 설정하였다.
- 그러나, 볼륨은 root 권한이지만 cron은 실행하는 ubuntu의 권한을 가지므로 접근 권한이 없어 에러가 발생하였다.

```yaml
    volumes:
      - mysql-data:/var/lib/mysql
      - mysql-backup:/backup
```

```bash
/home/ubuntu/06.dockerCompose/backup.sh: line 8: /var/lib/docker/volumes/06dockercompose_mysql-backup/_data/db_backup_2025-03-21_11-49-01.sql: Permission denied
```

**💡 해결 방안 :**

- 볼륨 마운트가 아닌 호스트 마운트로 변경한다.
    
    ```yaml
        volumes:
          - mysql-data:/var/lib/mysql       # 🟢 MySQL 데이터 볼륨
          - ./db-backup:/backup             # 🟢 백업 파일 공유 볼륨
    ```
    
- 실제 실무에서는 AWS S3와 같은 다양한 외부 스토리지(필요에 따라 내부)에 저장되도록 한다.
