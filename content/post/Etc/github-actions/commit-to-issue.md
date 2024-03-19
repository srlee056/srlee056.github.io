+++
author = "Seorim"
title =  "Commit to Issue"
slug = "commit-to-issue"
date = 2024-03-19T15:26:19+09:00

categories = [
    "GitHub Actions",
]
tags = [
    "TIL","자동화"
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

# 백준 허브를 통해 자동으로 커밋되는 코드를 Issue&Project로 기록하기

## 개요

- 프로젝트(?) 시작 계기
  - 백준 허브: 백준 등 문제를 풀면 연결된 repo로 코드를 자동 commit, push해주는 크롬 확장 프로그램
  - 파일은 자동으로 저장되지만, 분류와 언제 풀었는지 등 기록하는건 직접 해야하는 상황
  - Issue와 Project 기능을 사용해 언제 풀었는지 파악하고 복습과 분류 등을 하기 위해 workflow를 작성하기로 함
- 프로젝트 기간 : 2024.03.18 ~ (작성하는 지금 약 80% 완성)
- 프로젝트 사용 기술 : GitHub Actions의 workflow (yaml) & ChatGPT

## 과정

어제 고군분투 한 과정을 자세하게 적기엔 너무 번거로워서 일단 간단하게만 적으려고 한다.
[참고링크 1 - GitHub Actions를 사용하여 프로젝트를 자동화](https://docs.github.com/ko/issues/planning-and-tracking-with-projects/automating-your-project/automating-projects-using-actions)

1. commit to project

   - 백준허브 프로젝트에서 자동 커밋되는 문제들 외에, 직접 커밋할 때에도 issue에 추가됐으면 좋겠다고 생각.
   - 여러 commit을 한번에 push할수도 있으니, 이를 위해 push가 일어날 경우, 포함된 모든 commit에 대해 체크하고 issue로 발행해야 하는 문제풀이 커밋인지, 기타 커밋인지 확인함
   - 그러나 하나의 step안에서 너무 많은 코드를 작성해야 하는 불편함이 있었음
   - 또한 issue 발행 후 이를 project item으로 발행하기 위해 issue 발행의 nodeid가 필요한데 이를 기존 구조에서 얻기가 까다롭다고 여김
     <br>
   - 결과적으로 workflow 파일을 두개로 나누어 재 작성 시작

2. commit to issue
   - 1에서 언급한 과정을 진행
   - 기존에 푼 문제를 다시 풀었을 때 (코드 수정 등) 새로 issue를 만드는 대신 기존 issue에 댓글을 남김

- trouble shooting

  - 한글 인식 문제
    ```
        Committed Files:
        "\353\260\261\354\244\200/Silver/2178.\342\200\205\353\257\270\353\241\234\342\200\205\355\203\220\354\203\211/README.md"
        "\353\260\261\354\244\200/Silver/2178.\342\200\205\353\257\270\353\241\234\342\200\205\355\203\220\354\203\211/\353\257\270\353\241\234\342\200\205\355\203\220\354\203\211.py"
    ```
    decode하는 방식으로 변경하여 해결
  - branch 인식 문제 및 file 경로에 double qoute `"` 들어가는 문제

    ```
    Committed Files:

    "백준/Silver/2178. 미로 탐색/README.md"
    "백준/Silver/2178. 미로 탐색/미로 탐색.py"
    ```

    branch 부분을 `main`으로 하드코딩하고, file 이름 가져오는 부분을 수정하여 `"`제거

3. issue to project
   - 이슈가 발행될 때 trigger 됨
   - 해당 이슈에 대한 project item을 만들고, 문제를 푼 날짜를 기록하며 status를 Done으로 바꾸는 기능을 구현

## 결과

[링크](https://github.com/srlee056/algorithm-study/tree/main/.github/workflows)

## 회고 및 추가 기능

1. 기록을 더 세부적으로 남겨야함 (약속 나가야 해서 기본만 적음. 저녁에 추가할 것)
2. 생각중인 추가 기능
   - 라벨 또는 project item 속성으로 문제 종류나 난이도 기준 분류하면 좋을 것 같음
   - solved.ac에 이런 기능이 이미 있는지도 확인해봐야 함
   - github script를 사용하면 기존 코드를 더 간결하게 만들 수 있는지 알아보기
   - hardcoding된 main branch 이름 - ref? 이런걸로 받아올 수 있는것으로 아는데 수정해보기
   - commit message말고 파일 이름을 통해 title에 문제번호 + 문제이름 이 들어가게 하기
   - bakjoon 외에 programmers 등은 어떻게 commit되고 파일 이름은 어떻게 되는지 확인해서 확장에 용이하게 하기
   - 기존 코드 에러 처리하기
