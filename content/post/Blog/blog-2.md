+++
author = "Seorim"
title = 'Blog 제작기 #2'
url = '/Blog/blog-2'
date = 2023-10-25
draft = false
categories = [
    "Blog", 
]
tags = [
    "Blog", "hugo", "Github Pages",
]
+++


[Blog 제작기 #1](https://srlee056.github.io/p/day-1/)에서 이어지는 블로그 제작기 입니다. :>


## post template & 속성 설정 
### template
- `/archetypes` 내부에 `.md` 형식으로 template 생성
    - `/archetypes/default.md` : 아무 옵션도 설정하지 않았을 때 기본 생성되는 template
    - `/archetypes/til.md` : TIL 작성용으로 만든 template
        ```
        author = "Seorim"
        title =  "{{ replace .File.ContentBaseName "-" " " | title }}"
        date = {{ .Date }}

        categories = [
            "DevCourse",
        ]
        tags = [
            "TIL",
        ]
        ```
    - `--kind` 커맨드로 template 적용    
        ```
        hugo new content content/post/newpost.md --kind til
        ```


### 속성 설정
- `url`
    - 별도로 설정하지 않으면 title로 url 설정 됨
    ```
    title = 'Blog 제작기 #2'
    url = '/Blog/blog-2'
    ```
- `draft`
    - 빌드에 포함 시키는 여부. false : 포함 / true : 미포함
    - 기본 template `default.md`
    ```
    author = "Seorim"
    title = '{{ replace .File.ContentBaseName "-" " " | title }}'
    date = {{ .Date }}
    draft = false
    ```
- `structure`
    - post 폴더 안에 `<title>.md` 바로 생성 가능
    - `/<title>/index.md` 방식으로 따로 분류할수도 있음
    - 이외에 개인적으로 폴더를 나눠 분류할수도 있음

    ```
    content
    ├─  post 
    │   ├─ Blog
    │   │   └─ blog-2.md
    │   └─ DevCourse
    └─  _index.md
    ```

## 자동 빌드 /배포
### Github Actions
- `/.github/workflows/hugo.yaml`
- git push를 인식하여 자동으로 build/deploy 진행
- branch 이름을 `main` -> `master`
    (이전에 만든 repository라서 기본 브랜치 이름이 `master`였는데, hugo에서 제공하는 기본 코드의 기본 브랜치는 `main`으로 되어 있었다)

## 아바타, 파비콘 추가
### 아바타
- /assets/img/avatar.png 
- config file `hugo.toml`
    ```
    [params.sidebar.avatar]
    enabled = true
    local = true
    src = "img/avatar.png"
    ```

### 파비콘
- /static/img/favicon.ico
- config file `hugo.toml`
    ```
    [params]
    favicon = "/img/favicon.ico"
    ```

## config 수정
### 한국어 설정
- config file `hugo.toml`
    ```
    languageCode = "ko-KR"
    DefaultContentLanguage = "ko"
    
    ...

    [languages.ko]
    languageName = "Korean"
    title = "서림록"
    weight = 1
    [languages.ko.params]
    description = ""
    ```

### 기타 설정
- math : 수학 수식 표시 여부
- toc : 목차
- readingTime : 해당 글의 글자수를 기준으로 읽는 데 몇 분 정도 걸리는지 표시함

    ```
    [params.article]
    math = false
    toc = true
    readingTime = false
    ```




