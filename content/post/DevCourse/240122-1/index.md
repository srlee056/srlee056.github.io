+++
author = "Seorim"
title =  "데이터 처리(1): 실시간 데이터 처리"
slug = "240122-1"
date = 2024-01-22T16:29:58+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Kafka", "Spark Streaming"
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

# 구글의 기술 공유가 데이터 처리 분야에 미친 영향

## Page Rank

- 대용량 컴퓨팅 인프라와 소프트웨어가 필요해진 시점

# 데이터 처리 단계

## 데이터 수집 (Data Collection)

## 데이터 저장 (Data Storage)

> Data Warehouse -> Data Lake -> Cloud Data Platform / Messaging Queue -> Data Mesh

# 데이터 처리 (Data Processing)

> Batch Processing -> Realtime(& Semi Realtime) Processing

## 용어 및 개념 정리

- Throughput : 단위 시간 동안 처리할 수 있는 데이터 양
  - 배치 시스템에서 중요
- Latency : 데이터를 처리하는 데 걸리는 시간
  - 실시간 시스템에서 더 중요
- Bandwidth : Throughput x Latency

- SLA(Service Level Agreement) : 서비스 제공업체와 고객간의 계약 또는 합의사항으로, 사내 시스템 사이에서도 정의됨
  - `uptime` 99.9% 보장 : 1년의 0.1%인 8시간 45.6분을 제외하고는 항상 작동함을 보장
  - 99%(or average) of API's `latency` = 0.5s 보장
  - `freshness` of data

## Lambda Architecture

## 배치 처리(Batch Processing)

> 주기적으로 데이터를 이동시키거나 처리하는 것

- Throughput 중요
- 주기는 보통 `hourly, daily`이며 짧은 경우엔 5~10분 정도
- 주기가 이보다 짧을 경우 배치 처리를 위한 프레임워크에서 처리하기엔 적절하지 않음

### 시스템 구조

- Distributed File System : HDFS, S3
- Distributed Processing System : MapReduce, Spark (DataFrame, SQL), ...
- Data Process Scheduling : Airflow

## 실시간 처리(Realtime Processing, Streaming Processing)

> 연속적인 데이터를 처리하는 것

- Latency 중요
- realtime과 semi-realtime(micro batch)로 나뉨

# 실시간 데이터 종류와 사용 사례

# 실시간 데이터 처리 과정의 challenge
