+++
author = "Seorim"
title =  "Day 45 Airflow(4)"
slug = "day-45"
date = 2023-12-15T12:53:19+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Airflow", "MySQL", "Redshift" 
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

## OLTP 테이블 복사하기

-   Production MySQL Tables (OLTP) -> AWS Redshift (OLAP)

-   MySQL Tables(Source) --> Airflow Server
    -   --> S3(Cloud Storage) --`COPY Command`--> Data Warehouse
    -   --`INSERT Command`--> Data Warehouse

### 권한 설정

1. Airflow DAG에서 S3 접근 (Write 권한)
    - IAM User: S3버킷에 Read/Write 권한 설정
    - access key, secret key 사용
2. Redshift에서 S3 접근 (Read 권한)
    - Redshift에서 S3 접근할 수 있는 Role 생성 후 Redshift에 지정

### Full Refresh

### Incremental Update

## Backfill 실행하기

## Summary 테이블 만들기 (ELT)

###

# 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

# ❗ 느낀 점
