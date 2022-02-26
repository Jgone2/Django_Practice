# Django

# MVC패턴(MTV)

- **M**odel: 데이터베이스와 상호작용
- **V**iew: 사용자 인터페이스 담당
- **C**ontroller: 웹 서비스 내부의 논리 담당
  > 💡 MTV
  >
  > - **M**odel: 데이터베이스와 상호작용 담당
  > - **T**emplate: 사용자 인터페이스 담당(MVC의 View 담당)
  > - **V**iew: 웹 서비스 내부 동작의 논리 담당(MVC의 내부 동작의 논리 담당)

# 패키지 관리

```
pip install 패키지명                   # 패키지 설치
```

```
pip search 패키지명                    # 패키지 검색
```

```
pip install 패키지명==1.X.XX           # 특정 버전 지정하여 설치
```

```
pip uninstall 패키지명                 # 패키지 제거
```

```
pip freeze                           # 현재 설치된 패키지와 버전 목록
```

# 가상환경 셋팅

```
python -m venv 가상환경이름             # 가상환경 생성
```

```
source 가상환경이름/bin/activate        # 가상환경 실행(MacOS)
```

```
source 가상환경이름/Scripts/activate    # 가상환경 실행(Window)
```

```
deactivate                           # 가상환경 종료
```

# Django 기본 셋팅

```
pip install django                   # Django 설치(가상환경 실행 상태에서 진행)
```

```
django-admin startproject 프로젝트명    # 프로젝트 생성
```

### Django 서버 작동(manage.py기능 1)

```
python manage.py runserver           # Django 서버 작동
```

### App 생성하기(manage.py기능 2)

```
python manage.py startapp App명       # App 생성
```

1. App 생성 후 App이 생성되었다는 것을 알려줘야함.

- settings.py → Project에 생성된 App이 인식될 수 있게 settings.py에 apps 등록

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'helloworld.apps.HelloworldConfig', # ← 이렇게 등록(HelloworldConfig는 helloworld apps파일의 class명이다.)
    'helloworld', # App명만 작성해도 된다.
]
```

2. App폴더 내에 templates폴더를 생성

- templates → 사용자에게 인터페이스를 제공해주는 파일을 위치시킨다.(html, css 등)

### Database 초기화 및 변경사항 반영 → migrate(manage.py기능 3)

```
python manage.py migrate          # DB초기화 및 변경사항 반영
```

### 관리자 계정 만들기(manage.py기능 4)

```
python manage.py createsuperuser  # 관리자 계정 생성
```

- Email address는 공란처리 가능

# settings.py 파해치기

### BASE_DIR → Project 기본위치(root path) == manage.py의 위치

```python
BASE_DIR = Path(__file__).resolve().parent.parent
```

### SECRET_KEY → 평문의 암호화

```python
SECRET_KEY = '암호화키~~~'
```

### DEBUG → 서버 작동 방식

- True 설정 시 개발자를 위해 오류에 대한 정보 제공 O
- False 설정 시 오류 발생 시 오류에 대한 정보 제공 X(Not Found정도의 정보)

```python
DEBUG = True/False
```

### DATABASES → 사용할 DB위치와 이름

- 실제 데이터베이스와 연결해주는 Plug와 같은 역할

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

### 국제화 → 서버 기준 시간 및 언어 설정

```python
LANGUAGE_CODE = 'en-us'
```

```python
TIME_ZONE = 'UTC'
```

한국으로 변경 시

```python
LANGUAGE_CODE = 'ko-kr'
```

```python
TIME_ZONE = 'Asia/Seoul'
```

```python
USE_TZ = False        #  False로 설정해야 DB에 변경 된 TIME_ZONE이 반영 됨
```

## Django 시간설정

### datetime(Native방식)

- USE_TZ

```python
from datetime import datetime

datetime.now()
```

### django.utils

```python
from django.utils import timezone

timezone.now()          # 서버시간 기준

timezone.localtime()    # 현재 위치 기준
```

## STATIC_URL

- Static file들(CSS, JS, Img)의 위치

```python
STATIC_URL = '/static/'
```

<br /><br />

# Helloworld 프로젝트 만들기

### 프로젝트 생성

```
python startproject helloworld
```

### App 만들기

```
python manage.py startapp helloapp
```

### settings.py에 추가

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'helloapp.apps.HelloappConfig',
]
```

### templates 폴더 생성 후 html 파일 저장

- templates 폴더 생성 후 index.html 파일 생성 후 저장

### vies.py에 로직함수 생성

```python
def home(request):
    return render(request, 'index.html')
```

### 생성된 로직함수를 urls.py에 정의

```python
from django.contrib import admin
from django.urls import path
import helloapp.views                  # helloapp.view 불러오기

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', helloapp.views.home, name='home'),   # url경로, 함수위치, path명(작성하지 않아도 됨, 임의로 설정)
]
```

### 결과

![](./img/helloDjango.jpg)<br />
