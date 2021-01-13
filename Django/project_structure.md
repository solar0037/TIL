# 프로젝트 구조

> - django-admin startproject
> - 문제점
> - 현재 사용하는 구조

- [첫번째 장고 앱 작성하기](https://docs.djangoproject.com/ko/3.1/intro/tutorial01/)
- [Django - 실전코딩](https://opentutorials.org/course/4201)

## django-admin startproject

- Django가 설치되었다면 django-admin 유틸리티 사용 가능

```Bash
$ django-admin startproject mysite
$ cd mysite
$ django-admin startapp hello
```

## 문제점

- virtualenv나 Pipenv, Poetry 등을 사용하려고 할 때 프로젝트 루트 디렉터리 안에 가상환경 생성 필요 (Pipfile, pyproject.toml 등도 있는 위치)
- mysite(루트 디렉터리) 에서 django-admin createproject mysite로 생성

```
mysite(createproject를 실행한 디렉터리)
├── .venv
├── mysite
│   ├── mysite
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   └── manage.py
└── hello
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```

- mysite 패키지 안에 mysite 패키지가 존재
- mysite/manage.py를 manage.py로 옮기고 중복을 없앨 순 없을까?
- (차라리 create-react-app처럼 global하게 설치해도 되는 패키지가 있으면 좋겠다.)

## 현재 사용하는 구조

```Bash
$ mkdir mysite
$ cd mysite  # 루트 디렉터리: mysite

$ python3 -m virtualenv .venv  # Pipenv, Poetry를 써도 무관
$ pip install django

$ django-admin startproject mysite  # mysite(루트)-mysite-mysite 구조
$ mv mysite temp  # mysite(루트)-mysite로 만들기 위해 삭제
$ mv temp/* .  # 원래 mysite(루트)-mysite에 있었던 것들 mysite(루트)로 옮기기
$ rmdir mysite  # mysite(루트)-mysite 구조 완성
```

- mysite의 상위 디렉터리에서 .venv를 만들고 mv .venv mysite/로 진행해도 무관
- 단, 그 상위 디렉터리에 .venv가 없어야 함

```
mysite(createproject를 실행한 디렉터리)
├── .venv
├── mysite
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── hello
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```
