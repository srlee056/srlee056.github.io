---
tags:
  - obsidian_blog
author: Seorim
slug: github-actions
categories:
  - GitHub Actions
title: GitHub Actions
date: 2024-05-10T06:15:05.330Z
lastmod: 2024-05-10T06:18:06.211Z
---
(github actions 관련 간단하게 발표한 내용입니다.)

# 1. About `GitHub Actions`

## GitHub Actions란 무엇인가?

* GitHub 플랫폼의 `(CI/CD)` 서비스

* 프로젝트의 다양한 이벤트에 반응하여 `자동화된 워크플로우`를 실행할 수 있음

  * push / pull request 등 여러 event에 대응해 test, build, deploy 등 작업을 자동화

  ### **CI/CD?**

  ![](/ob/github_blog/images/pasted_image_20240510150557.png)\
  https://www.redhat.com/ko/topics/devops/what-is-ci-cd

  * CI (Continuous Integration)
    * 변경 사항을 정기적으로 병합하고 자동으로 테스트하는 프로세스
    * 코드 품질을 유지하고 조기에 문제 발견
  * CD (Continuous Deployment)
    * 개발이 완료된 코드를 자동으로 배포하는 프로세스
    * 제품을 빠르게 배포하고 업데이트할 수 있음

## GitHub Actions 이점

1. **통합된 환경**
   1. GitHub 플랫폼 내에서 바로 CI/CD 파이프라인 구성 가능 → 별도의 CI/CD 도구를 사용할 필요가 없음
   2. 일관된 환경을 제공하며, GitHub의 다른 기능들과도 쉽게 통합됨
2. **유연성**
   1. YAML로 구성된 워크플로우를 사용하여 다양한 작업을 수행 가능
3. **확장성**
   1. 여러 가지 공용 액션과 자체적으로 정의한 커스텀 액션을 사용하여 파이프라인을 확장할 수 있음
   2. 재사용 가능한 코드를 작성한 후에, 다른 프로젝트에서 쉽게 활용할 수 있음

## GitHub Actions 왜 써야하죠?

* **사용 예시**

  * `프로젝트 CI/CD`
    * Airflow 사용 시 여러분이 작성한 DAG 코드를 서버에도 배포해줘야 함 (GitHub Actions 외에도 sync 할 수 있는 다른 방식도 있으니 찾아보세용)
    * main branch에 merge하기 전에, pull request를 통해 코드 테스트 가능
  * 자동화(개인적인 용도)
    * 블로그 deploy - GitHub Blog 발행기
      * <https://srlee056.github.io/Blog/blog/#hosting>
    * 코딩 테스트 복습을 위한 issue발행과 label 설정
      * <https://github.com/srlee056/algorithm-study/issues>
* **사용 이유**

  * CI/CD 경험
    * 저도 작년 말, GitHub pages로 블로그를 만들기 위해 접하게 되었고 당시엔 CI/CD가 뭔지도 잘 몰랐습니다. 개념적으로는 알았어도 실제로 어떻게 하는지 아무것도 몰랐어요

    * 그런데 잘 몰라도 매번 프로젝트에 사용하니까 점점 익숙해졌고, 구현할 수 있는 수준이 점점 높아졌습니다.\
      ![](/ob/github_blog/images/pasted_image_20240510150525.png)

    * 여러분이 접할 수 있는 가장 쉽고 간단한 CI/CD 구현 도구일거라고 생각합니다(아닐수도) 많이 써보세요!
  * script언어를 사용하는 경험
    * 대부분 ubuntu-latest 기반으로 코드를 작성할거라, script언어를 써보게 될 것
    * Data Engineer CLI 환경 많이 접해야 합니다. 익숙해지기 좋아요
    * 물론 실제 CLI와 다르지만 비슷하다는 의미입니다!

# **2. 기본 구조와 작동 방식**

```yaml
# .github/workflows/github-actions-workflow-test.yaml

name: workflow name

**on:**
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

**jobs**:
  build:
    **runs-on: ubuntu-latest**
    ...

    **steps:**
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v3
      with:
        python-version: ...
        
    - name: Install Dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        
```

### **워크플로우 workflow**

* 자동화된 절차 / **`.github/workflows`** 디렉토리에 있는 YAML 파일
* 하나 이상의 job을 포함하며, GitHub 이벤트 또는 수동으로 trigger 될 수 있음

### **작업 job**

* 같은 runner에서 실행되는 일련의 단계
* 각 작업은 워크플로우 파일 내의 **`runs-on`** 레이블로 지정된 인스턴스에서 실행됨

### **단계 step**

* job 내에서 명령을 실행하는 개별 작업으로, 스크립트 또는 Action을 사용할 수 있음
* 동일한 job 내에서 순차적으로 하나씩 실행됨

### **러너 runner**

* 워크플로우를 실행하는 서버 machine
* GitHub가 제공하는 runner (ubuntu-latest 등)을 사용하거나, 직접 machine을 구성하고 self-hosted-runner를 사용할 수도 있음

# **3. 사용 예시**

## **1) 간단한 `CI` - lint & test**

### 목표

* 코드 lint & test
  * linting?
    * 프로그래밍에서 소스 코드를 분석하여 오류, 버그, 스타일 오류, 의심스러운 구조와 같은 문제점을 찾아내는 과정

### 상세 기능

* `main branch`로 `push / pull request`가 일어날 경우, 코드를 lint (수정 및 여러가지 오류 체크) 및 test 하기

### workflow code

* code

  <https://github.com/EstateTrend-DevCourse/EstateTrend/blob/main/.github/workflows/django.yml>

  ```yaml
  name: Django CI

  **on:**
    push:
      branches: [ "main" ]
    pull_request:
      branches: [ "main" ]

  jobs:
    build:
      **runs-on: ubuntu-latest**
      strategy:
        max-parallel: 4
        matrix:
          python-version: [3.12]

      **steps:**
      - uses: actions/checkout@v3
      
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v3
        with:
          python-version: ${{ matrix.python-version }}
          
      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          
      - name: Run Tests
        run: |
          python manage.py test
  ```

## **2) `CD` 자동화**

### 목표

* `**Airflow DAG파일**`을 서버 환경으로 자동 배포

### 상세 기능

* `main branch로 push` event가 일어날 경우, DAG 코드를 개발 서버로 `sync`
* 서버를 self-hosted runner로 사용해 코드를 자동으로 서버에 받은 후, DAG 파일 위치로 copy하기
  * 서버환경과 연결하는 방법
    1. self-hosted-runner 사용
    2. GCP등 cloud sdk를 설치 후 VM에 script(git pull) 전송하여 실행
    3. 또 다른 방법이 있을수도?

### workflow 코드

* code

  <https://github.com/zizzic/airflow_repo/blob/main/.github/workflows/sync_dag_code.yml>

  ```yaml
  name: CI/CD Workflow

  on:
      push:
          branches: develop
      pull_request:
          branches: develop

  permissions:
      contents: read
      pull-requests: read

  jobs:
      path-filter:
          runs-on: ubuntu-latest
          outputs:
              dags-changed: ${{ steps.filter.outputs.dags }}
              docker-changed: ${{ steps.filter.outputs.docker }}
              plugin-created: ${{ steps.filter.outputs.plugin}}
          steps:
              - uses: actions/checkout@v4

              - name: Path Filter
                id: filter
                uses: dorny/paths-filter@v3
                with:
                    filters: |
                        dags:
                          - 'dags/**/*.py'
                          - 'tests/dags/**/*.py' 
                        docker:
                          - 'docker-compose.yaml'
                          - 'Dockerfile'
                        plugin:
                          - 'plugins/***'

      build:
          needs: path-filter
          if: (needs.path-filter.outputs.dags-changed == 'true'|| needs.path-filter.outputs.plugin-created == 'true') && github.event_name == 'pull_request'
          runs-on: ubuntu-latest
          steps:
              - uses: actions/checkout@v4

              - name: Setup Python
                uses: actions/setup-python@v5
                with:
                    python-version: "3.11"

              - name: Set Environment Variables
                run: |
                    echo "PYTHON_VERSION=3.11" >> $GITHUB_ENV
                    echo "AIRFLOW_VERSION=2.7.2" >> $GITHUB_ENV

              - name: Install dependencies
                run: |
                    CONSTRAINT_URL="<https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt>"
                    echo "Installing Apache Airflow version ${AIRFLOW_VERSION} with constraints from ${CONSTRAINT_URL}"
                    pip install "apache-airflow[amazon,mysql]==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
                    pip install pytest

              - name: Install Pylint
                run: pip install pylint
              - name: Run Pylint
                env:
                    PYTHONPATH: plugins
                run: pylint --output-format=colorized --disable=missing-docstring,invalid-name,W,C0301,C0411 $(find dags/ tests/dags/ plugins/ -name "*.py") || true
              - name: Test DAG integrity
                run: pytest tests/

      deploy:
          needs: path-filter
          if: github.event_name == 'push'
          runs-on: self-hosted
          steps:
              - name: Checkout code
                uses: actions/checkout@v4

              - name: Extract repository name
                run: echo "REPO_NAME=$(echo $GITHUB_REPOSITORY | cut -d '/' -f 2)" >> $GITHUB_ENV

              - name: Sync specific files
                env:
                    DEPLOY_DIRECTORY: /home/zizzicteam2/${{ env.REPO_NAME }}
                run: |
                    rsync -av --include='dags/***' --include='plugins/***' --include='docker-compose.yaml' --include='Dockerfile' --exclude='*' ./ $DEPLOY_DIRECTORY
              - name: Restart Docker Services
                if: ${{ needs.path-filter.outputs.docker-changed == 'true' || needs.path-filter.outputs.plugin-created == 'true'}}
                env:
                    DEPLOY_DIRECTORY: /home/zizzicteam2/${{ env.REPO_NAME }}
                run: |
                    cd $DEPLOY_DIRECTORY

                    docker compose down
                    docker compose up -d
  ```

## **3) Issue 및 project 등 연계**

### 목표

* 알고리즘 문제 풀이 코드 복습을 위한 `GitHub Issue 생성`

### 상세 기능

* 백준허브(<https://github.com/BaekjoonHub/BaekjoonHub>)가 내 repo에 코드를 올려주면(문제 설명 + 풀이 코드)
  1. 문제 이름을 title로 하는 Issue 생성
  2. 본문에 풀이 코드 링크
  3. 문제 분류를 label으로 추가해서 분류별 복습 가능
  4. 복습해서 다시 푼 경우 해당 Issue에 댓글로 추가하기 (중복 title Issue 새로 발행 X)

### workflow 코드

* code

  <https://github.com/srlee056/algorithm-study/blob/main/.github/workflows/commit_to_issue.yaml>

  ```yaml
  name: Convert Commit to Issue
  on:
      push:
          branches:
              - main
          paths:
              - '백준/**'
              - '프로그래머스/**'
      workflow_dispatch:

  jobs:
      create_issue:
          runs-on: ubuntu-latest
          steps:
              - name: Checkout repository
                uses: actions/checkout@v4
                with:
                    fetch-depth: 0 # 모든 커밋을 가져오기 위해
              - name: Create Issue for Commit
                env:
                    GITHUB_TOKEN: ${{ secrets.ACC_TKN }}
                run: |
                    git log ${{ github.event.before }}..${{ github.event.after }} --format='%H' | while read commit_hash; do
                      COMMIT_MESSAGE=$(git log -1 --format=%B $commit_hash)
                      COMMIT_TITLE=$(echo "$COMMIT_MESSAGE" | head -n 1)
                      COMMIT_BODY=$(echo "$COMMIT_MESSAGE" | awk '/^$/ {seen_blank=1; next} seen_blank {print}')
                      
                      echo "Commit Title: $COMMIT_TITLE"
                      echo "Commit Body: $COMMIT_BODY"
                      echo "Files changed in this commit: $COMMITTED_FILES"

                      problem_title=$(echo "$COMMIT_TITLE" | awk -F 'Title: |,' '{print $2}')

                      if [[ -z "$problem_title" ]]; then
                          echo "This commit is not by algorithm solving."
                      elif [[ $COMMIT_TITLE == [* ]]; then
                          echo "문제: $problem_title"
                          echo "Creating issue for commit starting with ["
                          
                          # remove double qoute using tr -d '"'
                          COMMITTED_FILES=$(git diff-tree --no-commit-id --name-only -r $commit_hash | tr -d '"')
                          echo $COMMITTED_FILES
                          
                          # Initializing an empty string for the links
                          COMMITTED_FILES_LINKS=""
                          
                          # Looping through each committed file to create a Markdown link
                          for file in $COMMITTED_FILES; do
                              decoded_file=$(printf "%b" "$file")
                              # Replace this with your GitHub repository details
                              file_url="<https://github.com/$GITHUB_REPOSITORY/blob/main/$decoded_file>"
                              COMMITTED_FILES_LINKS+="- [$decoded_file]($file_url)\\n"
                          done

                          for file in $COMMITTED_FILES; do
                              # Check if the file is a README.md in the specific path
                              decoded_file=$(printf "%b" "$file")
                              if [[ "$decoded_file" == 백준/*/*README.md ]]; then
                                  # Assuming $decoded_file holds the path to the README.md file
                                  README_CONTENT=$(gh api repos/:owner/:repo/contents/$decoded_file --jq '.content' | base64 --decode)
                                  echo $README_CONTENT
                                  # Now you can process README_CONTENT with awk or other tools as needed
                                  LABELS=$(echo $README_CONTENT | awk '{
                                      match($0, /### 분류 ([^#]+) ###/, arr);
                                      if (arr[1] != "") print arr[1];
                                  }')

                                  echo $LABELS
                                  # Convert labels into format suitable for `gh issue create` command
                                  
                                  GH_LABELS=$(echo $LABELS | tr ',' '\\n' | sed 's/ /-/g' | sed 's/^-//;s/-$//' | awk '{printf "--label \\"%s\\" ", $0}')

                                  echo $GH_LABELS
                                  break # Assuming only one README.md is relevant per commit
                              fi
                          done

                          # Use gh issue list and grep to find an issue by title
                          existing_issue=$(gh issue list --repo "$GITHUB_REPOSITORY" | grep -w "$problem_title" | awk '{print $1}')

                          if [[ ! -z "$existing_issue" ]]; then
                              echo "Issue already exists with title: $problem_title, Issue Number: $existing_issue"
                              # Create a comment on the existing issue
                              comment_body=$(echo -e "A new commit has been made that references this issue.\\n\\n**Committed Files:**\\n$COMMITTED_FILES_LINKS")
                              gh issue comment $existing_issue --body "$comment_body" --repo "$GITHUB_REPOSITORY"
                          else
                              # 먼저 현재 리포지토리의 모든 라벨을 가져옵니다.
                              existing_labels=$(gh label list --repo "$GITHUB_REPOSITORY" | cut -f1)

                              # 라벨이 존재하는지 확인하고, 없으면 생성합니다.
                              for label in $(echo $LABELS | tr ',' '\\n' | sed 's/ /-/g' | sed 's/^-//;s/-$//'); do
                                  # echo 명령을 사용하여 현재 처리 중인 라벨을 출력할 수 있습니다.
                                  echo "Processing label: $label"
                                  
                                  # 현재 라벨이 이미 존재하는 라벨 목록에 있는지 확인합니다.
                                  if ! grep -q "^$label$" <<< "$existing_labels"; then
                                      echo "Creating label: $label"
                                      gh label create "$label" --force --description "Automatically created label" --repo "$GITHUB_REPOSITORY"
                                  else
                                      echo "Label already exists: $label"
                                  fi
                              done

                              # No existing issue found, proceed to create a new issue
                              issue_body=$(echo -e "This issue was automatically generated from a commit.\\n\\n**Committed Files:**\\n$COMMITTED_FILES_LINKS")
                              issue_response=$(gh issue create --title "$problem_title" --body "$issue_body" --repo "$GITHUB_REPOSITORY" --assignee @me $GH_LABELS --label "auto-generated")
                              echo "New issue created: $issue_response"
                          fi
                      fi
                    done
  ```

## 4) Airflow CI/CD

같이 프로젝트 진행했던 `장태수` 님의 글입니다.

링크 참고해서 따라해보시면 좋을 것 같아서 가져왔어요!

[Airflow Dag CI/CD 구축기 (1)](https://velog.io/@pori/Airflow-Dag-CICD-%EA%B5%AC%EC%B6%95%EA%B8%B0-1)

# 4. 조언 및 Q\&A시간

## 조언

* 써보고 싶은 기술이 있으면 지금 써보세요. **지금 아니면 도전해볼 시간적 정신적 여유가 없습니다.**

* **`질문`은 적극적으로, 검열하지 않고 많이 하세요**

* **박ㅇㅇ** : 백엔드 파트가 있다면 기피하지 말 것

* **장태수** : \*\*\*\*코테 준비, 이력서 정리 (경험 정리), 자기소개서 써보기 등 **취업 준비하기**

* **김ㅇㅇ**

  1. Git 이나 AWS 기능쓸때 IDE 쓰지말고 쉘에서 직접 사용해서 익숙해지기.

     회사마다 다르겠지만 프로그램 구현에 맞춰서 사양을 짜놓는경우가 있어 IDE로 작업하면 서버가 못버티는 경우도 있더라구요.

  2. 작은프로젝트부터 로그 남기는 습관 가지기

     로그레벨을 낮게 설정해도 상관없으니 로그를 남긴다 는 습관을 소규모 프로젝트부터 잡는게 좋을것 같습니다. (로그가 없으면 유지보수가 너무 어렵습니다...)

  3. 백엔드 무조건 하기

     AWS GCP를 자유롭게 만져볼수 있는기회가 생각보다 몇 없는것같아요.

* **김ㅇㅇ**

  * **데이터 엔지니어링이 곧 빅데이터 엔지니어링이라고 착각하지 않도록 주의**
  * 이 둘을 혼동하는 순간 DE 분야에서 습득할 지식의 우선 순위를 잘못 숙지해 버릴 수 있음

* **조ㅇㅇ**

  * 데이터 엔지니어링을 하고싶어서 시작한게 아니라 막연한 생각만 갖고 오셨다면, 데엔이 어떤 일을 하는지 더 공부해보고, 고민해보세요.
  * 코어타임 외에도 더 시간 투자해서 공부하고, 데브코스 외에도 외적으로 개인 공부를 더 해라(자격증 공부도 가능하면 해보시면 좋을 것 같다.)
  * **자소서**는 진짜 쓰세요 계속 쓰세요
  * **알고리즘 문제** 하루에 한문제라도 풀어라

## Q\&A

프로젝트 관련 키워드(?)

* 2차 - 데이터
* 3차 - Airflow
* 4차 - 데이터 파이프라인

역할분담

* 데이터 소스별로 ETL / ELT 각자 구현

프로젝트 중에 번아웃이 온 경험?
