# Django 프로덕션

> - 구조
> - uWSGI
> - Nginx

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

## Nginx

- Nginx: 웹서버 역할

### 설치

```Bash
$ sudo apt install nginx
```

### 실행

```Bash
$ sudo /etc/init.d/nginx start
```

- start 외에도 stop, restart, reload, force-reload, status 등 가능

### 프로젝트 설정

- [uwsgi_params](https://github.com/nginx/nginx/blob/master/conf/uwsgi_params)를 프로젝트에 둬야 함
- 맨 위의 링크를 참조해 /etc/nginx/sites-available/mysite.conf 파일 생성, /etc/nginx/sites-enabled/에 심볼릭 링크 생성 (nginx가 참조할 수 있게)
