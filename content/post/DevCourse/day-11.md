+++
author = "Seorim"
title =  "Day 11"
date = 2023-10-30T12:39:04+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL","Django", "Model", "QuerySet"
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

# <g1>TIL - Django</g1>

## <g2>📋 공부 내용</g2>

### Django 설치 및 개발환경 설정

#### Python virtual environment (venv)

> 서로 다른 환경에서 개발되는 여러 프로젝트를 진행할 때, 환경을 체크하고 변경하는 번거로움 및 프로젝트간의 충돌이 발생한다.  
> 이와 같은 문제점을 방지하고 프로젝트들을 각 목적에 맞게 효율적으로 관리하기 위해, 프로젝트마다 가상환경을 만들어 사용하는것을 권장한다.

-   가상환경 프로젝트 생성

```shell
$ python -m venv {project-name}
```

-   활성화

```shell
$ source project-name/bin/activate
```

-   비활성화

```shell
$ deactivate
```

#### Django

_가상환경이 `활성화 된 상태`에서 설치_

```shell
$ pip install django
```

### Django 활용

> Django : 파이썬으로 제작된 웹 프레임워크

#### 프로젝트

프로젝트 생성 및 서버 실행

```shell
$ django-admin startproject mysite
$ python3 manage.py runserver
```

_(python으로 입력하는 경우 실행이 되지 않을 때가 많아 python3 사용)_

#### App & Url

-   새로운 앱 `polls`와 `index`, `some_url` 페이지를 생성하고 url 연결

```shell
$ python3 manage.py startapp polls
```

-   `polls/views.py`

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world.")

def some_url(request):
    return HttpResponse("Some ulr을 구현해 봤습니다.")
```

-   `mysite/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("polls/", include('polls.urls'))
]
```

-   `polls/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('',views.index, name='index')
    path('some_url',views.some_url)
]
```

#### RDB **R**elational **D**ata**b**ase

> 관계형 데이터베이스
> 데이터를 `행`과 `열`로 이루어진 `테이블`의 형태로 구성하고, `테이블 간의 관계`를 정의하는 데이터베이스

-   테이블 table
    > 행과 열로 구성되어 있는 `데이터의 집합`
-   열 column

    > 테이블의 `필드(field)`

    -   primary key
        : 테이블의 각 행(row)을 고유하게 식별할 수 있는 열(column)
    -   forien key
        : 다른 테이블의 primary key를 참조하는 열(column)으로, 두 테이블 간의 관계를 설정할 수 있음

-   행 row
    > 테이블에 저장된 `데이터 레코드(Record)`

#### Model

> Django에서 RDB와 연동을 담당하는 클래스로, 각 모델은 데이터베이스의 테이블(Table)에 해당하며 필드(Field)를 가지고 있다.

-   모델 필드 (model field)
    -   다양한 타입의 필드와 옵션이 존재한다. 자세한 설명은 아래 링크를 참고.
    -   [Django Model Fields](https://docs.djangoproject.com/en/4.2/ref/models/fields/)
-   `mysite/settings.py`

```python
...
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls.apps.PollsConfig', #새로 생성한 app을 settings.py에 추가
]
...
```

-   `polls/models.py`

```python
from django.db import models

# polls app에 모델 두 개를 만들고 각각의 field를 설정

# 따로 primary key(=pk)를 설정하지 않을 경우, 모델이 생성될 때 주어지는 id와 같은 값을 갖게 됨
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

# foreign key로 Question 모델의 pk를 참조
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

-   migration 파일 생성

```shell
$ python3 manage.py makemigrations polls
```

-   migration으로 실행될 SQL 문장 살펴보기

```shell
$ python3 manage.py sqlmigrate polls 0001
```

-   migration 실행하기

```shell
$ python3 manage.py migrate
```

-   migration 롤백

```shell
#0001 버전으로 롤백
$ python3 manage.py migrate polls 0001
```

#### Admin

> 관리자 계정을 생성하고 웹 브라우저를 통해 사이트에 접속하여  
> User, App 그리고 Model 등을 생성, 수정 및 삭제할 수 있다.

-   Admin 생성

```shell
$ python manage.py createsuperuser
```

-   `polls/admin.py`

```python
from django.contrib import admin
from .models import * # polls/models.py 에서 정의한 모델을 가져온다.

#admin site에 생성한 모델 등록
admin.site.register(Question)
admin.site.register(Choice)
```

-   `polls/models.py`

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    # field 출력을 위한 함수
    def __str__(self):
        return f'제목: {self.question_text}, 날짜: {self.pub_date}'

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

#### Model Method

-   `polls/modes.py`

```python
from django.utils import timezone
import datetime

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

    def __str__(self):
        if self.was_published_recently():
            new_badge = 'NEW!!!'
        else:
            new_badge = ''
        return f'{new_badge} 제목: {self.question_text}, 날짜: {self.pub_date}'
```

### QuerySet

> List of model objects  
> 데이터베이스로부터 데이터를 `읽고, 필터링하거나 정렬`할 수 있음

(_Django Shell에서 진행_)

-   Django Shell 실행

```shell
$ python manage.py shell
```

#### Read model object

```python
# 모델 가져오기
>>> from polls.models import *

#모든 Question,Choice 오브젝트 출력
>>> Question.objects.all()
>>> Choice.objects.all()

#첫번째 Choice 오브젝트를 가져와 필드 출력
>>> choice = Choice.objects.all()[0]
>>> choice.id
>>> choice.choice_text
>>> choice.votes

#첫번째 Choice와 연결된 Question을 가져와 필드 출력
>>> q = choice.question
>>> choice.question.pub_date
>>> choice.question.id

#해당 Question과 연결되어 있는 모든 Choice 가져오기
>>> q.choice_set.all()

```

#### CRUD model objects (Records)

(_CRUD : Create, Read, Update & Delete_)

```python
>>> from polls.models import *

# 새로운 Question 오브젝트를 내용과 함께 생성하고 'q1'이라는 변수에 저장
>>> q1 = Question(question_text = "커피 vs 녹차")

# 'q1' 생성시각을 설정
>>> from django.utils import timezone
>>> q1.pub_date = timezone.now()

# 'q1'을 데이터베이스에 저장하기
>>> q1.save()

# 같은 과정을 통해 q2를 생성하고 데이터베이스에 저장
>>> q2 = Question(question_text = "abc")
>>> q2.pub_date = timezone.now()
>>> q2.save()

# .create method로 Choice object를 생성하여 q2와 연결
>>> q2.choice_set.create(choice_text = "b")
# Choice object를 생성할 때 question field에 q2값을 넣어 연결
>>> choice_c = Choice(choice_text='c', question=q3)
#새로운 Choice 오브젝트를 데이터베이스에 저장하기
>>> choice_c.save()
# q2와 연결된 Choice 오브젝트들을 확인
>>> q2.choice_set.all()
<QuerySet [<Choice: b>, <Choice: c>]>

# 가장 마지막으로 생성된 오브젝트 가져와 삭제하기
>>> q = Question.objects.last()
>>> q.delete()
```

#### Filter model objects

-   QuerySet method `get()`

```python
# 조건에 맞는 오브젝트 필터링
>>> Question.objects.get(id=1)
>>> Question.objects.get(question_text__startswith='휴가를')
# 오브젝트 여러개를 반환하는 필터링을 진행할 경우 에러 발생
>>> Question.objects.get(pub_date__year=2023)
# get() returned more than one Question
```

-   QuerySet method `filter() & exclude()`

```python
# 여러 오브젝트를 반환하는 필터링 가능
>>> Question.objects.filter(pub_date__year=2023)
<QuerySet [<Question: 제목: 휴가를 어디서 보내고 싶으세요?, 날짜: 2023-02-05 18:52:59+00:00>, <Question: 제목: 가장 좋아하는 디저트는?, 날짜: 2023-02-05 18:53:27+00:00>, ...]>
>>> Question.objects.filter(pub_date__year=2023).count()
2

# 관계기반의 필터링
# foreign key로 지정된 Model의 필드값으로 필터링 가능
>>> Choice.objects.filter(question__question_text__startswith='휴가')
>>> Choice.objects.exclude(question__question_text__startswith='휴가')
```

-   SQL query

```python
# example 1
>>> print(Question.objects.filter(pub_date__year=2023).query)
SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."pub_date" BETWEEN 2023-01-01 00:00:00 AND 2023-12-31 23:59:59.999999

# example 2
# pk == primary key = id
>>> q = Question.objects.get(pk=1)
>>> print(q.choice_set.all().query)
SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 1
```

-   Field lookups

QuerySet methods `filter()`, `exclude()` & `get()` 에서 `keyword arguments`로 지정됨

```python
# startswith
>>> Question.objects.filter(question_text__startswith='휴가를')

# contains
>>> Question.objects.filter(question_text__contains='휴가')

# gt, gte, lt, lte
>>> Choice.objects.filter(votes__gt=0)

# 정규표현식
>>> print(Question.objects.filter(question_text__regex=r'^휴가.*어디').query)
SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date", "polls_question"."owner_id" FROM "polls_question" WHERE "polls_question"."question_text" REGEXP ^휴가.*어디
```

#### 날짜와 시간 date & time information

-   datetime
    -   date : 날짜를 다루기 위한 클래스(Year, Month, Day)
    -   time : 시간을 다루기 위한 클래스(Hour, Minute, Second, Microsecond)
    -   datetime : 날짜와 시간을 다루기 위한 클래스 (date 클래스와 time 클래스의 속성을 모두 가짐)
    -   timedelta : 시간 간격을 다루기 위한 클래스

```python
from datetime import datetime, timedelta

now = datetime.now()

# one hour / one day / one week / one year later
hour_after = now + timedelta(hours=1)
day_after = now + timedelta(days=1)
week_after = now + timedelta(days=7) # weeks=1
year_after = now + timedelta(days=365)
```

-   timezone
    -   UTC 정보를 포함하며, 기본값은 `UTC+0`

```python
from django.utils import timezone

now = timezone.now()
# datetime.datetime(2023, 10, 28, 6, 25, 23, tzinfo=datetime.timezone.utc)
```

## 👀 <g2>CHECK</g2>

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

-   vscode 세팅

    -   [vscode 파이썬 가상환경 세팅 참고 블로그](https://blog.devwon.site/python/2021/08/01/Vscode-venv-python-interpreter/)
    -   vscode setting 에서 가상환경(venv) 루트 지정 가능
    -   `f1` -> interpreter -> 가상환경의 interpreter 지정하여 django 개발 환경으로 설정

-   문자열 보간 String Interpolation

    -   `%`, `.format`, `f`

    ```python
    def __str__(self):
        return f"제목: {self.question_text}, 날짜: {self.pub_date}"
    ```

    -   [참고 블로그](https://velog.io/@cmin95/Python-string-interpolation)

-   에러노트

    1. 사용중인 Port

    ```
    Error: That port is already in use.
    ```

    -   Ctrl + z 로 종료해서 서버가 제대로 종료되지 않아 생기는 에러 (Ctrl + c 로 종료해야 함)
    -   해당 포트에서 돌아가고 있는 서버 확인
        ```
        lsof -i:{port number}
        ```
    -   해당 프로세스 kill
        ```
        kill -9 {port number}
        ```

-   MVC in Django : MTV
    -   MVC(Model-View-Controller) : 웹 개발 디자인 패턴 중 하나
    -   MTV(Model-Template-View) : MVC에 대응되는 Django의 디자인 패턴
    -   [참고 블로그1](https://tibetsandfox.tistory.com/16)
    -   [참고 블로그2](https://velog.io/@hidaehyunlee/Django-MTV-%ED%8C%A8%ED%84%B4)

## ❗ <g2>느낀 점</g2>

오늘은 Django 프레임워크의 소개 및 기본 기능과 구조에 대해 배웠다. 간단한 개념 및 소개였기 때문에 어렵다기 보다는 정리하고 기억해야 할 내용이 많아 공부하고 정리하는 데 오랜 시간이 들었다.

오늘 공부한 기본적인 개념을 잘 정리하고싶어서 강의를 들은 시간만큼 TIL을 적었다. 100% 만족하진 못해도 내가 정리한 내용과 공식 document를 통해 따로 찾아본 내용들을 기반으로 적어서 뿌듯하다. 오늘 정리하는게 정말 힘들었지만 이 경험 덕분에 이후 강의들을 잘 이해할 수 있을거라고 본다.

모르는 내용을 찾아보고 적용하는덴 자신이 있지만 남들이 이해할 수 있게 정리하는덴 정말 자신이 없다. (말로 설명하는것도 비슷한 편이다.) 그래도 TIL을 적기 전보다는 훨씬 나아졌다는 걸 스스로도 체감중이다. 정리하는데 들인 시간들이 거름이 되어 나중에는 '잘 이해되는 글', '잘 설명한 글' 이라는 좋은 결과를 수확할 수 있기를!!
