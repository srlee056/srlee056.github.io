+++
title = '231023'
date = "2023-10-23"

tags = [
    "TIL", "HTML"
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

# <span style="color:#79AC78">TIL - HTML</span> 

## 📋 <span style="color:#B0D9B1">공부 내용</span>

**<span style="color:#FAF8ED">HTML</span>**
> Hypertext Markup Language   
웹 브라우저가 이해할 수 있는 "언어"

**<span style="color:#FAF8ED">CSS</span>**
> Cascading Style Sheets   
문서를 예쁘게 "꾸미는" 언어

**<span style="color:#FAF8ED">JavaScript</span>**
> JS = JavaScript   
문서에 "기능"을 만들어주는 언어

---

### <span style="color:#D0E7D2">HTML - 기본 문법</span>

#### <span style="color:#F9B572">태그</span>   
- 컨텐츠를 갖는 태그 / 가지지 않는 태그 
- 열리는 태그 & 닫히는 태그 / 단일 태그 (셀프 클로징)
- `<div> contents </div> / <br />`

#### <span style="color:#F9B572">속성과 값</span>
- `<div attributes = value>content</div>`
- `<div title = "제목"> ...`
- title : 전역 속성이라 모든 태그에서 사용 가능
- 속성(Attributes)의 종류는 아주 많고 다양함 

#### <span style="color:#F9B572">부모요소 & 자식요소</span>
```
html
├─ head
│   └─ title
└─ body
```
html (부모) - head, body (자식)   

부모/자식 구조를 파악하기 좋게, 들여쓰기/내어쓰기 **깊이(depth)를 잘 지켜**서 작성해야 함   
탭 잘 쓰라는얘기 

#### <span style="color:#F9B572">HTML 주석</span>

```
<!-- 주석 내용 -->
```

```
<!-- 
    줄바꿈 가능
-->
```

```html
<!--
    <!-- -->
-->
이처럼 주석 안에 주석을 넣으면 바깥의 여는 태그가 안쪽 닫는 태그를 인식해 주석이 풀리게 된다. 고로 주석 안에 주석은 X 
```
웹 브라우저의 소스보기 등에서 확인 가능하므로 보안이 필요한 정보는 적지 않아야 한다.

### <span style="color:#D0E7D2">HEAD</span>
> 사용자에게는 보이지 않지만, 문서의 정보가 담기는 영역

#### <span style="color:#F9B572">타이틀</span>
- 웹 브라우저 탭이나 창에서 표시되는 `문서의 제목`

#### <span style="color:#F9B572">메타 데이터</span>
- 인코딩 정보
    - charset(character set) : 문서에서 허용하는 문자의 집합
    - 영어만 허용하는 규칙(ISO-8859-1)을 사용할 경우, 한글은 제대로 출력이 되지 않음
    - `<meta charset = "ISO-8859-1">`
    - `utf-8`(전 세계 언어 지원) 을 기본으로 사용
- 문서 설명
    - `<meta name = "description" content = "이 문서는 실습 문서입니다.">`
- 문서 작성자
    - `<meta name = "author content = "srlee"`
    
#### <span style="color:#F9B572">CSS, Javascript</span>
> 문서 외형에 영향을 주는 태그들로 구성
- style
    ```html
    <!-- 문서 글자색을 blue로 만드는 코드 -->
    <style> 
        body {
            color: blue;
        }
    </style>
    ```
    길이가 너무 길어지면 작성 및 수정이 불편 -> `<link>` 

- link
    ```html
    <!--단일 속성
        rel: 링크된 파일의 속성
        href: 파일 경로 -->
    <link rel = "stylesheet" href="style.css" />
    ```
    별도로 분리된 CSS파일을 링크 

- script
    
    _콘텐츠 방식_
    ```html
    <script>
        const hello = 'world';
        console.log(hello)
    </script>
    ```
    _링크 방식_
    ```html
    <!-- 콘텐츠를 가지지 않지만 단일태그는 아니기 때문에 셀프 클로징 X -->
    <script src = "script.js"></script>
    ```


### <span style="color:#D0E7D2">BODY</span>
> 사람 눈에 보이는 콘텐츠의 영역 

#### <span style="color:#F9B572">block - 블록 레벨 요소</span>
- 블록처럼 차곡차곡 쌓이며 화면 너비가 꽉 참
- 블록의 크기와 내/외부에 여백 지정 가능
- 페이지의 구조적 요소
- 인라인 요소 포함 가능 / 포함 될 수는 없음

- `<div>, <article>, <section>, ...`

#### <span style="color:#F9B572">inline - 인라인 레벨 요소</span>
- 블록 요소 내에 포함되는 요소 (블록 요소를 포함할 수 없음!)
- 문장, 단어같은 작은 부분에 사용되며, 한 줄에 나열됨
- 좌우 여백만 허용

- `<span>, <a>, <strong>, ... `

#### <span style="color:#F9B572">inline-block</span>
- 인라인 요소의 불편함을 해결하기 위한 요소
- 글자처럼 취급되지만 block태그의 성질을 가짐
- 크기와 내/외부 여백 지정 가능
- CSS로 성질을 바꾼 것이기 때문에 의미상 인라인 레벨 요소

활용 예시

```html
<body> 
    <span>인라인</span> 옆에 글자
    <div>블록</div>
</body>
```

```css
span{
    padding-left: 100px;
    padding-top: 100px; /* 적용 X */
}
div{
    padding-top: 50px;
    padding-bottom: 30px;
    padding-left: 100px;
}
```

![span](https://velog.velcdn.com/images/srlee056/post/a6ec119a-686c-4695-bf9e-d749c5f41161/image.png)

![div](https://velog.velcdn.com/images/srlee056/post/c8d47ee6-6120-413a-be14-3bee40235710/image.png)


### <span style="color:#D0E7D2">Layout</span>

#### <span style="color:#F9B572">Layout Tag?</span>

- html5부터 태그를 의미있게 사용하기 위해 `Semantic`태그를 사용하기 시작
- div만 사용하지 않고 적절한 태그를 사용해 웹 문서가 담은 정보와 구조를 의미 있게 전달
- semantic한 markup -> 검색 엔진의 순위에 가산점, 로딩속도 빨라짐 등 

#### <span style="color:#F9B572">Tags</span>

- div
    - 가장 흔하게 사용
    - 구역을 나누기 위한 태그
- header
    - 제목, 작성일 등 주요 정보를 담는 태그 
- footer
    - 페이지 바닥줄에 사용, 저작권 정보, 연락처 등 부차적 정보를 담는 태그
- main
    - 페이지의 가장 큰 부분으로 내용, 즉 주요 콘텐츠를 담는 태그
    - 한 페이지에 한번만 등장해야 함 (header, footer는 여러번 등장 가능)
-section
    - 콘텐츠의 구역을 나누는 태그
- article
    - 구역 안에서 작성된 정보를 전달하는 독립적인 문서의 역할을 하는 태그
- aside
    - 문서 내용에 부가적인 갖접정보를 전달하는 태그
    - 예) 쇼핑몰의 '오늘 본 상품', 블로그의 '위젯' 등

**레이아웃 분석**
_(배운 내용을 토대로 해본거라 정확하지 않을 수 있습니다)_
![](https://velog.velcdn.com/images/srlee056/post/6266f98d-a17e-4902-9633-2592af3db1ff/image.png)


### <span style="color:#D0E7D2">Contents</span>

1. 제목 태그 h1 ~ h6
    - 문서 구획 제목을 나타내는 태그
    - h1 태그는 페이지 내에 한번만 사용
    - 구획 순서(h1 ~ h6)는 지켜져야 함
2. 문단 태그 p
    - 문단을 담당하는 태그
    - 제목태그와 함께 사용되기도 하고 단독으로도 사용
    - 레이아웃태그처럼 사용 X
3. 서식 태그 b/strong, i/em, u, s/del
    - b/strong : 굵은 글씨로 변경. strong - 강조 의미 부여
    - i/em : 기울기 조절, em - 기울임과 내용에 강조 의미 부여
    - u : 밑줄을 넣고 주석을 가짐. 단순히 밑줄만 긋는 용도로는 사용 X
    - s/del : 취소선, del - 문서에서 제거된 텍스트를 나타냄
        ```html
        <p>
            안녕하세요.<br>
            <del>섦</del> <ins>서림</ins>입니다.
        </p>
        ```
        <p>
            안녕하세요.<br>
            <del>섦</del> <ins>서림</ins>입니다.
        </p>
4. 링크 이동 a
    - 클릭하면 페이지를 이동할 수 있는 링크 요소
    - href 속성으로 이동하려는 파일 / URL 작성
    - target 속성으로 새 창(_blank), 현재창(_self)등 타겟 지정 가능

        ```html
        <a href = "https://velog.io/@srlee056" target = "_blank"> 새 창에서 블로그 열기</a>
        ```
        <a href = "https://velog.io/@srlee056" target = "_blank"> 새 창에서 블로그 열기</a>

#### <span style="color:#F9B572">멀티미디어</span>
1. img
- 이미지를 추가하는 태그 
- src : 이미지의 경로
- alt : 로딩에 문제가 발생했을 때 대체 텍스트
- alt 태그에 적힌 메세지가 검색엔진에 키워드로 들어감
    ```html
    <img src = "" alt = "잘못된 로고">
    <img src = "https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" alt = "파이썬 로고">
    ```
    <img src = "" alt = "잘못된 로고">
    <img src = "https://cdn.icon-icons.com/icons2/2699/PNG/512/python_logo_icon_168886.png" alt = "파이썬 로고">
        
2. figure, figcaption
- 태그 안의 내용을 하나의 독립적인 콘텐츠로 분리하고 설명을 넣을 수 있는 태그
- 보통 이미지를 넣으며 인용문, 비디오/오디오 등 문서 흐름에 참조는 되지만 독립적으로 분리되어도 되는 내용을 담는다.
- 이미지 - 인라인 레벨 요소 / 피규어 - 블록 레벨 요소

    ```html
    <figure>
        <img src = "https://www.lgtwins.com/images/emblem/01.emblem.jpg" alt = "엘지 트윈스 로고">
        <figcaption> 엘지 트윈스 우승하자! </figcaption>
    </figure>
    ```
<figure>
    <figcaption> 엘지 트윈스 </figcaption>
    <img src = "https://www.lgtwins.com/images/emblem/01.emblem.jpg" alt = "엘지 트윈스 로고">
    <figcaption> 우승하자! </figcaption>
</figure>

3. video
- 문서 내에 영상을 첨부할 수 있는 태그
- src : 비디오 파일이나 온라인 링크 연결
- poster : 비디오 로드되기 전 포스터를 보여줄 수 있음
- <`source>` 태그로 여러 타입의 비디오 제공 
    ```html
    <video src = "/video.mp4"></video>
    <video poster = "/poster.png">
        <source src = "/video.mp4" type = "video/mp4">
        <source src = "/video.webm" type = "video/webm">
        비디오 태그가 실행되지 않을 때 보이는 글자
    </video>
    ```
4. audio
- 문서 내에 소리를 첨부할 수 있는 태그
- src : 음성 파일이나 온라인 링크 연결
- <`source>` 태그로 여러 타입의 오디오 제공
- controls : 재생/정지 버튼 등이 있는 컨트롤러

    ```html
    <audio src = "/audio.mp3" controls></audio>
    <audio controls>
        <source src = "/audio.mp3" type = "audio/mp3">
        <source src = "/audio.ogg" type = "audio/ogg">
        오디오 태그가 실행되지 않을 때 보이는 글자
    </audio>
    ```

    
5. svg
> Scalable Vector Graphics   
그래픽으로 만들어진 이미지
-  해상도의 영향을 받지 않아 확대/축소 자유로움
- 크기를 자주 바꾸어야 하는 작은 아이콘 등에 많이 사용
- 최근 기기들은 해상도가 다양하게 변화하고 있어, 아이콘 외에 로고 등 주요 이미지에도 사용
- `<img>`태그처럼 svg 파일을 불러올수도 있고, 태그를 그대로 사용할수도 있음
- 코드로 이루어져 있어서 스타일을 변경하거나, JS를 사용해 기능 추가도 가능

    ```html
    <img src="baseball.svg" alt="야구공 아이콘" />
    <!--또는 .svg 파일의 내용을 가져다가 쓸 수 있음-->

    <svg>
        파일 내용
    </svg>
    ```

- svg 파일 출력 결과

    <svg width="100px" height="100px" viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg">
        <path fill="#000000"
            d="M195.2 828.8a448 448 0 1 1 633.6-633.6 448 448 0 0 1-633.6 633.6zm45.248-45.248a384 384 0 1 0 543.104-543.104 384 384 0 0 0-543.104 543.104z" />
        <path fill="#000000"
            d="M497.472 96.896c22.784 4.672 44.416 9.472 64.896 14.528a256.128 256.128 0 0 0 350.208 350.208c5.056 20.48 9.856 42.112 14.528 64.896A320.128 320.128 0 0 1 497.472 96.896zM108.48 491.904a320.128 320.128 0 0 1 423.616 423.68c-23.04-3.648-44.992-7.424-65.728-11.52a256.128 256.128 0 0 0-346.496-346.432 1736.64 1736.64 0 0 1-11.392-65.728z" />
    </svg>


#### <span style="color:#F9B572">리스트</span>
1. `<ul>, <li>`

    > 순서가 없고 정렬되지 않은 목록
    
    ```html
    <ul>
        <li>list 1</li> <!--자식 요소로 li만 와야 함-->
        <li>list 2
            <ul> <!-- ul or ol tag-->
                <li>sub list 1</li>
                <li>sub list 2</li>
            </ul>
            <ol> 
                <li>sub list 3</li>
            </ol>
        </li>
    </ul>
    ```
    
    HTML 출력 결과
    <ul>
        <li>list 1</li> <!--자식 요소로 li만 와야 함-->
        <li>list 2
            <ul> <!-- ul or ol tag-->
                <li>sub list 1</li>
                <li>sub list 2</li>
            </ul>
            <ol> 
                <li>sub list 3</li>
            </ol>
        </li>
    </ul>

2. `<ol>`

    > 순서가 존재하며 정렬된 목록
    
    ```html
    <ol>
        <li>list 1</li> <!--자식 요소로 li만 와야 함-->
        <li>list 2
            <ol> <!-- ul or ol tag-->
                <li>sub list 1</li>
                <li>sub list 2</li>
            </ol>
            <ul> 
                <li>sub list 3</li>
            </ul>
        </li>
    </ol>
    ```
    HTML 출력 결과
    <ol>
        <li>list 1</li> <!--자식 요소로 li만 와야 함-->
        <li>list 2
            <ol> <!-- ul or ol tag-->
                <li>sub list 1</li>
                <li>sub list 2</li>
            </ol>
            <ul> 
                <li>sub list 3</li>
            </ul>
        </li>
    </ol>

3. `<dl>, <dt>, <dd>`
- 설명 목록 태그
- `<dt>`에 작성된 단어 혹은 내용의 설명을 `<dd>`에 작성
- 용어사전이나 `key-value`쌍의 목록을 나타낼 때 사용
- `<dt>` 여러개에 하나의 `<dd>` 가능 / `<dt>` 하나에 여러개 `<dd>` 가능
    ```html
    <dl>
        <dt>Chrome</dt>
        <dd>웹 브라우저<dd>
        <dd>구글에서 제작</dd>
        <br>
        <dt>Whale</dt>
        <dt>Microsoft Edge</dt>
        <dd>Web Browser</dd>
    </dl>
    ```

#### <span style="color:#F9B572">표</span>
- `<table>` : 표를 만드는 태그 
- `<tr>` 로 행을, `<td>`로 열을 나타냄
- `<th>` : 열 제목
- `<thead>` : 제목 그룹 태그, 한번만 사용
- `<tbody>` : 표의 본문 요소 그룹 태그, 역시 한번만 사용
- `<tfoot>` : 표 바닥글 요소 태그
- `<caption>` : 표 설명 태그
    ```html
    <table>
        <caption>sample table</caption>
        <thead>
            <tr>
                <th>col 1</th>
                <th>col 2</th>
            </tr>
        </thead>
        <tfoot> <!-- HTML4 에선 tbody 앞-->
            <tr>
                <td>footer</td>
                <td>footer</td>
            </tr>
        </tfoot>
        <tbody>
            <tr>
                <td> row1 col1 </td>
                <td> row1 col2 </td>
            </tr>
        </tbody>
        <tr>
            <td> row2 col1 </td>
            <td> row2 col2 </td>
        </tr>
    </table>
    ```
    <table>
        <caption>sample table</caption>
        <thead>
            <tr>
                <th>col 1</th>
                <th>col 2</th>
            </tr>
        </thead>
        <tfoot> <!-- HTML4 에선 tbody 앞-->
            <tr>
                <td>footer</td>
                <td>footer</td>
            </tr>
        </tfoot>
        <tbody>
            <tr>
                <td> row1 col1 </td>
                <td> row1 col2 </td>
            </tr>
        </tbody>
        <tr>
            <td> row2 col1 </td>
            <td> row2 col2 </td>
        </tr>
    </table>

#### <span style="color:#F9B572">iframe</span>
> 현재 문서 안에 다른 HTML 페이지를 삽입하는 역할

- src 속성에 원하는 HTML 문서나 URL 전달
- name 속성을 지정해 `<a>` target 속성을 사용해 iframe에서 문서나 URL 열리게 가능
- 불러온 외부 페이지의 영향을 받을 수 있다.
    ```html
    <iframe src = "/sample.html" frameborder = "0"></iframe>

    <iframe name="sample" frameborder="0"></iframe>
    <a href="https://example.com/" target="sample">example.com</a> 
    <!--target으로 설정된 iframe에서 외부 페이지가 열림-->
    ```

### <span style="color:#D0E7D2">양식 태그</span>

#### <span style="color:#F9B572">form</span>
- 정보를 제출하기 위한 태그
- 정보 입력을 위한 `<input>`, `<selectbox>`, `<textarea>`
- 정보 제출을 위한 `<button>`
- action 속성 : 정보 제출 시 페이지 이동
- method 속성 : 정보 제출 시 처리 방식 결정 
    - get : 검색 엔진 등에 사용
    - post : 로그인 등 보안이 중요한 방식에 사용

    ```html
    <form action="form-result.html" method="post">
        <input name="id" type="text">
        <input name="password" type="password">
        <select name="opt">
            <option>옵션1</option>
            <option>옵션2</option>
            <option>옵션3</option>
        </select>
        <button type="submit">전송</button>
    </form>
    ```

#### <span style="color:#F9B572">label</span>
- `<input>`, `<selectbox>`, `<textarea>` 의 설명을 작성하는 태그
- for 속성 : 연결하려는 태그의 id속성으로 지정하면 label클릭 시 연결된 태그가 선택됨
- label 태그 내부에 input 태그를 넣으면, for->id 연결을 직접 작성하지 않아도 같은 처리를 해 준다.
- **id 속성값은 절대 중복되면 안됨**

```html
<label for ="userid">아이디</label>
<input id = "userid" type = "text" name = "userid">

<label>
    비밀번호
    <input name = "password" type = "password">
</label>
```
#### <span style="color:#F9B572">input</span>
- 사용자에게 데이터를 입력 받을 수 있는 대화형 태그

- type 속성 : 받을 수 있는 input 유형을 정함 (기본:text)
- value 속성 : 기본 내용을 입력해둘 수 있음
- name : input 이름 지정
- 자주 사용되는 input type
    - checkbox
    - radio
    - file
    - button : input 태그를 버튼역할로 사용해야할 때 활용
    - hidden : 시각적으로 숨겨지지만 정보 제출 시 value속성에 입력된 값은 전송됨

    ```html
    <input type="text">
    <input type="text" name="input-name">
    <input type="text" value="입력 내용">
    ```
#### <span style="color:#F9B572">select</span>
- select, selectbox
- 옵션 메뉴를 제공하는 태그
- option 태그 : 선택할 옵션들을 정의
- value 속성 : 제출 시 선택한 옵션의 value값이 전송됨
- value 속성을 선언하지 않은 경우엔 option 태그 콘텐츠가 기본값
- placehoder 속성 사용 불가

```html
<select name = "selectbox">
    <option value="">choose</option>
    <option value = "opt1">opt1</option>
    <option value = "opt2">opt2</option>
    <option>opt3</option> 
</select>
```

#### <span style="color:#F9B572">textarea</span>
- 여러 줄을 입력할 수 있는 대화형 태그
- 콘텐츠에 내용을 입력할 경우 그 내용이 기본적으로 표시됨
- cols/rows 속성 : 기본 너비와 높이를 지정할 수 있으며, 이는 글자 크기 기준으로 정의됨
- 알아두면 좋은 속성
    - readonly : 수정 불가능한 읽기 전용
    - required : form 제출 시 "필수 입력 사항"으로 설정 (안내 문구나 행동 등은 브라우저가 자동으로 처리함)
    - placeholder : input, textarea에 부가설명을 입력해둘 수 있으며, select 에는 사용 불가
    - disabled : 비활성화되어 제출 시 값이 전송되지 않음


#### <span style="color:#F9B572">button</span>
- 클릭가능한 버튼
- form tag 내 어디서든 사용 가능
- type 속성 
    - submit(기본): 양식 제출
    - reset: 입력 양식 모두 초기화
- 콘텐츠에 블록레벨을 제외한 태그 입력 가능
- disabled 속성 가능


## 👀 <span style="color:#B0D9B1">CHECK</span>

###### *(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)*

- vscode -> cmd + , -> folder 검색 -> 계층 구조 관련 옵션 바꿀 수 있음
- markup : [설명 블로그](https://zdnet.co.kr/view/?no=00000010047880)
- 맥에서 크롬 개발자 도구 한번에 키는 단축키 : cmd + shift + c

## ❗ <span style="color:#B0D9B1">느낀 점</span>
HTML문법이나 태그 등에 대한건 이미 알고 있었지만 이론적으로 정리해본 적은 없었기에 이번 강의를 듣고 정리할 수 있어서 좋았다. 특히 contents 태그와 단일 태그의 존재는 알았지만 그 명칭은 몰랐기 때문에 머리속에서 모호하게 정의된 개념들이 이름을 가지고 명확해져서 좋았다.

강의 내용을 정리하고 직접 실행해보고 궁금한 부분은 따로 찾아보는 등 하다보니 주어진 코어타임보다 훨씬 더 오랜 시간을 붙잡고 있게 되었다. 중간중간 집중력도 떨어지고 체력적으로도 힘든 걸 느끼면서, 이론적인 공부보다는 실제 코드를 작성하는 게 더 재밌다는걸 새삼 체감했다. 그렇지만 이론적인 공부가 전부가 아니더라도 기본적인건 알아야 하니까 재미없다고 등한시 하지는 않아야겠다. :<

이번주는 드디어 웹 크롤링과 분석을 시작하는데, 내가 해봤던 것과 다른점은 무엇이고 어떤걸 배워나가게 될지 벌써부터 기대가 된다. 내일도 열심히 하는 하루를 보내야겠다. :>
