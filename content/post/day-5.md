+++
author = "Seorim"
title =  "Day 5"
date = 2023-10-25T17:43:06+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Algorithms", "Data Structure", "Heap", "DP", "DFS&BFS"
]
+++
# **<span style="color:#79AC78">TIL - Heap / Dynamic Programming / DFS&BFS (문제풀이 위주)</span>**

## 📋 **<span style="color:#B0D9B1">공부 내용</span>**

### <span style="color:#D0E7D2">Heap</span>
****

#### <더 맵게> 문제 풀이

```python
import heapq #heap 라이브러리 사용

def solution(scoville, K):
    answer = 0
    # heapify, heappop, heappush 활용
    heapq.heapify(scoville) #min heap 구성
    while True:
        s1 = heapq.heappop(scoville)
        if s1 >= K : #모든 스코빌 지수가 K이상
            break
        elif len(scoville) == 0: # K이상으로 만들 수 없는 경우
            answer = -1
            break
        s2 = heapq.heappop(scoville)
        newS = s1 + s2*2
        heapq.heappush(scoville,newS)
        answer += 1
    return answer
```
<br>

### <span style="color:#D0E7D2">Dynamic Programming</span>
***

#### DP(Dynamic Programming)
> 알고리즘 진행에 따라 `탐색해야 할 범위를 동적으로 결정`하여 탐색 범위를 한정
        
 **EX1)** *피보나치 수열*
 
| Complexity | 재귀함수 | DP |
| --- | --- | --- |
| Time | O(2^n) | O(n)|
| Space | O(n) | O(n)|

참고 : [Recursion vs Dynamic Programming - Fibonacci](https://towardsdatascience.com/dynamic-programming-i-python-8b20387870f5)

 **EX2)** *knapsack problem*   
weight / value를 가진 여러 물건이 있을 때, weight 제한이 있는 베낭에 value의 합이 가장 높도록 물건을 골라 담는 문제

<br>

#### <N으로 표현> 문제 풀이

```python
def solution(N, number):
    strN = str(N)
    # 직관적인 코드를 위해 다음과 같이 index : 1~8 사용 설정 (index 0은 사용하지 않음)
    numberUsed = [{} for _ in range(9)]
    for i in range(1, 9):
        numberUsed[i] = {int(strN * (i))}   # N을 i번 나열한 숫자 ex) 555, 7777, ...
        for j in range(1 , i):
            for n1 in numberUsed[j]:
                for n2 in numberUsed[i-j]:
                    newNumbers = [n1*n2, n1+n2, n1-n2]
                    if n2 != 0 : newNumbers.append( n1//n2 )
                    numberUsed[i].update(newNumbers) #set1.update(set2) 사용하여 원소 추가. set 아닌 list를 넘겨줄수도 있다.
        if number in numberUsed[i]:
            return i
    return -1
```
<br>

### <span style="color:#D0E7D2">DFS & BFS</span>


#### 1. Depth First Search
> 한 vertex에서 인접한 모든 vertex를 방문할 때, 인접한 vertex를 기준으로 `깊이 우선 탐색`을 끝낸 후 다음 vertex로 진행하는 방식   
`스택`을 이용하여 어느 정점에서 DFS를 진행하고 있는지를 기억


#### 2. Breadth First Search

> 한 vertex에서 인접한 모든 vertex를 방문하고, 방문한 인접 vertex를 기준으로 `너비 우선 탐색`을 진행하는 방식

#### 3. Graph
> -vertex(=node) & edge(=link)   
-directed / undirected graph

#### <여행 경로> 문제 풀이
DFS를 응용한 재귀 한 붓 그리기   
```python
def solution(tickets):
    routes = {}
    for a, b in tickets: #출발지, 도착지
        routes[a] = routes.get(a, []) 
        routes[a].append(b)
    
    for r in routes:
        routes[r].sort(reverse=True) 
        # 역순인 이유? 파이썬의 데이터 삭제 과정을 살펴보면 뒤에서 뽑는게 더 효울적이기 때문    
    stack = ["ICN"]
    path = []
    while 0 < len(stack):
        top = stack[-1]
        if top not in routes or len(routes[top]) == 0: # 이 공항에서 떠나는 티켓이 존재하지 않음
            path.append(stack.pop()) 
        else :
            stack.append(routes[top].pop())
    return path[::-1]
```

<br>

### <span style="color:#D0E7D2">추가 문제풀이</span>
<details>
<summary>문제 링크 모음(많이 김)</summary>

<!-- summary 아래 한칸 공백 두어야함 -->
<br>
프로그래머스 코딩테스트 문제 링크 (코스에 있는 문제여도 프로그래머스 코테 링크로 적음)


- [완주하지 못한 사람](https://school.programmers.co.kr/learn/courses/30/lessons/42576)
- [올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)
- [스킬트리](https://school.programmers.co.kr/learn/courses/30/lessons/49993)
<br>
- [배달](https://school.programmers.co.kr/learn/courses/30/lessons/12978)
<br>
- [세 소수의 합]
- [주사위 게임]
- [사탕 담기](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236812)
<br>
- [빙고]
- [방문 길이](https://school.programmers.co.kr/learn/courses/30/lessons/49994)
- [쇠막대기]
- [자물쇠와 열쇠](https://school.programmers.co.kr/learn/courses/30/lessons/60059)
<br>
- [게임 아이템]
<br>
- [기능개발](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236806)
- [더 맵게](https://school.programmers.co.kr/learn/courses/30/lessons/42626)
- [배상 비용 최소화]
- [문자열 압축 코드]
<br>
- [카펫](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236811)
- [예산](https://school.programmers.co.kr/learn/courses/30/lessons/12982)
- [N-Queen](https://school.programmers.co.kr/learn/courses/30/lessons/12952)
- [버스 여행]
<br>
- [예산_소팅](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236805)
- [최솟값 만들기](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236808)
- [가장 큰 수](https://school.programmers.co.kr/app/courses/19071/curriculum/lessons/236807)
- [N으로 표현](https://school.programmers.co.kr/learn/courses/30/lessons/42895)
- [2 x n 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/12900)
- [등굣길](https://school.programmers.co.kr/learn/courses/30/lessons/42898)
- [가장 긴 팰린드롬](https://school.programmers.co.kr/learn/courses/30/lessons/12904)
<br>

</details>

## 👀 **<span style="color:#B0D9B1">CHECK</span>**
###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

#### PEP 8

파이썬이 추구하는 코드 스타일. 들여쓰기, 공백, 변수명 작성 규칙 등이 포함된다.   
  - [PEP 8 - Style Guide for Python Code(공식 문서)](https://peps.python.org/pep-0008/)   
  - [PEP8 스타일 가이드를 설명하는 블로그(한국어)](https://wayhome25.github.io/python/2017/05/04/pep8/)   

#### Tim sort

Insertion sort + Merge sort   
- Insertion sort는 `n이 작을 때` (Quick sort보다도) 빠름   
- 전체를 작은 덩어리로 잘라 Insertion sort -> Merge 
- [Tim sort에 대해 설명하는 글](https://d2.naver.com/helloworld/0315536)


#### DP 익숙해지기
- knapsack problem 등 문제풀이를 통해 익숙해지기

## ❗ **<span style="color:#B0D9B1">느낀 점</span>**

오늘은 DP(Dynamic Programming)과 DFS&BFS의 개념에 대해 가볍게 배우고, 문제풀이를 위주로 강의가 진행되었다.   

아는 알고리즘들이고 어제 강의의 문제가 나에겐 어렵지 않았기 때문에, 해설 강의를 듣기 전에 문제를 먼저 풀어 보았다. 문제를 풀다가 막히는 부분이 있으면 강의를 틀고, 해결되면 멈추고 다시 풀어보곤 했다.   
Heap 문제를 제외한 두 문제는 해설 없이 풀었고, Heap 문제도 heapq 라이브러리를 사용해야하는 것을 깨닫고는 금방 풀 수 있었다.

이번주 강의를 듣고 문제를 풀면서 느낀점이 몇개 있다. 
1. 나는 알고리즘 자체는 꽤 잘 알고 활용도 잘 하는 편이지만 라이브러리 사용에 있어서 소극적인 면이 있다. 문제를 풀 때 파이썬 표준 라이브러리조차도 잘 import하지 않고 Dictionary나 Set정도만 사용하는 편이었다. *(Heap도 자체적으로 구현하거나 list를 사용하곤 했는데 왜 이런 습관이 들었는지는 잘 모르겠다.)*    
이번 강의를 들으면서 표준 라이브러리에 있는 여러 데이터형을 활용했고 다음에 문제를 풀 때도 적극적으로 활용하면서 더 효과적인 코드를 작성하는데 가까워지려고 한다.   

2. 나는 CS 용어를 잘 모른다. 아예 모른다는게 아니라, 많이 헷갈리며 정확한 명칭을 찾아보지 않았다는 의미이다. member method, list comprehension 등 용어들은 분명 내가 궁금해서 찾아봤지만 아직도 헷갈리거나 명칭 자체를 몰랐던 것들이다. 강의를 통해 명확해지거나 새롭게 알게 된 용어들이 있어서 좋았고, 이제부터는 모르는 부분에 대해 검색할 때는 아예 용어와 그 정의부터 찾는것부터 시작해야겠다고 다짐했다. 

내일은 강의가 없는 날이다. 하지만 해야 할 일들은 있다. 오늘까지 배운걸 복습하고, 배운 자료구조와 알고리즘을 활용하는 문제들을 lv3정도로 풀어 볼 것이다. 내일도 화이팅!!