# 라우팅

> - 개요
> - urls.py

- [django.urls functions for use in URLconfs](https://docs.djangoproject.com/en/3.1/ref/urls/)

## 개요

- 요청 url에 맞게 함수, 자원 등으로 경로를 안내(?)해주기
- Django를 이용한 서버 사이드 라우팅

## urls.py

- 라우팅을 위한 파일

```Python
from django.contrib import admin
from django.urls import path
from myapp.views import index, hello

urlpatterns = [
    # admin
    path('admin/', admin.site.urls),

    # blog
    path('', index),
    path('hello/<name>', hello),
]
```

- `django.urls.path()`를 통해 정의
- `django.urls.re_path()`를 통해 정규 표현식도 사용 가능
- `urlpatterns = []`에 라우팅 경로 알려주기
- `<name>`과 같이 감싸서 kwargs로 전달 가능
- 이름이 겹치는 경우? -> 네임스페이스 활용! (`from myapp import views as myapp_views`)
