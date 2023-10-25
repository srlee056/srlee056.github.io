+++
author = "Seorim"
title =  "Day 8"
date = 2023-10-25T16:23:38+09:00

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

# <span style="color:#79AC78">TIL - HTML 분석</span> 

## 📋 <span style="color:#B0D9B1">공부 내용</span>

`requests` 
- 라이브러리를 사용해 웹 브라우저와 같이 웹 페이지를 요청하고 응답을 받아옴    
- 응답 받은 문서 -> **분석 필요!**

### <span style="color:#D0E7D2">BeautifulSoup</span>

> HTML 코드를 분석 하는 라이브러리 (HTML Parser)

#### <span style="color:#F9B572">설치 방법</span>
_(mac, python3 기준)_
  ```
  pip install bs4
  or
  pip3 install bs4
  ```

#### <span style="color:#F9B572">HTML 분석 실습</span>

- 필요 라이브러리 불러오기
```python
import requests
from bs4 import BeautifulSoup #import bs library
```


- 사이트를 요청하고 응답받기
```python
# requests.get으로 사이트 HTML을 받아 와 저장
res = requests.get("https://www.example.com")
```

- 응답받은 HTML으로 BeautifulSoup 객체를 만들고 내용을 출력해보기
```python
# bs 객체를 만들고, res.text와 "html.paser"를 인자로 넘겨
# HTML parser 역할을 하게 만듬
soup = BeautifulSoup(res.text, "html.parser")

# HTML의 구조를 잘 보여주도록 들여쓰기하여 출력
print(soup.prettify())
```
- HTML의 특정 요소 찾기
```python
# 해당하는 각 태그의 내용을 보여줌
soup.title
soup.head
soup.body
    
# h1태그들 중 가장 먼저 나오는것을 찾아 반환
h1 = soup.find("h1")
# p태그 모두를 찾아 반환
soup.find_all("p")
    
# 태그 이름과 (h1) 태그 안의 내용을 가져옴
h1.name
h1.text
```
- 특정 요소를 찾아 그 안의 원하는 정보만 추려내기
```python
# 책 리스트를 찾아 그 제목만 추출하는 코드

# 직접 사이트를 확인하고 h3태그에 책 이미지, 저자, 제목 등이 있는것을 확인하여 find_all로 가져옴
h3_results = soup.find_all('h3')

# 객체로 만들게 되면, 내부 태그를 속성처럼 쓸 수 있음
# h3 > a 태그 내에 title 속성 value를 출력하는 함수
for book in h3_results:
	print(book.a['title'])
```
- `id`를 이용해 요소 가져오기
```python
# .find(<tag_name>, id = <id_name>)
soup.find_all("div", id="bo_list")
soup.find("div", id="bo_cate")
```
- `class`를 이용해 요소 가져오기
```python
# .find(<tag_name>, <class_name>)
soup.find_all("li", "questions") 
soup.find("div", "question")
```

- `user-agent` 정보를 넘기면서 요청하기   
    [user agent 확인 사이트](https://www.whatismybrowser.com/detect/what-is-my-user-agent/)
```python
user_agent = {"User-Agent": <본인의 user agent 정보>}
import requests
from bs4 import BeautifulSoup
url = "https://qna.programmers.co.kr/"
res = requests.get(url, user_agent)
```

- 페이지네이션 (Pagination)   
    > 정보를 인덱스로 구분하는 기법
    
    ```python
    # 해당 사이트는 query string으로 구분
    import time
    import requests
    from bs4 import BeautifulSoup

    url = "https://qna.programmers.co.kr/?page={}"

    for i in range(1, 6):
        res = requests.get(url.format(i), user_agent)
        soup = BeautifulSoup(res.text, "html.parser")
        questions = soup.find_all("li", "question-list-item")
        for question in questions:
            print(question.find("div", "question").find("div", "top").h4.a.text)
        # 과도한 요청을 방지하기 위해 1초마다 요청
        time.sleep(1)
    ```
### <span style="color:#D0E7D2">웹 사이트와 스크래핑</span>


#### <span style="color:#F9B572">정적 or 동적 웹 사이트</span>
- 웹 사이트는 _정적(Static) 웹 사이트_ 와 _동적(Dynamic) 웹 사이트_ 로 나눌 수 있다.

  |웹 사이트 | Static | Dynamic|
  |---|---|---|
  | HTML 내용| 고정 | 변경|
  | HTML 데이터 로드 | 응답 이전에 완료됨 | 응답 이후에 완료되기도 함|


#### <span style="color:#F9B572">동기 or 비동기 처리</span>

동기 처리 (정적 웹 사이트)
- 렌더링이 완료된 이후에 데이터 처리를 진행
- 요청에 따른 응답을 기다려야 함

비동기 처리 (동적 웹 사이트)
- 렌더링과 데이터 처리가 동시에 진행됨
- 요청에 따른 응답을 기다리지 않음
- 상황에 따라 응답 시 받은 HTML 문서의 데이터가 `완전하지 않은 경우 발생`

#### <span style="color:#F9B572">스크래핑</span>

- requests 요청의 문제점
  1. 불완전하고 원하지 않은 내용의 응답을 받게 되어 **동적 웹사이트에 적용이 어려움**
  2. 키보드, 마우스 입력 등 **UI와 상호작용 하기 어려움**

- 해결 방법
  1. 데이터 처리 후 응답을 받아오는 방식 적용
  2. UI Action을 프로그래밍
  3. 웹 브라우저 역할을 하는 대신 **웹 브라우저를 조작** -> `Selenium`
## 👀 <span style="color:#B0D9B1">CHECK</span>

*<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>*

- Pagination : 정보를 인덱스로 구분하는 기법
    - Query String
- .format method   
    ```python
    "{}".format(a) -> "a"
    ```
- 동기 처리와 비동기 처리의 개념에 대해 한번 더 복습
- bs .find 함수
    - 여러 요소를 찾을 때 or을 적용하여 A or B or C class 모두를 찾을 수 있는지 궁금
    - class A, B / class A 인 요소 두 개가 있을 때 class B를 가진 요소는 배제하고 찾는 방법도 궁금
- 정적 웹 사이트에는 requests같은 라이브러리를 쓰고 동적 웹 사이트에는 Selenium을 쓰는지 아니면 일괄적으로 Selenium처럼 웹 사이트를 조작하는 형식의 라이브러리만 쓰는지 궁금하다.
- 중첩된 div 내에 h4가 있는 경우에 (div>div>h4) 가장 바깥 div 내에 h4가 하나뿐이라면 `div_container.find("div").h4` 로 쓰지 않고 `div_container.h4`로 작성해도 원하는 기능을 하던데 이유가 궁금하다.
## ❗ <span style="color:#B0D9B1">느낀 점</span>

오늘은 꽤나 만족스러운 하루를 보냈다.

강의와 실습은 내 기준엔 어렵지 않아 2시간 내로 완료했다. 실습을 하면서 주어진 사이트 외에 다른 사이트도 들어가서 분석하고 배운 함수를 적용해보았다.
실습을 하면서 궁금한 점이 몇개 생겼고 CHECK에도 적어놨는데, TIL을 다 적은 후에 구글링 및 슬랙 채널을 통해서 해결해 볼 생각이다.

그리고 코어타임 중간에 hugo로 만든 블로그에 글을 올려봤는데 내가 생각했던것 보다 더 쉬워서 안심했다. 블로그와 관련해 다음 세 가지 목표를 세웠다. 

1. github action 수정해서 push 하면 자동으로 빌드, 배포되게 만들기
2. hugo new post 템플릿 만들기
3. 한국어 설정하기

이렇게 세팅을 완료한 후에, 기존에 작성했던 TIL을 올리고 카테고리 설정까지 하면 완성이다. 나중엔 블로그 메인 페이지 커스텀, 댓글 위젯 설정, SEO 세팅 등을 해보려고 한다.

