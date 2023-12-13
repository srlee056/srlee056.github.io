+++
author = "Seorim"
title =  "Day 43 Airflow(2)"
slug = "day-43"
date = 2023-12-13T17:15:12+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Airflow"
]
+++

# 📋 공부 내용

## Airflow 실습

## Airflow 과제

[과제 github](https://github.com/srlee056/devcourse-week10-day3-hw)

###

# 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

## SSH 연결

### in vscode

### ssh key 생성법

### root user 로그인 방법

## SQL

### single quote in string

-   insert into table_name values ('Seorim's name')
    -> 에러 발생

-   replace ' to ''

    ```python
    text = "Seorim's name"
    text = text.replace("'", "''")
    # Seorim's -> Seorim''s
    ```

-   insert into table_name values ('Seorim''s name')
    -> 잘 실행되는걸 볼 수 있음 :>

# ❗ 느낀 점
