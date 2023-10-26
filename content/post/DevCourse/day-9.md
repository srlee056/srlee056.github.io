+++
author = "Seorim"
title =  "Day 9"
date = 2023-10-26T14:37:54+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Selenium", "WebDriver", "Chrome", "XPath", "Events", 
]
+++

# TIL - Selenium

## 📋 공부 내용
### Selenium?
#### Selenium
> 웹 브라우저를 조작할 수 있는 자동화 프레임워크 
#### WebDriver
> 웹 브라우저와 연동하고 제어하는 자동화 프레임워크 
#### 설치 방법
```
pip3 install selenium
pip3 install webdriver-manager
``` 

### Selenium 활용
#### import
- Selenium, webdriver 불러오기
```python
# web browswer와 직접 연결
from selenium import webdriver
# 크롬 객체를 넣을때 인자로 넣어주게 됨
from selenium.webdriver.chrome.service import Service
# 사용중인 크롬과 동일한 버전으로 싱크하기 위함
# ex) firefox - firefox driver manager 
from webdriver_manager.chrome import ChromeDriverManager
```

#### driver 생성 및 request&response
- Chrome 객체를 생성하여 크롬창을 실행

```python
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
```
- `.get(url)` 으로 request 보냄

```python
url = "https://www.example.com"
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.get(url)
```

- `.page_source`로 Response HTML 문서 확인
```python
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.get(url)
print(driver.page_source)
```

- with-as 구문으로 driver 자동 종료
    - 작성한 코드를 실행한 후 driver 종료되며 크롬 창도 꺼짐
```python
with webdriver.Chrome(service=Service(ChromeDriverManager().install())) as driver :
    driver.get(url)
    print(driver.page_source)
```

#### HTML 요소 추출
- HTML 특정 요소 추출
    - .find_element(by, target)
    - .find_elements(by, target)
    - by : By.ID, BY.TAG_NAME, BY.CLASS_NAME, BY.XPATH ...
```python
from selenium.webdriver.commom.by import By
```
- example 1

```python
with webdriver.Chrome(service=Service(ChromeDriverManager().install())) as driver :
    driver.get(url)
    p1 = driver.find_element(By.TAG_NAME, "p")
    p_list = driver.find_elements(By.TAG_NAME, "p")
    print(p1.text)
    for p in p_list:
        print(p.text)

```

- example 2
    - 찾으려는 요소를 XPath를 통해 찾으려는 경우
    ```python
    path = '//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[1]/div/a/div[2]/p[1]'
    result = driver.find_element(By.XPATH, path)
    print(result.text)
    ```

    - 유사한 XPath를 가진 여러 요소들을 가져오는 경우 
    ```python
    # 반복되는 부분을 제외하고 변하는 부분을 {}로 적은 후 .format() 활용
    path = '//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[{}]/div/a/div[2]/p[1]'
    for i in range(1, 11):
        result = driver.find_element(By.XPATH, path.format(i))
        print(result.text)
    ```    

#### Wait and Call
동적 웹페이지를 스크래핑하기 위해 일정 시간을 기다림(Wait)
- Implicit Wait
    - .implicitly_wait({num}) : num초동안 로딩을 기다리는데 그 전에 완전한 응답이 오면 다음으로 진행
    ```python
    from selenium.webdriver.support.ui import WebDriverWait

    driver.get(url)
    driver.implicitly_wait(10) #10초
    result = driver.find_element(By.XPATH, path)
    ```

- Explicit Wait
    - WebDriverWait()
        - until() : 조건이 만족될 때까지
        - until_not() : 조건이 만족되지 않을때까지
        - `expected_conditions`(EC) : selenium에 정의된 조건들 

    ```python
    from selenium.webdriver.support import expected_conditions as EC

    with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
        driver.get(url)
        # XPath 가 path 인 요소가 존재할때까지 최대 10초동안 기다림
        element = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, path)))
        print(element.text)
    ```
    
#### Events
- 마우스 이벤트
    - 마우스 움직이기, 마우스 누르기, 마우스 떼기 등 여러 이벤트가 일어날 수 있음
    - 특정 버튼을 찾은 후 클릭하는 코드

    ```python
    # 다른 import 과정은 생략 
    from selenium.webdriver import ActionChains

    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
    driver.get(url)
    driver.implicitly_wait(0.5)
    # class 두 개 이상인 경우 .으로 연결해 동시에 참조 가능 
    # class 이름으로 버튼을 찾음
    button = driver.find_element(By.CLASS_NAME, 'UtilMenustyle__Link-sc-2sjysx-4.ewJwEL')
    # 찾은 버튼을 마우스로 `click`하는 이벤트를 실행
    ActionChains(driver).click(button).perform()
    ```

- 키보드 이벤트
    - 키보드 누르기, 키보드 떼기 등 여러 이벤트가 일어날 수 있음

    - 특정 입력창을 찾은 후 요소에 입력하는 코드
    ```python
    # 다른 import 과정은 생략
    from selenium.webdriver import ActionChains, Keys

    # XPath를 통해 id 입력하는 input 태그를 찾음
    id_input = driver.find_element(By.XPATH, id_path)
    # {your_id}를 input에 입력하는 이벤트를 실행
    ActionChains(driver).send_keys_to_element(id_input, {your_id}).perform()    
    ```


- 로그인 자동화 예시
    - 로그인 페이지로 이동 후, 아이디와 비밀번호를 입력하고 로그인버튼을 누르는 코드
    ```python
    import time

    # url, id_path, pw_path 등은 사이트마다 달라짐
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
    driver.get(url)
    time.sleep(1)

    driver.implicitly_wait(0.5)
    button = driver.find_element(By.CLASS_NAME, {class_name_1})
    ActionChains(driver).click(button).perform()
    time.sleep(1)

    id_input = driver.find_element(By.XPATH, id_path)
    ActionChains(driver).send_keys_to_element(id_input, {your_id}).perform()
    time.sleep(1)

    pw_input = driver.find_element(By.XPATH, pw_path)
    ActionChains(driver).send_keys_to_element(pw_input, {your_password}).perform()
    time.sleep(1)

    login_button = driver.find_element(By.CLASS_NAME, {class_name_2})
    ActionChains(driver).click(login_button).perform()
    ```
## 👀 CHECK

*<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>*
- selenium 
    - mouse events, keyboard events 찾아보기
    - EC(expected condition)의 다른 조건에는 어떤게 있는지 찾아 정리해보기

- 여러 class를 가진 요소 찾기
    - `class_name_1.class_name_2`처럼 `.`으로 연결하여 동시에 참조할 수 있음

## ❗ 느낀 점
오늘도 2시간 반 정도에 강의와 실습을 모두 끝냈다. 계속 일찍 끝나고 쉬운 내용만 나오니까 너무 겉핥기로만 배우는 것 같고, 혼자서 더 공부해야할까 불안감도 조금 들었다.
그래서 오늘은 TIL을 적은 다음 배운것들을 활용해 KBO사이트에서 스탯을 추출해보려고 한다.
다 진행하고 나서 따로 정리하고 글을 써서 올릴 예정이다.

그리고 어제 블로그를 세팅한 이후에 기존에 TIL을 올리던 `velog`를 어떻게 해야할지도 고민하고 있다. 다 지우기도 아깝고 더 안올리자니 멈춰져있는 블로그 같아서..
백업용으로 주말에 몰아서 일주일치를 올리는 방법도 고민중이다!

아직 전반적으로 쉬운편이기는 하지만, 점점 더 흥미로워지고 있다. 이후에 배울 내용들과 다음달 초에 하게 될 프로젝트가 기대된다. 🤗
