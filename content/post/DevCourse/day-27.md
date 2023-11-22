+++
author = "Seorim"
title =  "Day 27"
date = 2023-11-21T14:53:46+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "DB", "Network", "AWS", "VPC", 
]
+++

# TIL - AWS Services - DB & Network

## 📋 공부 내용

### DB

#### SQL vs NoSQL

|             | SQL              | NoSQL                                    |
| ----------- | ---------------- | ---------------------------------------- |
| 특징        | 구조화 된 데이터 | 비정형 데이터                            |
| db          | MySql 등         | Mongodb 등                               |
|             | OLAP Cube        | Key-Value, Graph, Document, Column store |
| AWS Service | RDS, ...         | DocumentDB, Dynamo DB                    |

#### Route53

> Amazon Route 53 : DNS 관리 서비스. 도메인 등록, DNS 라우팅, 상태 확인 가능

#### ACM

> AWS Certificate Manager

-   사용할 SSL/TLS 인증서를 발급받거나, 등록할 수 있다.
-   DNS 또는 email 검승으로 해당 인증서의 도메인 소유권을 검증한다.
-   등록된 인증서는 AWS 서비스, 리소스등에 사용할 수 있다.

#### CloudFront

> AWS CDN(콘텐츠 전송 네트워크) Service  
> 데이터 사용량이 많은 application의 웹 페이지 로딩 속도를 빠르게 하는 서비스 (영상, 사진 등 콘텐츠를 빠르게 로딩, 캐싱 등 기술 활용)

**특징**

-   대기 시간 감소
-   보안 향상
-   비용 절감

#### ELB

> Elastic Load Balancing  
> 리소스 전체에 네트워크 트래픽을 균등하게 배포

Load Balancer

-   서버에 가해지는 로드를 분산시키는 장치 또는 기술을 통칭
-   계층에 따라 `L4` LB(Transport layer), `L7` LB(Application layer)가 존재한다.

#### VPC

> Amazon Virtual Private Cloud  
> 사용자가 정의하는 가상네트워크

**특성**

-   Subnet
-   IP 주소 지정
-   Routing Tables
-   Gateway & Endpoint
-   peering 연결

**CIDR**

## 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

-   참고 사이트들
    -   [IP, 그리고 VPC와 서브넷에 대해 알아보자](https://velog.io/@semi-cloud/AWS-VPC-%EC%84%9C%EB%B8%8C%EB%84%B7-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9D%B8%ED%94%84%EB%9D%BC-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
    -   [AWS-Region, AZ, VPC, Subnet 편](https://a1010100z.tistory.com/174)

## ❗ 느낀 점

궁금한 건 있는데 표현이 잘 안되니까 너무 답답하다. 한마디로 `뭘 모르는지 모르는`상태..
내가 궁금한건 웬만하면 어디에 적어두기만 하고 '그렇구나' 하고 넘어가야 속이 편할 것 같다. 왜냐면 나도 뭘 모르는지 모르니까 질문을 할 수 없기 때문

> VPC의 각 subnet은 하나의 AZ안에 존재한다. 근데 subnet은 왜 여러 AZ를 걸쳐서 존재할 수 없지? 리소스를 분배하기 어려운가? 기능적으로 불가능한게 아니라 여러 AZ에 걸쳐서 존재할경우 한 AZ에 문제가 생기면 영향을 주는 서브넷의 수가 많아지기 때문인가? 같은 AZ안에 있어야 속도 등 효율도 더 좋고? 그럼 한 subnet이 하나의 서비스(instance)를 담당하는건 맞나? 그럼 보통 한 application에 VPC는 몇개까지 할당될 수 있지? 한 VPC내에 4~5개의 public subnet과 여러 private subnet이 존재하는 거 같은데 필요한 서비스 개수를 만족할만큼 subnet을 가질 수 있나?

어제만 해도 이런식으로 꼬리에 꼬리를 물고 궁금한게 떠올랐고... 정리하기도 힘들고 결과적으로 뭘 질문하고 찾아봐야하는지도 모르겠어서 포기했다. 적어두고 나중에 내가 더 지식이 생기면 이 질문들을 어떻게 물어봐야 할지도 알게 되겠지.... ㅠㅠ
