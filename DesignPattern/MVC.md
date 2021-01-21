# MVC

> - 디자인 패턴
> - MVC
>   - M(odel)
>   - V(iew)
>   - C(ontroller)

- [소프트웨어 디자인 패턴](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EB%94%94%EC%9E%90%EC%9D%B8_%ED%8C%A8%ED%84%B4)
- [MVC 디자인 패턴](https://opentutorials.org/course/697/3828)
- [[개발자 면접준비]#1. MVC패턴이란](https://m.blog.naver.com/jhc9639/220967034588)

## 디자인 패턴

- 소프트웨어 개발 도중 발생하는 특정 문맥의 문제에 대한 공통적인 해결책
- 순서나 방법도, 템플릿을 제공 (소프트웨어의 처리 방식 추상화?)

## MVC

<img src="./mvc.png" width=240px height=262px>

출처: [생활코딩](https://opentutorials.org/course/697/3828)

### Model

- 정보, 데이터. 데이터베이스에 접근하는 컴포넌트
- 예: Mongoose, django.db.models

```Python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=100)
    phone_number = models.CharField(max_length=20)
```

### View

- 사용자 인터페이스. 모델의 정보를 토대로 완성될 예정
- 예: JSP, Pug, Django HTML 템플릿

```HTML
<!DOCTYPE HTML>
<html>
  <head></head>
  <body>
    <p>Name: {{ name }}</p>
    <p>Phone Number: {{ phone_number }}</p>
  </body>
</html>
```

### Controller

- 모델과 뷰를 중재. URL에 따라 모델로부터 데이터를 받아 뷰에 반영. 클라이언트와 직접 통신
- 예: Django views 등 프로그래밍 언어로 로직 직접 구현

```Python
from django.shortcuts import render
from .models import Person

def profile(request):
    data = Person.objects.get(pk=1)
    return render(request, 'blog/profile.html', data=data)
```
