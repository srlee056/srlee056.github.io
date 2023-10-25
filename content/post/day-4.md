+++
author = "Seorim"
title =  "Day 4"
date = 2023-10-25T17:43:03+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Algorithms", "Data Structure", "Hash", "Greedy", "Sort"
]
+++

# **<span style="color:#79AC78">TIL - Hash / Greedy & Sort (문제풀이 위주)</span>**

## 📋 **<span style="color:#B0D9B1">공부 내용</span>**

### <span style="color:#D0E7D2">Hash</span>
- Hash?   
      개념 정리가 필요하다고 느껴서 글을 따로 발행했다.

    - [Understanding Hash Table](https://www.baeldung.com/cs/hash-tables)

    -  [(번역 및 정리)Hash Table 이해하기](https://velog.io/@srlee056/Hash-Table-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

- `완주하지 못한 선수` 문제풀이
    - python dictionary 를 활용하여 hasing 구현
    ```python
    def solution(participant, completion):
        answer = ''
        pDict = {}
        # 참가자/ 완주자 정보가 주어질 때, 이름을 key로 활용하여 dictionary 형태에 넣고 빼는 방식으로
        # (동명이인이 있을 때에도) 어떤 이름을 가진 사람이 완주를 하지 못했는지 확인할 수 있다. 
        for p in participant:
            pDict[p] = pDict.get(p, 0) + 1 #.get()을 통해 default 값을 세팅하는 한 방법
        for p in completion:
            pDict[p] -= 1
        
        incompletion = [k for k, v in pDict.items() if v > 0]        
        answer = incompletion[0]
        return answer
    ```


### <span style="color:#D0E7D2">Greedy Algorithm</span>

- 탐욕법(greedy algorithm)
    - 알고리즘의 각 단계에서 **그 순간의 최적의 선택**을 함
    - 탐욕법으로 최적해를 찾을 수 있는 문제   
        = 현재 선택이 마지막 답의 최적성을 해치지 않는 문제
    
- `체육복` 문제풀이
    - 비슷해보이는 조건문이라해도 그 순서에 따라 전혀 다른 결과가 나올 수 있음
    - 작은 번호부터 큰 번호로 순회하기 때문에, 작은 번호가 조건을 만족하는지 먼저 고려함

- `큰 수 만들기` 문제풀이
    - 매 단계마다 작은 숫자를 지우는 탐욕법을 사용하며, 이 방식은 마지막에 최적성을 충족하게 됨
    ```python
    def solution(number, k):
        collected = []
        for i, num in enumerate(number):
            # collected에 미리 들어간 원소가 있을 것
            # 제일 마지막으로 들어간 원소는 현재 num 값보다 작을 것
            # (제거해야 하는 숫자의 개수) k가 0보다 클 것
            while len(collected) > 0 and collected[-1] < num and k >0:
                collected.pop()
                k -=1
                
            if k == 0:
                collected += list(number[i:])
                break
            collected.append(num)
        
        # 제거해야 하는 숫자의 개수가 남아있을 때 
        # (코드가 너무 직관적이고 예뻤다. :>)
        collected = collected[:-k] if k > 0 else collected
            
        answer = ''.join(collected)
                
        return answer
    ```


### <span style="color:#D0E7D2">Sort</span>

- `가장 큰 수` 문제풀이
    - 자릿수가 다른 숫자들을 문자열처럼 나열하여 더 큰 숫자를 만들 때, 숫자들의 우선순위를 정하는 방법은?
    ``` python
    sortedNumbers = sorted(map(str, numbers), key = lambda x : (x * 4)[:4], reverse = True)

    # map(str, numbers) : numbers 정수 리스트를 문자열 리스트로 변환
    # sorted(..., reverse = True) : 주어진 조건으로 정렬하며, 내림차순으로 반환
    # lambda x : (x*4)[:4] : 네 자리 수까지 주어지므로 4번 반복 후 네번째자리까지 끊음
    ```
    
## 👀 **<span style="color:#B0D9B1">CHECK</span>**

###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

- Hash
    - 가볍게 정리했지만 더 자세하게 찾아보고 싶음 (활용되는 곳, 특성을 구현하는 법, hashing function, collision 등)
- list comprehension
    - 한 줄에 반복문 할당 배열생성 등이 한번에 일어남
    - [list comprehension 설명 블로그 글](https://shoark7.github.io/programming/python/about-list-comprehension-python)
    - 쓸 줄 아는 방식이지만 용어를 처음 알게 됨 (예전에 배우고 까먹은 게 분명)
    
- list slicing 에 바로 대입하여 직관적으로 코드 작성 가능
    - EX) list[1:3] = [1, 2] 

## ❗ **<span style="color:#B0D9B1">느낀 점</span>**

어제 더 나은 코드를 위해 어떤점을 고려해야하는지 많은 고민을 했는데, 오늘 강의에서 그 부분을 짚어줘서 좋았다.

오늘은 각 알고리즘이나 자료구조에 대한 설명보다는 문제 풀이와 해설을 위주로 강의가 진행되었는데, 그래서 나 스스로 그 개념에 대해 찾아보고 정리하는 시간이 필요했다. greedy나 sort는 이미 잘 알고 있는 부분이지만 hash는 헷갈리는 부분이 있어서 블로그와 document를 읽어보면서 글로 정리하는 시간을 가졌다. 

깊이가 정해져있지 않다 보니 원하는 만큼 궁금해하고 파헤칠 수 있었지만, 내가 그 내용을 받아들이고 정리할 수 있는가는 별개의 문제임을 깨달았다. hash를 설명하는 블로그 글에 연결된 링크를 타고 여러 글을 읽어가다 보니, 어느새 encoding&decoding, scheduling에 대한 글을 읽고 있었다. 이런 글들을 읽고 어느정도 이해할 수는 있었지만, 글로 정리하거나 그 사이의 관계를 명확하게 아는것은 큰 차이가 있었고, 머리속이 혼란스러웠다. 어떤 주제와 관련된 내용을 잘 찾고 정리하는것도 많은 노력이 필요하다는 걸 새삼 느끼는 순간이었다.

오늘처럼 깊이가 없는 공부를 해야하는 순간은 계속 있을것이고(있어왔고), 정해진 주제에 대해 찾아본 내용들을 글로 정리하면서 나만의 기준을 정립해야겠다는 결론을 내렸다. 

+) 퇴고하기 위해 글을 읽어보는데 두서없고 추상적이라 맘에 들지 않음..(ㅠㅠ) 문장력을 키우고 싶은데 온라인으로도 필사할 수 있는지 찾아봐야겠다.
