+++
author = "Seorim"
title =  "Day 1"
date = 2023-10-16

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Algorithms", "Data Structure", "Blog", "hugo", "Github Pages"
]
+++

# **<span style="color:#79AC78">1. 자료구조 & 알고리즘 강의 및 코드 리뷰</span>**


## 📋 **<span style="color:#B0D9B1">공부 내용</span>**

### <span style="color:#D0E7D2">선형 배열 - Linear Arrays</span>
    
- 배열(array) : 개념적 구조 / 리스트 : python 데이터형
- 리스트 methods
    - time complexity O(1)
        - `.append()`
        - `.pop()`
    - time complexity O(n)
        - `.insert()`
        - `.del()`
        - `.index()`
        - `.insert()`
- 정렬(sort)
    - `sorted()` : function, 정렬된 새로운 리스트를 반환하며 기존 리스트에는 변화 없음.
    - `.sort()` : method, 기존 리스트가 정렬됨
    - 숫자가 아닌 문자열 등 데이터형의 정렬 : 사전순이 기본, 문자열의 길이 등 다른 정렬 조건을 사용하고 싶다면 `lambda`  활용
        - 문자열을 길이순으로 정렬
            
            ```python
            sorted(L, key = lambda x : len(x))
            ```
            
        - 사전 데이터형(dictionary)에 key = ‘name’인 value의 문자열 순서대로 정렬
            
            ```python
            L = [ {'name' : 'John', 'score': 90},
                  {'name' : 'Paul', 'score': 80} ]
            sorted(L, key = lambda x : x['name']
            ```
            
- 탐색(search)
    - 선형(linear) 탐색, 순차(sequential) 탐색
    - 이진(binary) 탐색

### <span style="color:#D0E7D2">재귀 알고리즘 - recursive algorithms</span>
- **종결 조건(trivial case)** 을 명시해야 함
- 예시
    - 1부터 x까지 숫자의 합을 구하는 함수 (sum)
        
        ```python
        def recursiveSum(x):
            if x < 1 : return x
            return recursiveSum(x-1) + x
        ```
        
    - 조합의 수 (nCm)
    - 하노이의 탑
    - 피보나치 순열
- 장점 : 알고리즘을 간단하고 이해하기 쉽게 풀어냄
- 단점 : time complexity 부분에서 비효율적인 경우가 많음
- 이러한 특성 때문에 tree 자료구조를 이용하는 알고리즘에 활용

### <span style="color:#D0E7D2">Complexity</span>
- Time Complexity
- Space Complexity
- 다루는 데이터가 커질수록, 더 효율적인 complexity를 가지는 방식이 필요함 (2^n, n! 등의 complexity **X**)

## 👀 **<span style="color:#B0D9B1">CHECK</span>**

###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

- 코드로 elapsed time을 확인 -> 디버깅이나 time complexity를 직관적으로 확인하는 등에 활용
- list method `.pop()` 과 `.remove()`의 차이점
- big O notation - O of n 으로 읽음
- list의 앞과 뒤에 접근하는것은 O(1)의 time complextity를 갖는다.

## ‼️ **<span style="color:#B0D9B1">느낀 점</span>**

아직까지는 전공 과정 복습하는 느낌이고, 쉽다고 느꼈다. 막히는 부분은 없었다. 코드를 작성하면서 조금 더 나은 코드에 대해 생각해보는것 정도.    
공부하는 중이나 이후에 바로 TIL을 적는게 제일 좋다고 생각하는데, 블로그 세팅에 정신이 팔려서 나중에 작성하게 된 건 조금 아쉽다.    
오늘은 코어타임보다 일찍 강의를 들어버려서 시간 분배가 애매했다. 내일은 코어타임 시작할 때 듣기 시작하고, 그 전에는 전날 내용을 복습하거나, 알고리즘 문제를 풀고 code review를 작성하는 시간을 가져야겠다.


# **<span style="color:#79AC78">2. 블로그 세팅 과정</span>**

![https://blog.kakaocdn.net/dn/cKgXTh/btsyuerJaNh/HhtKTmduAbBOsbBmTD8g00/img.png](https://blog.kakaocdn.net/dn/cKgXTh/btsyuerJaNh/HhtKTmduAbBOsbBmTD8g00/img.png)
    

노션이나 옵시디언으로 TIL을 적고 깃에 업로드 한 적은 많지만 블로그에 올리는건 처음이라, 어디에 올리지 고민하다가 일단 티스토리 계정과 블로그를 만들었다.   
그런데 만들다 보니 **github repo와 연동되는 page를 세팅하고 싶다..!** 라는 욕심이 들기 시작했고,    
jekyll 등을 알아보다가 hugo를 활용한 페이지 세팅을 접하게 되어 장장 3시간을 투자했다.

그런데 이 글을 티스토리에 올리는 이유는? 세팅에 실패했기 때문 ㅠㅠ   
TIL 작성하고 나서 다시 도전해서 세팅 과정에 대해서도 글을 써 볼 예정이다.

+) 티스토리에서 md로 작성한게 예쁘게 안나와서 벨로그로 옮겼다 😂 훨씬 편한 것 같은데 깃허브랑 연동 전까진 벨로그로 해야겠다 :>

## <span style="color:#B0D9B1">(지금까지 참고한 링크들)</span>

- [휴고 사이트 생성 가이드](https://gohugo.io/getting-started/quick-start/)
- [휴고 사이트 깃허브 연결 가이드](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [사용 테마1](https://themes.gohugo.io/themes/salinger-theme/#quick-start-)
- [사용 테마2](https://github.com/CaiJimmy/hugo-theme-stack-starter)