+++
author = "Seorim"
title =  "Day 2"
date = 2023-10-25T17:42:55+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Algorithms", "Data Structure", "LinkedList", "Stack"
]
+++

# **<span style="color:#79AC78">TIL - LinkedList & Stack</span>**


## 📋 **<span style="color:#B0D9B1">공부 내용</span>**
### <span style="color:#D0E7D2">연결 리스트 Linked List</span>

   - **추상적 자료구조 Abstract Data Structures**
        - 내부 구현에는 신경쓸 필요 없는 구조 
        - data & a set of operations    
        - 이 두 가지를 **추상적으로 보여줌**
   - **Linke List?**
    	- Node가 선형적으로 연결된 구조 
      
   - **Node & LinkedList 구현**
        
       ```python
        class Node:
        	def __init__(self, item):
        		self.data = item
        		self.next = None # 다음 노드를 가리킴
        
        class LinkedList:
        	def __init__(self):
        		self.nodeCount = 0 # 노드의 총 갯수
        		self.head = None # 첫번째 노드
        		self.tail = None # 마지막 노드
       ```
        
    
   - **Linked List 연산 (operations)**
    
        
       linked list의 index는 1부터 시작 / 0은 다른 용도로 사용(Dummy node)
       ###### (실습으로 구현한 코드만 첨부했음)
       - kth element 참조
       - 리스트 순회
            
           ```python
            def traverse(self):
                    curr = self.head
                    returnList = []
                    while curr is not None: 
                        returnList.append(curr.data)
                        curr = curr.next
                    return returnList
           ```
            
       - 길이 얻기
       - 원소 삽입
           - Time complexity
              - 맨 앞에 삽입 : O(1)
              - 중간에 삽입 : O(n)
              - 맨 끝에 삽입 : (Tail pointer가 있기 때문에) O(1)
                
       - 원소 삭제
           - Time complexity 
              - 맨 앞에 삽입 : O(1)
              - 중간에 삽입 : O(n)
              - 맨 끝에 삽입 : O(n)
               → 이 상황을 피하기 위해 이중 연결 리스트를 사용
         ```python
            # 삭제한 node 데이터를 반환
            def popAt(self, pos):
            	if pos < 1 or pos > self.nodeCount:
            	  raise IndexError

            	if pos == 1:
            	  prev = None
            	  curr = self.head
            	  self.head = curr.next
            	else:
                prev = self.getAt(pos - 1)
                curr = prev.next
                prev.next = curr.next

            	if pos == self.nodeCount:
                self.tail = prev

            	self.nodeCount -= 1
            	return curr.data
         ```
       - 두 리스트 합치기 concat
        
   - **배열 Array vs 연결 리스트 Linked List**
        
        
        |  | 배열 | 연결 리스트 |
        | --- | --- | --- |
        | 저장 공간 | 연속된 위치 | 임의의 위치 |
        | 특정 원소 참조 | 매우 간편 | 선형탐색과 유사 |
        |  | O(1) | O(n) |
        
        *time complexity에 불리함이 있는데도 사용하는 이유는?*
        
   - **연결 리스트 Linked List의 힘**
      - 유연한 삽입 및 삭제
      - Head, Tail에 dummy node를 추가하여 간편하고 직관적인 설계 가능
      - 추가 구현 operations            
        - insertAfter(prev, node)
        - popAfter(prev) & popAt(pos)
            
            ```python
            def popAfter(self, prev):
            	curr = prev.next
            	if curr is None : 
                return None
            	if curr.next is None:
                self.tail = prev
            	    
            	prev.next = curr.next
            	self.nodeCount -= 1 # nodeCount 꼭 체크하기
            	
            	return curr.data
                    
            def popAt(self, pos):
              if pos < 1 or pos > self.nodeCount:
                raise IndexError
                  
              prev = self.getAt(pos-1)
              return self.popAfter(prev)
            
            ```
            
    
---        
### <span style="color:#D0E7D2">양방향 / 이중 연결 리스트 Double Linked List</span>
   - **양쪽**으로 연결된 link
    - next node, previous node로 두 방향 모두 진행 가능
    - 메모리 사용량이 늘어나지만, 앞에서뿐만 아니라 뒤에서도 데이터를 찾아갈 수 있다는게 장점
        - getAt() 함수 또한 pos가 중간값 이상일 때는 뒤에서부터 찾도록 구현할 수 있음
   - **operations**
        - reverse
            
            ```python
            def reverse(self):
                result = []
                curr = self.tail
                while curr.prev.prev:
                    curr = curr.prev
                    result.append(curr.data)
                return result
            ```
            
        - insertBefore
            
            ```python
            def insertBefore(self, next, newNode):
                prev = next.prev
                newNode.next = next
                newNode.prev = prev
                prev.next = newNode
                next.prev = newNode
                self.nodeCount += 1
            		return True
            ```
            
        - popAfter, popBefore, popAt
            
            ```python
            def popAfter(self, prev):
                curr = prev.next
                next = curr.next
                prev.next = next
                next.prev = prev
                self.nodeCount -= 1
                return curr.data
            
            def popBefore(self, next):
                curr = next.prev
                prev = curr.prev
                prev.next = next
                next.prev = prev
                self.nodeCount -= 1
                return curr.data
            
            def popAt(self, pos):
                if pos < 1 or pos > self.nodeCount:
                    raise IndexError
                prev = self.getAt(pos - 1)
                return self.popAfter(prev)
            
                # next = self.getAt(pos+1) # getAt 함수가 pos == nodeCount+1 일 때 지원을 하지 않아서 사용은 어려움
                # next = self.getAt(pos).next #로 사용 가능
                # return self.popBefore(next)
            ```
            
        - concat(self, L)
            
            ```python
            def concat(self, L):
                lastNode = self.tail.prev
                firstNode = L.head.next
                lastNode.next = firstNode
                firstNode.prev = lastNode
                self.tail = L.tail
                self.nodeCount += L.nodeCount
            ```

---
### <span style="color:#D0E7D2">스택 Stack</span>
   - **data element를 보관할 수 있는 선형 구조 / LIFO**
   - **operations**
       - size()
       - isEmpty()
       - push(x)
            - 꽉 찬 스택에 push(x)로 원소를 더 추가하려고 할 때 `stack overflow` 발생
       - pop()
            - 비어있는 스택에서 pop()으로 없는 원소를 꺼내려 할 때 `stack underflow` 발생
       - peek()
            - 데이터 참조, 제거하지는 않음
   - **추상적 자료구조로 구현**
        - Array 또는 LinkedList 이용
        - 만들어져있는 Stack library 를 import 할 수도 있음
          `from pythonds.basic.stack import Stack`
          
   ---          
 
   - **스택의 응용 1) 후위 표기법으로 변환**
       - 중위 표기법 (infix notation) : (A+B) * (C+D)
        → 후위 표기법 (postfix notation) : AB+CD+*
       - 알고리즘 설계
           - operator 연산자를 스택에 넣는 방식
           - 연산자 우선순위 설정
                
                ```python
                prec = {'*':3, '/':3, '+':2, '-':2, '(':1}
                ```
                
           - 구현 코드
                
                ```python
                def solution(S):
                    opStack = ArrayStack()
                    charList = []  # 수식을 리스트 형태로 저장한 후 .join을 통해 문자열로 변환
                
                    for s in S:
                        if s in prec.keys():  # 연산자 + 여는 괄호
                            if s == "(":
                                opStack.push(s)
                            else:
                                while not opStack.isEmpty():  # 스택이 비어있는 동안 계속
                                    if prec[opStack.peek()] >= prec[s]:  # 스택 맨 위의 우선순위가 높거나 같은 경우에만
                                        charList.append(opStack.pop())  # pop()하여 문자열에 출력
                                    else:
                                        break
                                opStack.push(s)
                
                        elif s == ")":  # 닫는 괄호
                            while opStack.peek() != "(":  # 여는 괄호가 나올때까지 모든 연산자를 꺼내 출력
                                charList.append(opStack.pop())
                            opStack.pop()  # pop '('
                
                        else:  # 피연산자
                            charList.append(s)
                
                    while not opStack.isEmpty():  # 스택에 남아있는 연산자들을 모두 출력
                        charList.append(opStack.pop())
                
                    answer = "".join(charList)
                    return answer
                ```
                
   - **스택의 응용 2) 후위 표기법 계산**
        - 앞에서부터 뒤로 읽어나가면서 먼저 만나는 연산자를 먼저 계산
        - 알고리즘 설계
            - operands 피연산자들을 스택에 넣는 방식
            - 구현 코드
                
                ```python
                def postfixEval(tokenList):
                    valStack = ArrayStack()
                    
                    for t in tokenList:
                        if type(t) is int:
                            valStack.push(t)
                        else:
                            b = valStack.pop()
                            a = valStack.pop()
                            if t == '*':
                                valStack.push(a*b)
                            elif t == '/':
                                valStack.push(a/b)
                            elif t == '+':
                                valStack.push(a+b)
                            elif t == '-':
                                valStack.push(a-b)
                    
                    return valStack.pop()	
                ```
                

## 👀 **<span style="color:#B0D9B1">CHECK</span>**

###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

- member field, member method 
  + 용어의 뜻은 알지만 애매해서 다시 정리하기 위해 찾아봄    
    [정리에 참고한 사이트](https://as-i-am-programing.tistory.com/7)
    
- 추상적 자료구조
    - abstract data type vs. data structure
        - an ADT (Abstract Data Type) is more of a logical description, while a Data Structure is concrete.
    - [정리에 참고한 사이트 : 추상적 자료형 vs. 자료구조](https://velog.io/@hwan2da/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%B6%94%EC%83%81%EC%9E%90%EB%A3%8C%ED%98%95)
- dummy node를 추가한 구조의 linked list
- stack underflow

## ‼️ **<span style="color:#B0D9B1">느낀 점</span>**

오늘은 두 가지 교훈(?) 을 얻었다.

1. 한번에 두가지 일을 하지 않기
    강의를 들으면서 TIL을 적으려고 했는데, 외려 더 정신없고 힘들었다. 강의 듣는 중간에 적다 보니, 그냥 받아쓰기가 되는것도 별로였다. 큰 주제 (오늘 같은 경우 LinkedList / Stack)를 다 듣고 정리하는게 나을 듯!
    
2. 사소한 실수 하지 않고 꼼꼼하게 체크하기
    오늘 연습문제를 풀면서 
    >
    node.data 대신 node 객체를 반환함
    linked list의 nodeCount 증감시키는걸 빼먹음
    
    명시된 조건을 빼먹는 실수를 저질러서 디버깅 한다고 다 합해서 한시간 가까이 소모했다.
    몰라서 못 푸는것 보다 이런 부분에서 꼼꼼하지 못해서 못 푸는게 더 싫다. 심지어 원래는 그렇게 자주 하는 실수도 아니어서 자존심이 더 상했다. ㅠㅠ    
    그래도 이런 날이 있어야 앞으로 안 그럴 수 있으니까 계속 담아두지는 말아야지!
    
내일은 강의와 실습을 꼼꼼하게 진행하고, 내가 이해한 내용을 토대로 TIL을 잘 적어봐야겠다. :>. 
그리고 내일은 github 블로그에 올려봐야지! 🔥 어렵지만 할 만한 가치가 있어보인다 😊