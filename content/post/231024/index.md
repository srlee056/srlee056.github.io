+++
title = '231024'
date = "2023-10-24"

tags = [
    "TIL",
]
+++

# <span style="color:#79AC78">TIL - HTTP</span> 

## 📋 <span style="color:#B0D9B1">공부 내용</span>


### <span style="color:#D0E7D2">웹</span>
#### <span style="color:#F9B572">웹 페이지와 HTML</span>

웹 페이지   
- 웹 속의 **문서 하나**
- ex) 네이버 메인 페이지 
- HTML으로 구성되어있음

웹 사이트
- 여러 웹 페이지의 모음 
- ex) *네이버*라는 웹 사이트

웹 브라우저
- HTTP요청을 보낸 후, HTTP응답에 담긴 HTML문서를 사용자가 보기 쉽게 화면으로 그려주는 (렌더링) 역할

[HTML(개념 정리 글 링크)](https://velog.io/@srlee056/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%97%94%EC%A7%80%EB%8B%88%EC%96%B4%EB%A7%81-%EB%8D%B0%EB%B8%8C%EC%BD%94%EC%8A%A4-6%EC%9D%BC%EC%B0%A8) 
- 이전 강의에서 다룬 내용 참고
- 웹 브라우저마다 지원하는 태그와 속성이 달라짐


#### <span style="color:#F9B572">웹 스크래핑 / 웹 크롤링</span>
웹 스크래핑
- 특정 목적에 따라 웹 페이지에서 원하는 데이터를 "추출"
- ex) 날씨 정보 가져오기, 주식 데이터 가져오기, ...

웹 크롤링
- 크롤러를 이용해 URL을 타고 이동하며 반복적으로 웹 페이지의 데이터를 가져와 "인덱싱"(데이터 색인)
- 구글, 네이버 등 검색 엔진의 웹 크롤러


#### <span style="color:#F9B572">올바른 HTTP Request</span>

올바른 HTTP Request를 위해선..
- 어떤 목적을 달성하려 하는가?
- 서버에 영향을 미치는가?

로봇 배제 프로토콜(REP)
- 웹 크롤링, 스크래핑은 로봇에 의해 실행 가능
- 사이트의 모든 정보를 취득하는것이 정당한가? 의문에서 시작
- robots.txt
    - 각 사이트마다 허용하는 크롤러 정보와 허용범위에 대한 정보를 담고 있음
    - User-agent, Disallow, Allow

### <span style="color:#D0E7D2">HTTP</span>

#### <span style="color:#F9B572">HTTP?</span>
> HyterText Transfer Protocol   
웹 상에서 정보를 주고받기 위한 *약속*



| | HTTP Request | HTTP Response |
| --- | ---| ---|
| 방향 | Client -> Server | Client <- Server|
| 역할 | 정보 요청| 요청에 대한 내용을 담은 응답|
| HEAD| method, path, ...| content-type, date, ... |
|BODY||document|


#### <span style="color:#F9B572">통신하기</span>

`requests`
- Python으로 HTTP 통신을 진행할 수 있게 해주는 라이브러리


`GET`
- naver 메인 페이지를 요청하는 코드

```python
import requests
res = requests.get("https://www.naver.com") # HTTP Response
res.headers # Header 확인
res.text # Body(document) text 형태로 확인
```

`POST`
- <https://webhook.site> 를 통해 POST 통신을 진행할 수 있음

```python
payload = {"name": "Hello", "age": 13}
url = "https://webhook.site/<개인 주소>"
res = requests.post(url, payload)
res.status_code # 상태 코드 확인
```


### <span style="color:#D0E7D2">DOM</span>

#### <span style="color:#F9B572">DOM?</span>
> Document Object Model   
HTML을 파싱하여, 브라우저가 이해하도록 만든 Tree형태의 자료구조

- DOM의 각 노드를 객체로 생각하여, 문서를 편리하게 관리할 수 있음
- 원하는 요소를 동적으로 변경할 수 있음
- 원하는 요소를 쉽게 찾을 수 있음 


- python으로 HTML을 직접 분석하려면 DOM을 생성해주는 브라우저를 거치지 않기 때문에, 
직접 HTML을 분석하는 `HTML Parser`가 필요

## 👀 <span style="color:#B0D9B1">CHECK</span>

*<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>*

- Jupyter lab 
    - Jupyter notebook이나 Colab은 써 봤는데 Jupyter lab은 처음 접해봄
    - Jupyter notebook과 비슷하지만 더 개선된 버전(?)

- DOM에 대한 설명 및 활용 : 복습 후 다른 예시들을 더 찾아볼 것
## ❗ <span style="color:#B0D9B1">느낀 점</span>

HTML 스크래핑을 해본적이 있어서 이론이나 실습 모두 빠르게 진행했다. 5시간 분량의 강의인데 3시간 내로 끝난 것 같다. TIL을 잘 적고 싶어서 고민을 좀 했고 그 외의 시간은 평소보다는 널널하게 흘려보냈다.

TIL을 적을 때 기존에는 강의 받아쓰기처럼 적는 경향이 있었는데, 나중에 다시 읽어보니 이해하고 쓴 것 같은 느낌이 전혀 들지 않았다. 어제 HTML이론에 대한 TIL은 실습 부분 외에도 직접 사용해보고 하면서 적은거라, '내가 직접 써보고 이해한 내용'임을 알 수 있었다. 그런데 초반에 적은 TIL을 다시 보니까 그냥 받아적은게 티가 나기도 하고 잘 기억이 나지 않았다.

오늘은 강의를 처음부터 끝까지 들은 후 키워드만 체크해 필기한 다음, TIL을 적으면서 중간중간 다시 듣는 방식을 사용했다. 이 방식으로 더 잘 읽히고 짜임새 있는 구성의 글을 쓰게 되어서 만족스러웠다.  TIL 쓰는 시간은 오래 걸리지만 익숙해지면 줄어들 것 같아서 걱정은 되지 않는다. 

커리큘럼을 보니 내일부터는 BeautifulSoup를 사용해 스크래핑을 시작하는데, 모르는 부분이 많을 것 같아 벌써부터 기대가 된다. :>

