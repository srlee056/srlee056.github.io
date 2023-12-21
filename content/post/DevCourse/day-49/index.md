+++
author = "Seorim"
title =  "Day 49 Docker(4)"
slug = "day-49"
date = 2023-12-21T11:27:56+09:00

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

## Docker Compose

> 여러 Container로 소프트웨어가 구성되는 경우 사용할 수 있는 툴이자 환경설정파일  
> 개별 Container를 따로 관리하는 것 보다 더 생산성이 높음

`docker-compose.yml` or `docker-compose.yaml`

다양한 테스트를 수행할 수 있으며, 일반적으로 dev, test, prod 등 여러 버전을 만들기도 함

But, 그만큼 배워야 할 것이 많고 복잡해짐

### 사용법

- commands

```bash
docker-compose build
docker-compose up
docker-compose pull
docker-compose ps
docker-compose down
docker-compose start
docker-compose stop
docker-compose rm
```

- version

![](image.png)

### docker-compose.y(a)ml

**1. services**

- 프로그램을 구성하는 서비스들

- 각각 Docker Image 지정, Docker Container 실행으로 구성됨  
  (각각 Dockerfile을 갖고 있거나 docker Hub에서 이미지를 다운로드받아야 함)

- 서비스별로 포트번호, 환경변수, 디스크 볼륨 등을 지정

**2. volumes**

**3. networks**

---

**기본 이름이 아닌 다른 이름의 파일을 사용하고 싶다면 `-f` 옵션 사용**

```bash
docker-compose -f docker-compose.dev.yml up
```

### 이미지 생성과 관리

**docker-compose build**

- service들 중 build: {path} 로 지정된 것에 대해 빌드가 진행됨

**docker-compose pull**

- docker hub에서 이미지를 읽어오려 하며, image: {image_name}으로 지정됨

**docker images**

- build 된 이미지들은 개별 이미지 앞에 폴더 이름이 prefix로 붙음

![](image-1.png)

**docker-compose images**

- 컨테이너와 그에 의해 실행되고 있는 이미지들만 보여줌

**docker-compose push**

- docker hub 으로 이미지들을 push하려고 함

### 소프트웨어 시작과 중단

**docker-compose up**

- build(&pull) -> create -> start

**docker-compose down**

- stop + remove (이미지는 남아있음)

**docker-compose stop**

- 컨테이너를 중단

**docker-compose rm**

- 중단상태인 컨테이너들을 삭제

**docker-compose ls**

- docker compose로 실행시킨 컨테이너들을 그룹지어서 보여줌

**docker-compose ps**

- docker compose로 실행시킨 컨테이너들의 상태를 보여줌

### 네트워킹

**docker끼리 네트워크 연결이 필요한 경우**

- sevices에서 지정한 이름으로 host name 생성
- 내부에 DNS 서버가 생성되어 이름을 내부 IP로 변환

**별도 네트워크를 구성하는 경우**

- networks 에 네트워크를 나열하고, 각 네트워크를 적절하게 sevice에 지정해주어야 함

**docker network ls**

![](image-2.png)

## docker compose 실습

### docker-compose.mac.yml

- 읽어올 수 있는 이미지는 dockerhub에서 읽어오며,  
  이미지가 없는 경우엔 dockerfile의 위치를 제공하여 빌드를 진행
- 네트워크를 지정하지 않고 내부 네트워크 사용

```yaml
services:
  vote:
    build: ./vote # 이미지가 없는 경우이므로 빌드를 진행
    # use python rather than gunicorn for local dev
    # 폴더 내의 dockerfile에 지정된 command를 overwrite하는 것
    command: python app.py
    ports:
      - "5001:80"

  result:
    build: ./result
    # use nodemon rather than node for local dev
    entrypoint: nodemon server.js
    ports:
      - "5002:80"

  worker:
    build: ./worker

  redis:
    image: redis:alpine # 이미지를 docker hub에서 읽어옴

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
```

<br>

- 클린업 후 새로 만든 docker compose 파일을 활용하여 컨테이너들을 실행해보기

```bash
docker-compose -f docker-compose.mac.yml up
```

![created docker images](image-3.png)

![Running docker containers](image-4.png)

![left: one vote for dog, righr: result of votes - 100% dogs](image-5.png)

### docker-compose.yml 개선하기 (1)

- 각 service에 적절하게 `네트워크`를 지정해준다. (보안 강화 목적)
- 데이터 보존을 위해 `postgresql db에 볼륨`을 지정해준다.

<details>

  <summary>
    code
  </summary>

```yaml
services:
  vote:
    ...
    networks:
      - back-tier
      - front-tier

  result:
    ...
    networks:
      - back-tier
      - front-tier

  worker:
    ...
    networks:
      - back-tier

  redis:
    ...
    networks:
      - back-tier

  db:
    ...
    networks:
      - back-tier
    volumes:
      - db-data:/var/lib/postgresql/data

networks:
  back-tier:
  front-tier:

volumes:
  db-data:
```

</details>

### docker-compose.yml 개선하기 (2)

<o1>[최종 개선된 .yml 파일](https://github.com/dockersamples/example-voting-app/blob/main/docker-compose.yml)</o1>

**서비스 개선**

- depends_on : 서비스들 간 의존성이 있을 경우, 사전에 실행되어야 하는 서비스를 기술

```yaml
vote:
  depends_on:
    redis: #longform
      condition: service_healthy # service_started, service_completed_successfully
    - db # shortform
```

- healthcheck : 해당 서비스의 건강 상태(잘 작동하고 있는지)를 확인할 수 있도록 기술  
  dockerfile에 기술된 내용을 overwrite 할 수 있음
  - return : service_started, service_healthy, service_completed_successfully

```yaml
vote:
  healthcheck:
    test: ...
    interval: 15s
    timeout: 5s
    ...
```

- volumes : 호스트의 폴더와 container의 폴더를 연결하는, mount하는 기능

```yaml
vote:
  volumes:
    - ./vote:/app
db:
  volumes:
    - "db-data:/var/lib/postgresql/data" #named volume
    - "./healthchecks:/healthchecks" #host volume
```

- environment : 서비스가 컨테이너 안에서 실행될 때 `환경변수`들을 지정 (Dockerfile의 ENV)

  - `map` 문법

  ```yaml
  environment:
    RACK_ENV: development
    SHOW: "true"
      USER_INPUT:
  ```

  - `array` 문법

  ```yaml
  environment:
    - RACK_ENV=development
    - SHOW=true
      - USER_INPUT
  ```

- build : context, dockerfile, args 등 `빌드 관련 정보`를 넘겨줄 수 있음

```yaml
worker:
  build:
    context: ./worker
    ...
```

## airflow의 docker-compose.yml 파일 분석

### x-airflow-common

- 여러 서비스에서 공유하는 `공통 구성`을 정의
- `anchor`(별칭) - &airflow-common
  - 나중에 YML 파일 블록을 `계승 형태로 재사용` 가능하게 해줌
  - 재사용 문법: `<<: *airflow-common`
- *version, services, volumes, networks*를 제외한 최상위 레벨의 키워드는 모두 `anchor`

```yaml
x-airflow-common:
  &airflow-common # 앵커, 별칭
  image: ${AIRFLOW_IMAGE_NAME:-apache/airflow:2.5.1}
  # build: .
  environment:
    &airflow-common-env
    AIRFLOW__CORE__EXECUTOR: CeleryExecutor
    ...
    _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:-}
  volumes:  # 모든 container들은 이 세개의 volume을 공유함, host volumes
    - ${AIRFLOW_PROJ_DIR:-.}/dags:/opt/airflow/dags
    - ${AIRFLOW_PROJ_DIR:-.}/logs:/opt/airflow/logs
    - ${AIRFLOW_PROJ_DIR:-.}/plugins:/opt/airflow/plugins
  user: "${AIRFLOW_UID:-50000}:0"
  depends_on:
    &airflow-common-depends-on
    redis:
      condition: service_healthy
    postgres:
      condition: service_healthy
```

### services

- postgres
- redis
- airflow-webserver
- airflow-scheduler

```yaml
services:
  ...
  airflow-scheduler:
    <<: *airflow-common # 위에서 언급되었던 별칭을 사용
    command: scheduler
    healthcheck:
      test: ["CMD-SHELL", 'airflow jobs check --job-type SchedulerJob --hostname "$${HOSTNAME}"']
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully
```

- airflow-worker
- airflow-triggerer
- airflow-init

### 파이썬 모듈을 추가로 설치하는 경우

- 매번 container에 접속해 설치하는 방법은 유지보수가 어렵고, 보존되지 않아 유지보수 측면에서 **BAD**
- docker-compose.yml의 값을 변경하는 방식을 사용

```diff
- _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:-}
+ _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:- yfinance pandas numpy}
```

# ❗ 느낀 점

docker compose와 그 사용방법, .yml 설정 파일 작성법 등을 배우면서 여러 컨테이너를 가진 프로그램을 도커로 받고 실행하는 과정에 대해 이해할 수 있었다. 강의 내용을 정리하면서, 도커, 컨테이너, 이미지, 도커 컴포즈 등 개념에 대해 한번 정리해야겠다는 생각이 들었다. 주말이나 다음주 방학 동안 공부하고 정리해서 블로그에 올려야겠다.

방학동안 이전 수업을 복습해야겠다고 느꼈다. 그리고 슬슬 CS 공부도 시간 정해서 시작해야겠다. 뭔지 알지만, 들으면 기억도 나지만 정확하게 설명을 못하는 개념이 점점 늘어나고 있어서 답답하고 불안하다.
