+++
author = "Seorim"
title =  "Day 3"
date = 2023-10-25T17:42:58+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Algorithms", "Data Structure","Queue", "Tree", "Heap"
]
+++

# **<span style="color:#79AC78">TIL - Queue, Tree & Heap</span>**

## 📋 **<span style="color:#B0D9B1">공부 내용</span>**

### <span style="color:#D0E7D2">Queues</span>

- Queue?
  - FIFO (First In First Out)
  - operations
    - enqueue
    - dequeue
      - 선형 배열으로 구현: `O(n)` -> 연결 리스트로 구현하는것이 더 좋음
     
- Circular Queues
  - 배열의 한쪽 끝과 다른쪽 끝이 닿아 있는 모습
  - 원소의 개수가 정해져있음
  - `front / rear 포인터`를 기억하고, dequeue된 원소 저장소는 재활용
  - 선형 배열으로 구현하는것이 더 좋음

- Priority Queues
  - 원소들 사이의 우선순위를 따름 
  - 우선순위 구현 
    - 원소를 넣을 때 enqueue vs. 원소를 꺼낼 때 dequeue
      - 넣을 때(enqueue) 우선순위에 따라 정렬하는것이 조금 더 나은 Time Complexity를 가짐
      - 데이터를 관리하고 활용하는 데에도 더 편리함


### <span style="color:#D0E7D2">Trees</span>

- **Tree?**
  - 구성 
    - root node 
    - interner nodes
    - leaf nodes
  - 특성
    - parent node / child node
    - 노드의 수준 (level) : root node로부터 거리
    - 노드의 차수 (degree) : 노드의 자식 수 
    - 트리의 높이(height) 또는 깊이(depth) : 제일 큰 level + 1
    - subtree 
    
- **Binary Trees**
    - 모든 node의 degree <= 2
    - operations
        - size()
        - depth()
        - 순회 traversal
            - Depth First Traversal : 재귀 호출을 통해 구현 
              - inorder
              - preorder
              - postorder
            - Breadth Tirst Traversal : queue 사용!
              - level이 낮은 노드를 우선으로 방문
              - 같은 level인 경우 부모 노드의 순서를 따름
    - 포화 이진 트리 (full binary trees)
      - 모든 node의 degree == 2
    - 완전 이진 트리 (complete binary trees)
      - (depth k) `level k-2`까지는 full binary tree, `level k-1`은 왼쪽부터 node 채워짐

- **Binary Search Trees**
    - 모든 노드에 대해 다음 성질을 만족하는 Binary Tree
        - 왼쪽 subtree's data < 현재 node's data < 오른쪽 subtree's data
    - 장단점
        - 장점 : 원소의 추가, 삭제가 편함
        - 단점 : 큰 공간을 소요함 (연결 리스트로 구현)
    - operations
        - insert()
        - remove()
        - lookup()
        - inorder()
        - min(), max()

### <span style="color:#D0E7D2">Heap</span>
- Heaps?
    - Binary Tree의 한 종류 (binary heap)
    - Min / Max heap
        - root node가 항상 최소/최대 값을 가진다
        - complete binary tree
            - `n` nodes -> depth `log(n)+1`
            - insert / remove operation의 Time Complexity : `O(log(n))`
        - subtree 또한 Min / Max heap

    - Binary Seacrh Tree vs. Heap?
        |     | BST | Heap |
        | --- | --- | ---  |
        | 데이터 정렬 | 크기순서대로 완전 정렬| 완전 정렬 X |
        | 데이터 검색 | O | X | 
        | 완전 이진 트리 | X | O |
        | 연산 시간 | (최악의 경우) O(n)| O(log(n))|
        | 선형 배열로 구현 | X | O | 
        
    - operations
        - insert()
        - remove()

    
## 👀 **<span style="color:#B0D9B1">CHECK</span>**

###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

- **노드의 차수(degree) : # of children**
    - binary tree : 모든 노드의 degree가 항상 2이하
    - leaf nodes : degree 0
- **단축 평가**
    - [단축평가에 대해 참고한 블로그](https://pydole.tistory.com/entry/Python-%EB%8B%A8%EC%B6%95%ED%8F%89%EA%B0%80short-circuit-evalution)
    - 2개 이상의 논리 조건식이 있을 경우에, **앞 조건이 계산한 값에 의해 결과가 확실해지면 두번째 조건은 확인하지 않음**
    - False and (), True or () 인 경우 등이 해당됨


## ❗ **<span style="color:#B0D9B1">느낀 점</span>**
어제의 교훈이 있어서 그런지, 오늘은 프로그래밍 과정에 실수가 적었다.
그리고, 자료구조를 구현하는 과정에서 더 나은 방식에 대해 고민하는 상황이 많아졌다.

오늘 주로 고민한 부분은

```
코드를 더 직관적으로 적고싶음
- 반복되는 같은 코드를 합칠 수 있는가?

같은 코드 다른 효율?
- 같은 기능을 하지만 살짝 다른 두 코드 중 더 나은것은?
```
정도였다. 

전자는 내가 평소 프로그래밍 할 때 자주 하긴 하는데, 항상 고민하는 부분이다.
반복되는 기능을 함수로 빼던가, 적용되는 변수를 리스트로 묶어서 반복하는 등의 방법을 쓰는데, 다른 방법은 뭐가 있을까 고민하고 있다.

후자는 Complexity를 구하는 방법에 대한 의문이 아니다. 실제로 이 operation이 어떻게 구현되고 있는지, 왜 그렇게 구현되었는지가 궁금한 것이다.    
실습 문제에서 우선순위 queue를 구현할 때, skeleton code의 dequeue 함수는 마지막 elements를 가져오는 방식으로 되어있었다.(`getAt(data.size())`)     
나는 처음 element를 가져오는 방식으로 생각하고(`getAt(1)`) 이에 해당하는 enqueue 함수를 작성했기 때문에 잠깐 막혔다가, 결국은 눈치채고 해결했다.   
그 코드를 보면서 이렇게 구현하는게 더 나은건지 궁금증이 들었고, 같은 기능을 하지만 결함이 적은(?) 코드를 작성하자는 생각이 들었다.

오늘은 추가 알고리즘 문제들이 있고 아직 풀지 않았는데, 쉬운 문제들이지만 이 두가지를 생각하면서 풀려고 노력해야겠다.
