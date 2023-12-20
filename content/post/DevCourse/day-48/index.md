+++
author = "Seorim"
title =  "Day 48"
slug = "day-48"
date = 2023-12-20T12:07:42+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL",
]
+++

<style>
g1 { color: #79AC78 }
g2 { color: #B0D9B1 }
g3 { color: #D0E7D2 }
g4 { color: #618264 }
o1 { color: #F9B572 }
w1 { color: #FAF8ED }
</style>

# 📋 공부 내용

## Docker Volume

> Container와 Host 시스템이 특정 폴더를 공유함으로써, Container가 사라지더라도 데이터를 보존하는 기능

### Container와 데이터

-   Container가 실행되었다가 중단되면 데이터가 유실됨
-   일회성으로 동작하는게 아니라면, 데이터가 영구적으로 보관되어야 함
    -   ex: MySQL 등과 같은 DB가 container에서 동작하는 것
-   데이터 보존을 위해 사용하는 것이 바로 `Docker Volume`

### Docker Volume 정의

-   Host 시스템 폴더 -> Docker Container 시스템 폴더로 `mount` (=mapping)

    -   Host에서 내용을 바꾸면 Docker Container 쪽에도 반영됨 (속성에 따라 반대도 반영됨)

-   Docker Container 상태와 관계 없이 데이터를 영구적으로 보관
    -   Container가 중단되더라도 데이터가남게 됨

### mount in file system

-   mount & unmount
-   disk같은 물리적인 장치를 파일 시스템의 특정 위치와 연결해주는 기능
    -   ex: 외장하드를 연결할 경우 `:E` 처럼 이 장치와 연결된 새로운 폴더가 생김

### Docker Volume Type

**<g1>1. Host Volumes</g1>**

```bash
docker run -v {host_file_system_path}:{container_file_system_path}
```

**<g1>2. Anonymous Volumes</g1>**

-   도커가 알아서 호스트 시스템 폴더를 만들고 연결

```bash
docker run -v {container_file_system_path}
```

**<g1>3. Named Volumes</g1>**

-   가장 많이 사용되고 선호되는 방식
-   하나의 Volume을 다수의 Container에서 공유하는 것도 가능
-   도커가 만들어 마운트하는 호스트 시스템 폴더에 `이름을 지정`할 수 있음

```bash
docker run -v {name}:{container_file_system_path}
# readonly volume 설정 옵션, default는 읽기&쓰기 가능
docker run -v {name}:{container_file_system_path}:ro
```

### Image 생성 시 Docker Volume을 사용하는 방법

**<g1>1. Dockerfile</g1>**

-   VOLUME command를 사용하며, anonymous volume만 지정 가능

**<g1>2. docker-compose</g1>**

-   Host Volume이나 Named Volume을 사용

## Docker Volume 실습

### nginx container without volume

**<g1>1. nginx container를 다운받고 서버를 실행</g1>**

-   command

```bash
# -d 옵션으로 detach하여 서버가 백그라운드에서 실행되게 함
# --p 8081:80 으로 포트포워딩 하여 호스트에서 8081 포트로 연결할 수 있게 함
docker run -d --name=nginx -p 8081:80 nginx
```

-   http://localhost:8081/에 연결한 웹 브라우저 화면

**<g1>2. 서버에 접속해서 html 파일 수정</g1>**

-   command

```bash
# exec으로 실행중인 nginx container에 연결
# --user=root -it 으로 서버에 root유저로 접속
# sh shell script 실행
docker exec --user=root -it nginx sh

# -----------------서버 내부----------------------
apt update
install nano
# 내용을 Welcome to Docker Volume으로 수정
nano /usr/share/nginx/html/index.html
exit

# ----------------------------------------------
```

-   웹 브라우저 연결 화면

**<g1>3. 재실행 후 파일 확인</g1>**

-   Volume이 지정되지 않은 상태 -> 변경이 적용되지 않고 원래대로 돌아왔음을 알 수 있음

-   command

```bash
# Container 재실행
docker restart nginx

docker exec --user=root -it nginx sh
# -----------------서버 내부----------------------
apt update
install nano
# 내용이 수정 전으로 돌아갔음을 확인할 수 있음
nano /usr/share/nginx/html/index.html
exit
# ----------------------------------------------

```

-   웹 브라우저 연결 화면

### nginx container with volume

**<g1>1. nginx container를 다운받고 서버를 실행(볼륨 사용 설정)</g1>**

-   command

```bash
# -v 옵션을 사용해서 Host Volumes 방식으로 연결
docker run -d --name nginx_demo -p 8080:80 -v /home/sarah/devcourse/nginx/html:usr/share/nginx/html nginx
```

-   http://localhost:8081/에 연결한 웹 브라우저 화면

**<g1>2. html 파일 수정 </g1>**

-   host 시스템 상에서 파일을 직접 수정
-   웹 브라우저 연결 화면

**<g1>3. 재실행 후 파일 확인</g1>**

-   host 시스템 상에서 파일 확인
-   웹 브라우저 연결 화면

# 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

## Docker review quiz

-   ![](image.png)
-   ![](image-1.png)

# ❗ 느낀 점
