+++
author = "Seorim"
title =  "Day 35 Superset Dashboard"
slug = "day-35"
date = 2023-12-01T11:54:04+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Superset", "Dashboard", "preset.io", "Docker"
]
+++

# 📋 공부 내용

## 시각화 툴

> **KPI, 지표**, 중요한 데이터 포인트들을 **데이터 기반**으로 계산/분석 표시해주는 툴  
> Dashboard or BI(Business Intelligence) Tool

> 결정권자 : 데이터 기반 의사결정을 할 수 있음  
> 현업 종사자 : 데이터 분석을 쉽게 할 수 있음

-   KPI : Key Performance Indicator

-   데이터 기반 의사결정의 종류
    -   데이터 기반 결정 (Data-Driven Decision)
    -   데이터 참고 결정 (Data-Informed Decision)
-   Citizen Data Analyst / Scientist

-   EDA : Exploratory Data Analysis
    -   데이터 특성 분석

### 어떤 시각화 툴이 있을까?

|    기업    | Tools                                              | 특징                                                     |
| :--------: | -------------------------------------------------- | -------------------------------------------------------- |
|            | Excel<br>Google Spreadsheet                        | 가장 많이 쓰이는 시각화 툴                               |
|            | Python                                             | EDA에 더 적합                                            |
|   Google   | [Looker](#looker)                                  |                                                          |
| Salesforce | [Tableau](#tableau)                                |                                                          |
|     MS     | Power BI                                           |                                                          |
|            | [Apache Superset](#supserset)<br>[ReDash](#redash) | 오픈소스                                                 |
|            | [Mode Analytics](#mode-anlytics)                   |                                                          |
|            | Google Studio<br>AWS Quicksight                    | 자사 클라우드 기반의 Dashboard<br/> 기능이 비교적 떨어짐 |

-   Excel, Python 등은 코딩이 가능해야 활용 가능 -> 기능상의 제약 존재

#### Looker

-   2019.06 구글에 인수됨
-   특징
    -   LookML이 자체 언어로 데이터 모델을 만들어줌
    -   내부고객 뿐 아니라 외부 고객을 위한 대시보드 작성 가능
    -   다른 사람이 작성한 Dashboard를 참고하여 내가 활용할 수 있음(Template 처럼)
    -   setup은 힘든 편, backend에 부하가 좀 있는 편

#### Tableau

-   2019.06 Salesforce에 인수됨
-   특징
    -   다양한 제품군, 일부는 무료로 사용 가능
    -   배우기 어렵지만 강력한 대시보드 -> 일부 전문가만 대시보드 작성 가능
    -   Looker 뜨기 전까지 오랫동안 마켓 리더로 군림해옴

#### ReDash

-   2020 Databricks에 인수됨
-   특징

    -   오픈소스로 시작, Superset과 흡사
    -   SQL 에디터 존재 -> Dashboad와 연결된 곳에 Query를 보낼 수 있음

-   Superset이 Redash와 다른 점
    -   `role 기반 사용자 역할, 권한 지정 가능`
    -   dashboard에 역할 지정 가능

#### Mode Anlytics

-   특징
    -   조금 더 기술적인 인력을 대상으로 하는 대시보드
    -   SQL, R, Python 등으로 분석 가능
    -   KPI 대시보드 보다는 EDA 툴에 더 가까움

### 시각화 툴 선택?

-   Looker vs. Tableau

    -   둘 다 초반 learning curve 존재
    -   Tableau: 가격적인 부분에 이점

-   Self Service Dashboard의 중요성
    -   매번 사람의 노동을 필요로 하지 않음
    -   사용하기 쉬워야 더 많은 인력들이 직접 대시보드를 만들 수 있음
        -   `Data Democratization`, `Data Decentralization`
        -   데이터 품질의 중요성, [데이터 거버넌스](#데이터-거버넌스)가 필요한 이유

## Supserset

-   Airbnb에서 시작된 오픈소스, Maxim(Airflow 개발자)과 같이 시작

### 특징

-   다양한 형태의 시각화
-   쉬운 인터페이스
-   대시보드 공유 지원
-   엔더프라이즈 수준의 보안, 권한 제어 기능 제공
-   SQLALchemy 연동 -> 다양한 db(SQLAlchemy와 연동되는) 지원
-   Druid.io(streaming db)와 연동한 실시간 데이터의 시각화 가능
-   API, 플러그인 아키텍쳐 제공 -> 기능 확장이 쉬움

### 구조

-   Python으로 제작됨
-   Web Interface -> Flask & React JS
-   metadata db : sqlite
    -   병렬성이 떨어지는 단점 -> postgresql or mysql 사용
-   Redis를 caching layer로 사용하여 성능 최적화

### 용어

-   Database : backend db (Redshift, Druid, ...)
-   Dataset : table
-   Dashboard - Chart : Dashboard는 하나 이상의 chart로 구성됨

## 실습 Dashboard

### 구성

-   DB : Redshift
-   2 charts in 1 Dashboard
    -   채널별 MAU(Monthly Active User) chart
        -   Dataset : analytics.user_session_summary
    -   Monthly Cohort chart
        -   Dataset : analytics.cohort_summary

### MAU chart

-   session단으로 완전한 정보를 갖게 만든 테이블

```sql
CREATE TABLE analytics.user_session_summary AS
SELECT usc.*, t.ts
FROM raw_data.user_session_channel usc
LEFT JOIN raw_data.session_timestamp t ON t.sessionid = usc.sessionid
```

### Monthly Cohort chart

-   Cohort?

    -   특정 속성(보통은 사용자의 서비스 등록`월`)을 바탕으로 나뉜 사용자 그룹

-   Cohort 분석?

    -   Cohort 기반으로 `사용자의 이탈률, 잔존률, 총 소비금액` 등을 계산
    -   사용자 `잔존률(Retention)` : 보통 `월` 기반으로 시각화해서 봄

-   analytics.cohort_summary

```sql
CREATE TABLE analytics.cohort_summary as SELECT cohort_month, visited_month, cohort.userid FROM (
SELECT userid, date_trunc('month', MIN(ts)) cohort_month
FROM raw_data.user_session_channel usc
JOIN raw_data.session_timestamp t ON t.sessionid = usc.sessionid GROUP BY 1
) cohort JOIN (
SELECT DISTINCT userid, date_trunc('month', ts) visited_month FROM raw_data.user_session_channel usc
JOIN raw_data.session_timestamp t ON t.sessionid = usc.sessionid
) visit ON cohort.cohort_month <= visit.visited_month and cohort.userid = visit.userid;
```

### Google Spreadsheet를 활용한 시각화 실습

-   +) Python `gspread` module을 활용하면 Python으로 스프레드시트 조작 가능

-   MAU chart

    ![](image-1.png)

-   Monthly Cohort chart

    ![](image.png)

## Superset 사용 방법

### preset.io

1. [회원가입](#presetio-가입)
2. workspace 생성
3. database 연결

### docker

1. [docker 설치](https://docs.docker.com/desktop/) 및 활용 ram 크기 설정 (mac: 6GB 이상)
2. Superset Github repo를 클론

    ```bash
    git clone https://github.com/apache/superset.git
    ```

3. superset 폴더로 들어가서 다음 두 command 실행

    ```bash
    cd superset
    # 특정 버전을 다운로드 하려면 아래 command 실행
    # git checkout 1.4.0
    docker-compose -f docker-compose-non-dev.yml pull
    docker-compose -f docker-compose-non-dev.yml up
    ```

4. <http://localhost:8088>으로 웹 UI 로그인 (id : admin, pw : admin)

### Redshift db 연결

-   SQLAlchemy URI 를 통해 연결 가능

    ```
    postgresql://admin:xxxx@default-workgroup.705556746971.ap-northeast-2.redshift-serverless.amazonaws.com:5439/dev
    ```

    ![](image-18.png)

-   연결된 db 정보
    ![](image-17.png)

### SQL Lab

-   Redshift로 SQL 쿼리 보낼 수 있음
-   [MAU](#mau-chart), [Monthly Cohort](#monthly-cohort-chart) chart를 위한 table을 생성

    ![](image-9.png)

-   설정

    -   사용하는 database에 DML 권한을 부여해야 함

    ![](image-4.png)

    ![](image-5.png)

### MAU

-   dataset & chart

![](image-2.png)

![](image-3.png)

### Cohort

-   dataset & chart

![](image-15.png)

### (추가) snowflake db 연결

-   입력 정보

![](image-7.png)

-   ACCOUNT 확인 방법 (in Snowflake)

![](image-8.png)

## HW - nps chart

-   dataset

    이전 단계에서 연결시킨 snowflake db를 사용

    ![](image-12.png)

-   chart

    Edit dataset > Metrics > `overal_nps` item 추가

    ![](image-11.png)

    ![](image-10.png)

    chart 설정 및 결과 화면

    ![](image-16.png)

### result dashboard

![](image-6.png)

# 👀 CHECK

## 용어

### 데이터 거버넌스

-   <https://cloud.google.com/learn/what-is-data-governance?hl=ko>
    _<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

## 기타

### preset.io 가입

-   google account로 가입 시 14일동안 Professional plan 무료 체험 가능

![](image-13.png)

-   14일 이후에는 starter plan(free)로 변경되는 듯 함

![](image-14.png)

# ❗ 느낀 점
