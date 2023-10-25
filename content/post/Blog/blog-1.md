+++
author = "Seorim"
title = 'Blog 제작기 #1'
url = '/Blog/blog-1'
date = 2023-10-16
draft = false
categories = [
    "Blog", 
]
tags = [
    "Blog", "hugo", "Github Pages",
]
+++


[1일차 TIL](https://srlee056.github.io/p/day-1/)에서 이어지는 블로그 제작기 입니다. :>

[(아직 제작중인)블로그](https://srlee056.github.io/)를 구경하시려면 클릭하세요!





## 1. 로컬에서 [휴고 사이트 만들기](https://gohugo.io/getting-started/quick-start/)

```
hugo new site blog
cd blog
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
echo "theme = 'hugo-theme-stack'" >> hugo.toml
hugo server
```
    
나는 가이드에 적힌 theme과는 다른 [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)을 사용했다. 
가이드에는 새 post를 만들고 config 파일인 `hugo.toml`을 수정하는 부분도 나와있지만, 이후에 둘 다 다른 내용으로 덮어써서 넘어가도 되는 과정이었다. 
    
## 2. theme 커스터마이징하기

`themes/hugo-theme-stack/exampleSite/config.yaml` 파일 내용을 변환하여 제일 상위 폴더의 `./hugo.toml` 파일에 붙여넣고,   
(구글에 'yaml to toml'로 검색하면 많은 converter를 볼 수 있다.)

`themes/hugo-theme-stack/exampleSite/contetns/` 안의 파일들을 `./contetns` 에 붙여넣었다.

이 과정에는 [이 블로그 글](https://kzeoh.github.io)을 많이 참고했다. 정말 감사합니다👍🏻  

`hugo.toml` 파일에 오류가 좀 있는 것 같지만
1) 내가 아직 hugo configuration 관련해서 잘 모르고   
2) 오류가 있는데도 서버가 잘 돌아가고 빌드도 돼서
나중에 고치기로 했다. (🤔 이게 왜 되지?)

## 3. 배포하기

(사실 배포가 제일 쉬웠다)
github 사이트에서 `<username>.github.io` 이름의 레포지토리를 만들고
**로컬**에서 변경 내용을 모두 commit한 이후,
```
git remote add origin https://github.com/<username>/<username>.github.io.git
```
위 커맨드를 입력해 로컬과 원격 레포지토리를 연결한다.
<br>
```
git push -u origin master
```
그리고 push하면 레포지토리 관련은 끝. (branch 관련 건드린 게 있다면 push하기 전에 master로 옮길것.)
+) 요즘은 master가 아닌 main branch로 바뀌었더라. 
<br>

빌드와 배포는 git actions기능을 활용했는데, hugo 사이트에 [github hosting 가이드라인](https://gohugo.io/hosting-and-deployment/hosting-on-github/)이 잘 적혀있다.

이 링크의 내용을 요약하자면
- 레포지토리 settings/pages에서 source를 `GitHub Actions`로 변경
- 로컬에서 `.github/workflows/hugo.yaml` 파일 생성 후 내용 붙여넣기 (폴더도 직접 생성해줘야함)
- 변경사항을 commit, push한 이후 레포지토리의  `Actions` 탭에 들어가면 **빌드/배포 기능을 하는 Action**이 추가됨 
- Run Workflow 버튼을 누르면 빌드/배포가 자동으로 진행되며 내 github page에 테마가 적용된 사이트가 잘 올라와 있다.

[왜인지 모르겠지만 잘 돌아가는 블로그](https://srlee056.github.io/)

[블로그 github repo.](https://github.com/srlee056/srlee056.github.io)

--- 
### 느낀점
전체적으로 해결하지 못한 오류도 많고, 이해하지 못한 구조도 많지만 오늘은 배포에 성공한 것에 의의를 두려고 한다.🥲

### 다음엔?
- hugo로 포스트 올리기
- 오류 해결하기 (layout 인식 문제 등)
- theme custom 으로 링크 등 UI 추가해보기

