+++
author = "Seorim"
title =  "Day 21"
date = 2023-11-13T14:03:32+09:00
lastmod = 2023-11-19
categories = [
    "DevCourse",
]
tags = [
    "TIL", "SQL","RDB", "Redshift", "Data Warehouse"
]
+++

# TIL - SQL and RDB

## 📋 공부 내용

### SQL

> SQL : Structured Query Language

-   DDL(Data Definition Language) : 테이블 schema 정의
-   DML(Data Manipulation Language) : 테이블 데이터(레코드) 조작과 질의

**1. SQL 특징**

-   `구조화된 데이터`를 분석하는 **쉽고 검증된 언어**
-   빅 데이터 등장 -> SQL 하락? -> `빅 데이터 또한 구조화` 된 부분이 있기 때문에 SQL이 필요 -> SQL 재기
-   모든 데이터 웨어하우스 (Redshift, Snowflake, BigQuery 등) SQL 기반
-   데이터 직군 : 데이터 엔지니어, 데이터 분석가, 데이터 과학자 모두 SQL이 중요

**2. 단점**

-   비구조화된 데이터를 다루는 데 제한적
    -   정규표현식을 통해 어느정도만 가능
    -   Spark, Hadoop 같은 분산 컴퓨팅 환경 필요
-   많은 RDB는 플랫한 구조만 지원(JSON처럼 nested 구조 X)

### RDB

> Relational Database  
> 구조화된 데이터를 저장하고 질의하는 데 사용되는 저장 장치

**1. Table Schema?**

-   table format
-   테이블을 구성하는 컬럼의 이름과 타입

**2. RDB 특징**

-   구조화된 데이터 : RDB에 저장하기 편함
-   But, 구조화되지 않은 형태의 데이터도 많음 (text, image, video, streaming, ...)

**3. data modeling**

-   RDB에서 데이터를 어떻게 표현할것인가? 에 대한 고민

#### 프로덕션 데이터베이스 Production Database

> OLTP (Online Transaction Processing)  
> 빠른 응답 속도, 서비스에 필요한 데이터 저장  
> 웹, 앱 서비스 등에 사용되며 FE, BE 개발자들이 주로 사용

**Star schema**

-   데이터를 논리적 단위로 나누어 저장하며, 필요할 경우 JOIN
-   스토리지의 낭비가 덜하며, 계산 속도가 상대적으로 느리지만 데이터 업데이트가 편함

#### 데이터 웨어하우스 Data Warehouse

> OLAP (Online Analytical Processing)  
> 큰 데이터 처리, 데이터 분석 또는 모델 빌딩을 위한 데이터 저장  
> 데이터 직군 및 데이터팀이 주로 사용

**Denomalized schema**

-   단위 테이블로 나누어 저장하지 않으며, 별도의 JOIN이 필요하지 않음
-   스토리지를 더 사용하지만, 빠른 계산이 가능

#### RDB 2단계 구조

1. db folder(schema)
2. tables

```
raw_data(DB)
|-patient(table)
|-patientData
|-patientProfile
|
analytics(DB)
|-patientSummary(table)
|-vitalSummary
|-alertSummary
```

#### table 구조

-   records(row)
-   하나 이상의 field(column)으로 구성
-   field는 name, type, primary key 여부로 구성됨

| column name | type        | pk? |
| ----------- | ----------- | --- |
| userId      | int         |     |
| sessionId   | varchar(32) | Yes |
| channel     | varchar(32) |     |

### Data Warehouse

> 데이터 분석 등 큰 데이터의 처리를 위해 별도로 존재하는 DB  
> 프로덕션 DB의 속도 저하를 막고 서비스에 영향을 주지 않기 위해 구성

-   고객이 아닌 내부 직원을 위한 데이터베이스
-   프로덕션 DB를 주기적으로 복사하여 데이터 웨어하우스에 저장
-   프로덕션 DB와 다르게 `primary key uniqueness를 보장하지 않음`

**1. 서비스 별 비용 옵션 비교**
| 회사 | Data Warehouse | 비용 청구 옵션 |
| ----------- | -------------- | -------------- |
| AWS | Redshift | 고정비용 |
| GoogleCloud | Big Query | 가변비용 |
| | Snowflake | |

-   실제 사용시 가변비용의 웨어하우스 사용 추천

**2. ETL(데이터 파이프라인)**

-   외부 데이터를 가져와 Data Warehouse로 저장하는 코드

#### 데이터 인프라

-   데이터 엔지니어가 관리
-   ETL, Data Warehouse
-   (비 구조적 데이터가 추가될 경우 )Spark 등 대용량 분산처리 시스템

### Cloud & AWS

#### Cloud

> 컴퓨팅 자원을 네트워크를 통해 서비스 형태로 사용하는 것

**1. 컴퓨팅 자원**

-   초기에는 서버 등 하드웨어에 국한되어 서비스됨
-   최근에는 mysql 등 소프트웨어 또한 서비스되고 있음

**2. 탄력적 자원**

> "No Provisioning", "Pay As You Go"

-   자원을 필요한만큼 실시간으로 할당하여 사용한만큼 지불
-   필요한만큼의 자원을 유지하는것이 중요

**3. 클라우드 컴퓨팅이 없다면?**

-   서버,네트워크, 저장소 등 구매와 설정 ㅅ직접 수행
-   확장이 필요하면 공간부터 확보해야함
-   서버 구매 및 설치에 2~3달 소모
-   Peak time 기준으로 Capacity planning -> 자원이 놀게 되는 현상 발생
-   직접 운영 비용과 클라우드 컴퓨팅 비용 -> 기회비용 (직접 설치에 소요되는 시간을 비용으로 환산한다면?)

**4. 클라우드 컴퓨팅 장점**

-   초기 투자 비용이 크게 줄어듬 -> 운영 비용이 증가
-   자원 준비 및 설정을 위한 대기시간 대폭 감소
-   노는 리소스 제거로 비용 감소

#### AWS

> 가장 큰 클라우드 컴퓨팅 서비스 업체
> 현재 Amazon에서 제일 큰 이익을 내고 있음
> Netflix, Zynga 등 상장 업체들도 사용중

**AWS 서비스**

-   EC2: 서버 호스팅 서비스
-   S3: 대용량 클라우스 스토리지 서비스
-   Redshift : Data Warehouse 서비스
-   기타 DB 서비스, AI&ML 서비스 등

### Redshift

> Scalable SQL 엔진

-   OLAP
-   Columnar storage : 컬럼별 압축 가능, 컬럼 추가 및 삭제가 빠름 (레코드 기준 X)
-   벌크 업데이트 지원: insert 커맨드로는 추가하기 어려운 많은 records를 갖고 있는 경우에 `copy` 커맨드를 통해 쉽고 빠르게 일괄 복사 가능
-   Postgresql 8.x와 SQL (대부분)호환됨

#### Redshift Schema

```
            DEV
             |
    -----------------------
    |        |            |
<raw_data> <analytics> <adhoc>
```

SQL command

```sql
CREATE SCHEMA raw_data
CREATE SCHEMA analytics
CREATE SCHEMA adhoc
```

## 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

## ❗ 느낀 점
