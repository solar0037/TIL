# Django 프로덕션

> - 구조
> - uWSGI

- [Setting up Django and your web server with uWSGI and nginx](https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)

## 구조

- Client - Nginx - uWSGI - Django
- Django 웹서버는 프로덕션에서 직접 사용할 수 없음 (성능, 보안 등. 순수 개발용이다. ~~친구가 그대로 쓰던데~~)

## uWSGI

- WSGI: Web Server Gateway Interface - 웹서버와 Django 애플리케이션을 소통해줄 중간자(브로커?) 역할
- uWSGI: WSGI 구현체

### 설치

```Bash
$ pip install uwsgi
```

### 실행

```Bash
$ uwsgi --http :8000 --wsgi-file test.py
```

- test.py 파일을 --wsgi-file 플래그를 통해 서빙

```Bash
$ uwsgi --http :8000 --module mysite.wsgi
```

- Django 애플리케이션을 --module 플래그를 통해 서빙 (mysite/wsgi.py -> mysite.wsgi)
