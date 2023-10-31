+++
author = "Seorim"
title =  "Day 12"
date = 2023-10-31T14:18:08+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "Django", "Template", "View"
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

### Django의 디자인 패턴

#### <o1>MVC</o1>

> 구성 요소를 Model-View-Controller로 구분하는 디자인 패턴  
> 각 구성 요소가 다른 요소에게 영향을 미치지 않아야 함

Model

-   데이터를 갖고, 처리하는 로직을 가짐

View

-   요청에 대한 결과를 화면에 보여주는 역할. 유저와의 인터페이스

Controller

-   Model과 View를 이어주는 역할

#### <o1>MTV</o1>

> Model-Template-View
> MVC 패턴에 대응되는 `Django의 디자인 패턴`

Model

-   DB에 저장되는 데이터를 의미하며, 클래스 하나가 table 하나에 대응됨

Template

-   MVC 패턴의 View 역할으로, 유저에게 보여지는 화면을 의미

View

-   MVC 패턴의 Controller 역할으로, 요청에 따른 로직을 수행하며 결과를 Template으로 렌더링하며 응답하거나 데이터를 주고받음

+URLConf

-   URL pattern을 정의하고 URL과 View를 매핑함

### View

-   `polls/views.py`

-   import

```python
from .models import *
from django.http import HttpResponse, HttpResponseRedirect #, Http404
from django.shortcuts import render, get_object_or_404
from django.urls import reverse
from django.db.models import F
```

-   index page

```python
# 웹 서버는 요청에 응답하는 역할이기 때문에, 요청을 받고 응답을 반환하는 기능이 필요
def index(request):
    # '-pub_date' : pub_date 기준 "역순"으로
    latest_question_list = Question.objects.order_by("-pub_date")[:5]

    # context = {"{Template 변수(?) 이름}": 값}
    context = {"questions": latest_question_list}
    # request에 대해 context를 넘겨 'polls/index.html' template를 렌더링한다.
    return render(request, "polls/index.html", context)
```

-   detail page

```python
def detail(request, question_id):
    """ Http404를 활용하는 방법
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    """
    # Django에서 제공하는 404 Shortcut
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/detail.html", {"question": question})
```

-   vote page

```python
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST["choice"])
    except (KeyError, Choice.DoesNotExist):
        return render(
            request,
            "polls/detail.html",
            {
                "question": question,
                "error_message": f"선택이 없습니다. id={request.POST['choice']}",
            },
        )
    else:
        # 동시에 두 서버에서 같은 선택을 한 경우, 값을 제대로 변경하기 위해
        # db에서 값을 읽어 계산 진행
        selected_choice.votes = F("votes") + 1  # F : django db
        selected_choice.save()
    # vote template를 render하지 않고, result page로 redirect
    return HttpResponseRedirect(reverse("polls:result", args=(question_id,)))
```

-   result page

```python
def result(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, "polls/result.html", {"question": question})
```

### Template

-   `polls/templates/polls/` 폴더 내에 `.html` 파일으로 작성

-   `index.html`

```html
{% if questions %}
<ul>
	{% for question in questions %}
	<li>
		<a href="{% url 'polls:detail' question.id %}">
			{{question.question_text}}
		</a>
	</li>
	{% endfor %}
</ul>
{% else %}
<p>no questions</p>
{% endif %} {% comment %} template에서는 인덱싱에 . 사용 questions[0] : X
questions.0 : O appname 설정하지 않은 경우 : url 'detail' appname 설정한 경우 :
url 'polls:detail' '{app_name : name}' in urls.py {% endcomment %}
```

-   `detail.html` : Using `form` to get input from User

```html
<form action={% url 'polls:vote' question.id %} method='post'>
  {# django에서 자동으로 토큰 생성 #}
  {% csrf_token %}
  <h1>{{ question.question_text }}</h1>
  {% if error_message %}
    <p>
      <strong>{{ error_message }}</strong>
    </p>
  {% endif %}
  {% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{choice.id}}">
    <lable for="choice{{ forloop.counter }}">
      {{ choice.choice_text }}
    </lable>
    <br>
  {% endfor %}
  <input type="submit" value="Vote">
</form>
```

-   `result.html`

```html
<h1>{{ question.question_text }}</h1>
<br />
{% for choice in question.choice_set.all %}
<lable> {{ choice.choice_text }} -- {{ choice.votes }} </lable>
<br />
{% endfor %}
```

### URL Config

-   `polls/urls.py`

```python
from django.urls import path
from . import views

app_name = "polls" # app_name이 설정된 경우, polls:detail, polls:vote 등 app_name을 필수적으로 포함하여 호출해야 함

urlpatterns = [
    path("", views.index, name="index"),
    # <int:question_id>/ : 사용자가 입력하는 주소를 통해 question_id가 전달됨
    path("<int:question_id>/", views.detail, name="detail"),
    path("<int:question_id>/vote/", views.vote, name="vote"),
    path("<int:question_id>/result/", views.result, name="result"),
]
```

### Customizing Admin page

-   같이 개발하는 또는 데이터를 활용하는 내부 User를 위해 Admin Page를 적절하게 조작하는 방법에 대해 배운다.

-   `polls/models.py`

```python
class Question(models.Model):
    # verbose_name 으로 넘겨준 문자열이 admin page에서 제목으로 표시된다.
    question_text = models.CharField(max_length=200, verbose_name="질문")
    pub_date = models.DateTimeField(auto_now_add=True, verbose_name="생성일")

    # admin page display를 표시하는 방식을 변경한다.
    @admin.display(boolean=True, description="최근생성(하루기준)")
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
    ...
```

-   `polls/admin.py`

```python
from django.contrib import admin
from .models import *

# StackedInline, TabularInline
# Question admin page에서 연결된 Choice를 Inline 형태로 출력하기 위한 class
class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 2  # 추가 등록 칸

# Question admin page customizing
class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        ("생성일", {"fields": ["pub_date"], "classes": ["collapse"]}),  # collapse : 숨김처리
        ("질문 섹션", {"fields": ["question_text"]}),
    ]
    list_display = ["question_text", "pub_date", "was_published_recently"]
    readonly_fields = ["pub_date"] # cannot modify.
    inlines = [ChoiceInline]
    list_filter = ["pub_date"]  # filter records by pub_date
    search_fields = ["question_text", "choice__choice_text"]

# register Question to admin site with customized page "QuestionAdmin"
admin.site.register(Question, QuestionAdmin)
```

## <g2>👀 CHECK</g2>

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

-   vscode django template 포매팅

    -   [참고글](https://velog.io/@junsikchoi/VSCode%EC%97%90%EC%84%9C-Django-%ED%85%9C%ED%94%8C%EB%A6%BF-%EC%98%A4%ED%86%A0-%ED%8F%AC%EB%A7%A4%ED%8C%85%ED%95%98%EA%B8%B0)

-   django template 주석 처리
    ```template
    {% comment %}
    주석 내용
    {% endcomment %}
    {# 한 줄 주석 #}
    ```

## <g2>❗ 느낀 점</g2>

오늘은 제일 맘에 들지 않는 TIL 인 것 같다. 일단 정리도 제대로 안됐고, 용어도 제대로 기억을 못했다. 강의를 다시 보면서 강사님이 사용하는 용어등을 체크하고 작성했으면 좋았겠지만 시간이 빠듯하기도 하고 집중도 잘 안돼서 제대로 못 적었다.
내일 중에 시간이 나면 오늘 강의를 다시 들으면서 수정을 좀 하고싶다. 내일 못하더라도 주말에는 꼭 할 생각이다.

어제 오늘 배운 내용들은 아무래도 프로젝트를 하면서 직접 더 찾아보고 사용해봐야 제대로 이해할 것 같다. View와 Template, URL Config가 하는 역할은 이해도 가고 활용해서 다른 페이지를 만들어보는것도 할 수 있다. 그런데 각각의 필드나 활용 가능한 메소드 등 깊게 들어가기 시작하면 말이나 글로 풀어서 설명하는게 너무 어렵다. 그리고 학교를 다니면서 영어로 된 term을 더 자주 접해서 그런가.. TIL을 적을 때에도 한국어로 설명하기 애매하고 어려운 용어들은 무조건 영어로 적는 습관이 있다.

요즘 강의들은 실습 위주라 그런지 TIL을 코드와 주석 위주로만 적는 경향이 있는데, 다른 방식으로 적어보고 싶다. 예전에 찾아둔 TIL 참고 링크와, 부트캠프를 참가자들 블로그를 보면서 잘 쓴 TIL들을 참고해봐야겠다.
