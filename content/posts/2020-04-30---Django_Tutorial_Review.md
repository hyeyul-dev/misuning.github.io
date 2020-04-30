---
title: Django Tutorial Review
date: "2020-04-30T15:30:32.169Z"
template: "post"
draft: false
slug: "django_tutorial_review"
category: "Typography"
tags:
  - "Django"
  - "Django_Tutorial"
  - "Web Development"
description: "파이썬 기반으로 작성된 오픈소스 웹 어플리케이션 프레임워크입니다. 프레임워크란 간단히 설명하자면 뼈대, 골조라고도 하며 프로그램을 개발하는 데에 있어서 사용되는 기본 개념 구조입니다.
즉, 파이썬 프로그래밍 언어를 기반으로 한 동적인 웹을 작성하는 데에 있어 장고라는 기본 개념 구조 요소를 이용하여 개발하게 되는 것입니다."
socialImage: "https://media.vlpt.us/images/hanrimjo/post/99fab083-1722-46d2-a46a-0e98b6c2eb62/image.png"
---

## Django란?

>파이썬 기반으로 작성된 오픈소스 웹 어플리케이션 프레임워크입니다.
프레임워크란 간단히 설명하자면 뼈대, 골조라고도 하며 프로그램을 개발하는 데에 있어서 사용되는 기본 개념 구조입니다.
즉, 파이썬 프로그래밍 언어를 기반으로 한 동적인 웹을 작성하는 데에 있어 장고라는 기본 개념 구조 요소를 이용하여 개발하게 되는 것입니다.




## 프로젝트 생성
```js
$ django-admin startproject mysite
```
이 명령은 현재 디렉토리에서 mysite 라는 디렉토리를 생성한다. 

```js
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```
 ```mysite/ ```디렉토리 내부에는 프로젝트를 위한 실제 python 패키지들이 저장된다. 
 ```mysite/__init__.py```: Python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 용도의 단순한 빈 파일이다.
```mysite/settings.py```: 현재 Django 프로젝트의 환경 및 구성을 저장합니다. Django settings에서 환경 설정이 어떻게 동작하는지 확인할 수 있다.
```mysite/urls.py```: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 "목차" 라고 할 수 있다.
```mysite/asgi.py```: 현재 프로젝트를 서비스하기 위한 ASGI 호환 웹 서버의 진입점이다.
```mysite/wsgi.py```: 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점이다.

```js
python manage.py runserver
```
이 코드를 작성하면 서버를 구동하여 브라우저에서 프로젝트가 제대로 작동하는지 알 수 있다.
포트 번호를 안 넣으면 default로 8000번으로 실행되며,
```js
python manage.py runserver 8080
```
뒤에 이렇게 포트번호를 입력해 줄 수도 있다.

## 설문조사 앱 만들기

이제 django 프로젝트로 설문조사 앱을 만들어 볼 것이다.

앱을 생성하기 위해 manage.py가 존재하는 디렉토리에서 다음 명령어를 입력해보자.
```js
python manage.py startapp polls
```
polls라는 디렉토리가 생성되고, 아래와 같다.
```js
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

## 뷰 작성

```polls/view.py```를 열어 다음 코드를 입력한다.
```js
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
다음 단계는, 최상위 URLconf에서 polls.urls 모듈을 바라보게 설정해야한다. mysite/urls.py파일을 열고 다음과 같이 코드를 입력한다.
```js
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```
> include()함수는 다른 urlconf들을 참조할 수 있도록 도와준다. 

자 이제 http://127.0.0.1:8000/polls 로 들어가면
-> studyPy/urls.py 전체 URLconf를 통하고
-> polls/urls.py polls URLconf를 통하여
-> polls/views.py index 함수가 실행되어

"Hello World with Django" 가 뜨게 된다!

## 데이터베이스 설치

이제, ```mysite/setting.py``` 파일을 열어보자.
이 파일은 django 설정을 모듈 변수로 표현한 일반적인 python 모듈이다. 기본적으로는 **SQLite**을 사용하도록 구성되어 있다.

### 지역시간 설정
데이터베이스를 수정하기 전에 ```setting.py```의 TIME_ZONE이라는 변수를 한국에 맞게 변경해야한다.
데이터를 저장할 때 생성 날짜와 시간을 자동으로 정할 경우, 한국 시간으로 저장하기 위해서이다.
```mysite/mysite```에서 ```vi setting.py```을 치고 들어가서 

```TIME_ZONE : 'Asia/Seoul'```로 바꿔준다. 원래는 ```UTC```로 되어있다.

### 테이블 생성

TIME_ZONE 윗 편을 보면 INSTALLED_APPS 라는 리스트가 있다. 이 리스트는 장고에서 활성화된 모든 장고 APP들을 나타낸다

django.contrib.admin - 관리용 사이트<br>
django.contrib.auth - 인증 시스템<br>
django.contrib.contenttypes - 컨텐츠 타입을 위한 프레임워크<br>
django.contrib.sessions - 세션 프레임워크<br>
django.contrib.messages - 메세징 프레임워크<br>
django.contrib.staticfiles - 정적 파일을 관리하는 프레임워크<br>

```$ ./manage.py migrate``` 또는 ```python manage.py migrate```
이 명령어를 실행하면 위의 INSTALLED_APPS 리스트 안의 App 에서 제공되는 데이터베이스 **migrations**와 settings.py의 **데이터베이스 설정**에 따라 테이블이 생성된다.

## 모델 생성

```polls/models.py```
```js

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

Question과 Choice라는 2가지 모델을 만들었다. 
Question은 question과 Publication date를 담고 있고, Choice는 CharField와 IntegerField라는 2개의 필드를 담고있다.

## 모델 활성화

앱을 현재의 프로젝트에 포함시키기 위해서는, 앱의 구성 클래스에 대한 참조를 INSTALLED_APPS 설정에 추가해야 한다. PollsConfig 클래스는 polls/apps.py 파일 내에 존재한다. 따라서, 점으로 구분된 경로는 'polls.apps.PollsConfig'가 된다. 이 점으로 구분된 경로를, mysite/settings.py 파일을 편집하여 **INSTALLED_APPS **설정에 추가하면 된다.

```js
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

#### migration

이제 장고가 polls 의 존재를 알게 되었다. 자 이제 아까 얘기한 **migration** 을 사용할 차례이다❗️
```js
python manage.py makemigrations polls
```

**makemigrations** 을 실행시킴으로서, 당신이 모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 **migration**으로 저장시키고 싶다는 것을 Django에게 알려준다.

#### migrate

migrate 명령은 아직 적용되지 않은 migration을 모두 수집해 이를 실행하며(Django는 django_migrations 테이블을 두어 마이그레이션 적용 여부를 추적한다.) 이 과정을 통해 모델에서의 변경 사항들과 데이터베이스의 스키마의 동기화가 이루어진다.

> **migration**은 매우 기능이 강력하여, 마치 프로젝트를 개발할 때처럼 데이터베이스나 테이블에 손대지 않고도 모델의 반복적인 변경을 가능하게 해준다. 동작 중인 데이터베이스를 자료 손실 없이 업그레이드 하는 데 최적화 되어 있다. 

✣ 지금은 모델의 변경을 만드는 세 단계의 지침을 기억하세요❗️

> 1. (models.py 에서) 모델을 변경합니다.
2. ```python manage.py makemigrations```을 통해 이 변경사항에 대한 마이그레이션을 만드세요.
3. ```python manage.py migrate```  명령을 통해 변경사항을 데이터베이스에 적용하세요.

## API 가지고 놀기

```js
python manage.py shell
```

python 쉘에서 django가 접근할 수 있는 Python 모듈 경로를 그대로 사용할 수 있기 때문에 django에서 동작하는 모든 명령을 쉘에서 그대로 시험해 볼 수 있다.

```js
* 쉘

>>> from polls.models import Choice, Question
>>> from django.utils import timezone
>>> q = Question(question_text="What`s new?",pub_date=timezone.localtime())
# q 라는 Question db를 생성함. qustion_text 필드에는 "what's new?" 라는 질문이 들어가고 pub_date 라는 필드에는 django.utils 모듈에 존재하는 timezone 함수를 사용해 현재 시간을 넣어줌
>>> q.save() 
# save함수로 db를 저장해준다
>>> q.id() 
# q라는 db에 id라는 필드를 생성해준적이 없는데 1이 반환된다. 자동으로 생성되는 고유 id값이라고 생각된다.
>>> q.question_text 
# q 라는 db의 question_text 필드의 값을 반환해준다. (what`s new? 가 반환됨)

>>> q.pub_date
# q 라는 db의 pub_date 필드의 값을 반환해준다.

>>> Question.objects.all()
# Question 객체로 정의된 db를 모두 반환한다.
>>> Question.objects.filter(id = 1)
# id가 1값을 가지고있는 Question db를 검색한다

>>> Question.objects.filter(question_text__startswith="what")
# question_text가 'what' 으로 시작되는 것을 검색한다 (__startswith 옵션은 시작되는 문자열이나 숫자를 검색)

>>> Question.objects.filter(question_text__endswith="what")
# question_text가 'what' 으로 끝나는 것을 검색한다 (__endswith 옵션은 끝나는 문자열이나 숫자를 검색)

>>> Question.objects.get(pub_date__year=current_year)
>>> Question.objects.get(id=2)
# Question이라는 model을 가진 db중 고유 필드인 id가 2인 db를 가져옴.
# 고유 필드인 id가 2인 db가 없으니 err
>>> q = Question.objects.get(pk=1)
# q라는 db에 Question이라는 model을 가진 db중 고유 필드인 pk 값이 1인 db를 가져와 q에 저장함.

>>> q.choice_set.all()
# q라는 db(Question모델)와 관계형 데이터베이스(ForeignKey)인 Choice 모델로 생성된 Choice db 목록을 반환한다. 아직 Choice 모델로 db를 생성한적이 없으니 아무것도 반환되지 않는다.

>>> q.choice_set.create(choice_text='The sky',votes=0)
# q라는 db(Question모델)와 관계형 데이터베이스인 Choice 모델로 Choice db를 생성해주고 q라는 db에 넣고 셋팅까지 해준다.
>>> q.choice_set.count()
# q 라는 db(Question모델)와 관계형 데이터베이스인 Choice 모델 db의 갯수를 반환한다.

>>> Choice.objects.filter(question__pub_date__year=current_year)
# Choice 모델 db와 관계형 데이터베이스인 Question 모델 db의 pub_date 필드가 current_year과 같은 Choice 모델 db를 검색한다.

>>> c = q.choice_set.filter(choice_text='The sky')
# q라는 Question 모델 db에 Question 모델과 관계형 데이터베이스인 Choice 모델의 db의 choice_text 필드가 The sky 인 Choice 모델 db를 c라는 변수에 담는다.

>>> c.delete()
# c라는 db를 삭제한다.
# c라는 변수에 따로 Choice 모델 db 를 담았어도 q 라는 Question 모델 db의 Choice 모델 db가 사라진다.
```

## 관리자 생성

```js
python manage.py createsuperuser

Username: misun  #원하는 username 입력하고 엔터 누르면

Email address: misun9283@gamil.com   #원하는 이메일주소 입력.

#그리고 암호입력

```

http://127.0.0.1:8000/admin
여기 들어가서 유저네임, 패스워드 입력한다.

### 관리 사이트에서 poll app을 변경하기

```polls/admin.py``` 들어가서 admin사이트에서 question을 등록한다.

```js
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```
![](https://images.velog.io/images/misun9283/post/40bc49f6-5c3c-4260-9da3-69872944ed90/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-27%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.19.18.png)

이렇게 Question이 추가가 되어있다.


## view 추가

이제ㅣ polls/views.py에 뷰를 추가해보자.

```js
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)
	 # polls/urls.py에 매핑되있는대로 url뒤에 int형 question_id가 오면 해당 HttpResponse를 반환한다.
     
def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```
다음, path()호출을 추가하여 이러한 새로운 뷰을 polls.urls 모듈로 연결한다.
```js
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path('', views.index, name='index'),
    # ex: /polls/5/
    path('<int:question_id>/', views.detail, name='detail'),
    # ex: /polls/5/results/
    path('<int:question_id>/results/', views.results, name='results'),
    # ex: /polls/5/vote/
    path('<int:question_id>/vote/', views.vote, name='vote'),
    #url뒤에 int형으로 question_id가 오면 views의 detail함수를 실행한다.
]
```

## template 파일

> **템플릿(Template) ?**
HTML은 파이썬 코드를 담을 수 없다. 템플릿은 파이썬 코드를 HTML에 넣을 수 있게 해준다.

먼저 polls 폴더에 templates 라는 폴더를 만들고, 그안에 polls 라는 폴더를 또 만든다. 그안에 index.html을 생성하고 다음 코드를 넣어주자.

```js
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```

> {% %} 안에 파이썬 코드를 넣고, {{ }} 안에 전달될 값을 넣으면 된다. 그리고 기본적으로 for문이나 if조건문을 사용시엔 무조건 파이썬문장이 끝났다는 태그, 즉, {%endfor%}나 {%endif%}로 닫아줘야한다.

자 이제 View에서 저 index 템플릿을 출력하는 코드를 작성하자

```polls/views.py```

```js
# from django.http import HttpResponse
# 이제 HttpResponse 말고 shortcut인 render을 써보자
from django.shortcuts import render
from .models import Question


def index(request):
  # question 인스턴스들을 불러오는 코드
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    return render(request, 'polls/index.html', {'latest_question_list': latest_question_list})
```
이러면 이제 템플릿을 통해 HTML이 랜더링 된다.🙃

> **render()** 함수는 request 객체를 첫번째 인수로 받고, 템플릿 이름을 두번째 인수로 받으며, context 사전형 객체를 세전째 선택적(optional) 인수로 받는다. 인수로 지정된 context로 표현된 템플릿 HttpResponse 객체가 반환된다.

## 404 에러 일으키기

이제, 질문의 상세 뷰에 태클을 걸어보자.
### Http404 

```js
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```
뷰는 요청된 질문의 id가 없을 경우 http404 예외를 발생시킨다.

### 지름길: get_object_or_404()

```js
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

## 템플릿에서 하드코딩된 URL 제거

방금전 만든 파일에서 이런 코드가 있다

```polls/templates/polls/index.html```

```js
<a href="/polls/{{ question.id }}">{{ question.question_text }}</a>
```
이렇게 하드코딩된 URL은 수많은 템플릿을 사용할 때 변경하기 어렵다.
**{% url %}** template 태그를 사용하여 url 설정에 정의된 특정한 URL 경로들의 의존성을 제거할 수 있다.
```js
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
 ```
 ```polls/urls/py``` 에서 확인할 수 있다.
 ```js
  path('<int:question_id>/', views.detail, name='detail'),
 ```
이제 url은 urls.py에서만 바꾸면된다.


    
## URL의 이름공간 정하기

튜토리얼의 프로젝트는 polls라는 앱 하나만 가지고 진행한다. 실제 Django 프로젝트는 앱이 몇개라도 올 수 있다. Django는 이 앱들의 URL을 어떻게 구별해 낼까요? 예를 들어, polls 앱은 detail이라는 뷰를 가지고 있고, 동일한 프로젝트에 블로그를 위한 앱이 있을 수도 있습니다. Django가 {% url %} 템플릿태그를 사용할 때, 어떤 앱의 뷰에서 URL을 생성할지 알 수 있을까?

정답은 ```URLconf에 이름공간(namespace)을 추가```하는 것이다. ```polls/urls.py``` 파일에 ```app_name```을 추가하여 어플리케이션의 이름공간을 설정할 수 있다.

```polls/urls.py```
```js
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('<int:question_id>/results/', views.results, name="results"),
    path('<int:question_id>/vote/', views.vote, name="vote"),
    path('<int:question_id>/', views.detail, name="detail"),
    path('', views.index, name='index'),
]
```

이제 polls/index.html 템플릿의 기존내용을 이름공간으로 나눠진 상세 view를 가리키도록 변경하자.

```js
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```

## form 

일단 detail.html 파일에 간단한 POST Form 을 넣었다

```polls/templates/polls/detail.html```
```js
<h1>{{ question.question_text }}</h1>

{% if error_message %}
  <p><strong>{{ error_message }}</strong></p>
{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
  {% csrf_token %}
  {% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label>
  {% endfor %}

  <input type="submit" value="Vote">
</form>
 ```
이제 POST 메서드로 들어온 폼 데이터를 처리해보자.
```{% csrf_token %} : 해커의 CSRF 공격을 막아주는 코드이다```

> **CSRF (Cross Site Request Forgery) 공격이란?**
사용자가 피싱사이트에 접근했을 때, 사용자의 권한을 가지고 지정된 사이트의 정보를 마음대로 조작하는 해킹방법이다.

csrf 공격을 막는 방법으론 대표적으로 2가지가 있다.
> <li> 
 Referrer 검증
 </li>
 <li>
 Security Token 사용 (A.K.A CSRF Token)
  </li>

```polls/views.py```
```js
from django.shortcuts import render, get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse

from .models import Question, Choice

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice"
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```
> 1. 위의 vote 뷰는 URL로 들어온 question_id 를 가진 Question을 찾는다.
2. POST로 들어온 id 와 같은 기본키를 가진 Choice를 찾아 그 Question에 단다.
3. 만약 KeyError(초이스를 선택안했거나)나 Choice를 못찾으면 다시 Detail 페이지로 ErrorMessage와 함께 랜더링 시킨다.
4. 에러가 없다면 찾은 Choice의 votes 값을 1올리고 Result 페이지로 리다이렉트 한다.

*reverse 함수는 app_name처럼 하드코딩을 제거해주는 역할을 하고 '/polls/3/results/' 를 반환해준다 (3은 임의의 id)

form datas를 받을 때는 ```request.POST['choice']``` 이렇게 받을 수 있다.

## generic view

제너릭 뷰는 만들어진 템플릿 클래스를 상속받아서 최소한의 설정으로만 템플릿을 랜더링 시켜주는 뷰이다.

index 뷰와 detail 뷰를 제너릭 뷰로 변경해보자

```polls/urls.py```
```js
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('<int:pk>/', views.DetailView.as_view(), name='detail')
]
```
먼저 urls.py에서 제너릭 뷰를 쓴다고 선언해두자
IndexView 와 DetailView는 views.py에서 만들 것이다.
```js
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from django.views import generic

from .models import Choice, Question


class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'
```

<li>ListView :객체의 리스트를 display
<li> DetailView : 객체 하나의 detail 페이지를 display
  

IndexView는 generic.ListView를 상속 받았고
DetailView는 generic.DetailView를 상속받았다.





