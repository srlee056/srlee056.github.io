+++
author = "Seorim"
title =  "Day 13"
date = 2023-11-01T12:58:26+09:00

categories = [
    "DevCourse",
]
tags = [
    "TIL", "DRF", "Serializer", "APIView"
]
+++

# TIL - Django REST framework

## 📋 공부 내용

### Django REST framework

>

#### install

#### `settings.py`

```python
...
INSTALLED_APPS = [
...
    "rest_framework",
]
```

### Serializer

#### Serializer

```python
from rest_framework import serializers
from polls.models import Question
class QuestionSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    question_text = serializers.CharField(max_length=200)
    pub_date = serializers.DateTimeField(read_only=True)

    def create(self, validated_data):
        return Question.objects.create(**validated_data)

    def update(self, instance, validated_data):
        instance.question_text = (
            validated_data.get("question_text", instance.question_text)
            + "[시리얼라이저에서 업데이트]"
        )
        # id, pub_date는 Readonly라 업데이트 하지 않음
        instance.save()
        return instance

```

#### ModelSerializer

```python
from rest_framework import serializers
from polls.models import Question
class QuestionSerializer(serializers.ModelSerializer):
    class Meta:
        model = Question
        fields = ["id", "question_text", "pub_date"]

```

### Views

CRUD in HTTPS

#### Method 기반

-   URL Conf

    -   `mysite/ursl.py`

    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path("admin/", admin.site.urls),
        path("polls/", include("polls.urls")),
        path("rest/", include("polls_api.urls")),
    ]
    ```

    -   `polls_api/urls.py`

    ```python
    from django.urls import path
    from .views import *

    urlpatterns = [
        path("question/", question_list, name="question-list"),
        path("question/<int:id>/", question_detail, name="question-detail"),
    ]
    ```

-   GET

```python
from polls.models import Question
from polls_api.serializers import QuestionSerializer
from rest_framework.response import Response
from rest_framework.decorators import api_view
# 데코레이터 아무것도 넣지 않으면 'GET'
@api_view()
def question_list(request):
    questions = Question.objects.all()
    # 여러개의 인스턴스를 줄 때 Many 옵션
    serializer = QuestionSerializer(questions, many=True)
    return Response(serializer.data)
```

-   POST

```python
...
from rest_framework import status
@api_view(["GET", "POST"])
def question_list(request):
...
    if request.method == "POST":
        serializer = QuestionSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

-   PUT & DELETE

```python
...
from django.shortcuts import get_object_or_404
@api_view(["GET", "PUT", "DELETE"])
def question_detail(request, id):
    question = get_object_or_404(Question, pk=id)

    if request.method == "GET":
        serializer = QuestionSerializer(question)
        return Response(serializer.data)

    if request.method == "PUT":
        serializer = QuestionSerializer(question, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_200_OK)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    if request.method == "DELETE":
        question.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

#### Class 기반

-   APIView

```python
...
from rest_framework.views import APIView

class QuestionList(APIView):
    def get(self, request):
        questions = Question.objects.all()
        serializer = QuestionSerializer(questions, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = QuestionSerializer(data=request.data)
        print(type(request.data))
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

class QuestionDetail(APIView):
    def get(self, request, id):
        question = get_object_or_404(Question, pk=id)
        serializer = QuestionSerializer(question)
        return Response(serializer.data)

    def put(self, request, id):
        question = get_object_or_404(Question, pk=id)
        serializer = QuestionSerializer(question, data=request.data)

        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_200_OK)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, id):
        question = get_object_or_404(Question, pk=id)
        question.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

-   URL Conf : `polls_api/urls.py`

```python
...
urlpatterns = [
    path("question/", QuestionList.as_view(), name="question-list"),
    path("question/<int:id>/", QuestionDetail.as_view(), name="question-detail"),
]
```

-   Mixin

```python
class QuestionList(
    mixins.ListModelMixin, mixins.CreateModelMixin, generics.GenericAPIView
):
    queryset = Question.objects.all()
    serializer_class = QuestionSerializer
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)


class QuestionDetail(
    mixins.RetrieveModelMixin,
    mixins.UpdateModelMixin,
    mixins.DestroyModelMixin,
    generics.GenericAPIView,
):
    queryset = Question.objects.all()
    serializer_class = QuestionSerializer
    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)
    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)
    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

-   URL Conf : `polls_api/urls.py`

```python
...
urlpatterns = [
    path("question/", QuestionList.as_view(), name="question-list"),
    path("question/<int:pk>/", QuestionDetail.as_view(), name="question-detail"),
]
```

-   Generic API View

```python
class QuestionList(generics.ListCreateAPIView):
    queryset = Question.objects.all()
    serializer_class = QuestionSerializer

class QuestionDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Question.objects.all()
    serializer_class = QuestionSerializer
```

-   URL Conf : `polls_api/urls.py`

```python
...
urlpatterns = [
    path("question/", QuestionList.as_view(), name="question-list"),
    path("question/<int:pk>/", QuestionDetail.as_view(), name="question-detail"),
]
```

## 👀 CHECK

_<span style = "font-size:15px">(어렵거나 새롭게 알게 된 것 등 다시 확인할 것들)</span>_

-   django rest framework

    -   강의에는 따로 나오지 않는데,
    -   설치 진행하고 <https://www.django-rest-framework.org/>
    -   mysite/settings.py -> INSTALLED_APPS에 "rest_framework", 추가해줘야 함
        -   [스택오버플로우](https://stackoverflow.com/questions/38366861/django-templatedoesnotexist-rest-framework-api-html)

-   DRF ReturnDict, OrderedDict
-   python venv 설정 공유 (협업)
    -   [참고 글](https://takeheed.tistory.com/13)
    -   [참고 글2](https://codingrepo.tistory.com/56)

## ❗ 느낀 점
