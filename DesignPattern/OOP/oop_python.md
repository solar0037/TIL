# 객체지향형 프로그래밍

> - 객체지향형 프로그래밍
>   - OOP의 특징
> - class, ``__init__``, ``__del__``
> - 접근 제한자
>   - Name Mangling
>   - getter, setter
>   - @property
> - classmethod, staticmethod
>   - classmethod
>   - staticmethod
> - 추상클래스

- [객체 지향 프로그래밍(OOP) 개념](https://victorydntmd.tistory.com/117)
- [Python 밑줄 문자와 던더 (name mangling)](https://blog.naver.com/reisei11/221749496623)
- [44. class 정리 - 정적메소드 @classmethod와 @staticmethod의 정리](https://wikidocs.net/16074)
- [Python @property 사용하기](https://medium.com/@hckcksrl/python-property-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-89eb0f0e2e56)

## 객체지향형 프로그래밍

- 컴퓨터 프로그램을 명렁어의 목록으로 보는 것에서 더 나아가 각각의 독립된 단위(객체)로 파악하는 프로그래밍 패러다임
- 객체: 어떠한 대상
- 클래스: 같은 속성을 가진 데이터의 종류(속성), 메서드(method)와 속성(attribute)로 이루어짐

### OOP의 특징

- 추상화(abstraction): 공통점 찾기, 관련이 없는 속성을 배제하여 필요한 부분만 표현하기
- 캡슐화(encapsulation): 정보 감추기, 알약은 쓰기 때문에 감춘다.
- 상속(inheritance): 부모로부터 속성 물려받기, 코드 중복 없애기 위해
- 다형성(polymorphism): 형태가 같은데 다른 기능, 오버라이딩(속성 재정의), 오버로딩(매개변수 유형, 개수 다르게 하기)

## class, ``__init__``, ``__del__``

- class: 클래스 정의
- ``__init__(self, *args)``: 생성자(객체가 만들어질 때 호출)
- ``__del__(self)``: 소멸자(객체가 소멸할 때 호출)

~~~Python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def say(self):
        print(f"My name is {self.name}, and I am {self.age} years old.")
    
    def __del__(self):
        print(f"{self.name} dies at age {self.age}.")

p = Person("James", 18)
p.say()  # My name is James, and I am 18 years old.
del p  # James dies at age 18.
~~~

## 접근 제한자

- 외부에서 객체의 정보에 접근하는 것을 신중하게
- 파이썬에는 protected, private 제한자가 없기 때문에 강제되지는 않음
- _method 형태로 쓰면 내부용으로 쓰일 것이라는 표시 (protected)
- __method 형태로 쓰면 (강한) 접근하지 말라는 표시 (private), name mangling


### Name Mangling

- private 비슷한 기능
- 서브클래스에서 변수가 재정의되는 것 막기
- __method() -> _ClassName__method로 바뀜
- 서브클래스에서 접근하려고 하면 에러 (_ClassName__method로 override할 수는 있음)

~~~Python
class Lock:
    def __init__(self):
        self.__key = "locked"

print(dir(Lock()))
# ['_Lock__key', ...]
print(Lock().__key)
# AttributeError  # _Lock__key로 name mangling되었기 때문
~~~

~~~Python
class NewLock(Lock):
    def __init__(self):
        self.__key = "overriden"

print(dir(Lock()))
# ['_AnotherLock__key', ...]
print(AnotherLock().__key)
# AttributeError  # __key는 애초에 name mangling되어 존재하지도 않음
~~~

### getter, setter

- 외부에서 직접 속성을 편집하는 것을 막기
- _name이나 __name 형태로 정의하고 getter, setter로 대체

~~~Python
class Account:
    def __init__(self, name):
        self._name = name
    
    def get_name(self):
        return self._name
    
    def set_name(self, name):
        self._name = name

acc = Account("John Doe")
print(acc.get_name())  # John Doe
acc.set_name("John Smith")
~~~

### @property

- getter, setter를 설정하는 것은 번거롭고 코드가 더러워짐
- class property(fget=None, fset=None, fdel=None, doc=None) - get, set 함수를 설정해 property 속성을 반환
- instance_name.property 형태로 접근해도 속성을 직접 편집하지 않고 getter, setter가 호출되게 할 수 있음
- getter가 None인 경우 - AttributeError: 'ClassName' object has no attribute 'name' - 속성 이름이 _name이면 애초에 객체에 name 속성이 없음!
- setter가 None인 경우 - AttributeError: can't set attribute

property() 사용

```Python
class Person:
    def __init__(self):
        self._name = None
    
    def get_name(self):
        return self._name

    def set_name(self, value):
        self._name = value
    
    def del_name(self, value):
        del self._name
    
    name = property(get_name, set_name, del_name, "The person's name.")

p = Person()
p.name = "John Doe"  # setter
print(p.name)  # getter
del p.name  # deleter
```

- property 객체를 통해 get, set, delete를 외부에서 더 자연스럽게 사용할 수 있음

@property 사용

```Python
class Person:
    def __init__(self):
        self._name = None
    
    @property
    def name(self):
        """The person's name."""
        return self._name
    
    @name.setter
    def name(self, value):
        self._name = value
    
    @name.deleter
    def name(self):
        del self._name

p = Person()
p.name = "John Doe"  # setter
print(p.name)  # getter
del p.name  # deleter
```

- 데코레이터를 사용해 위와 똑같은 코드 구현
- 데코레이터를 사용한 함수의 이름은 property의 이름과 같아야 한다.
- 의문점: @property를 사용해 getter를 구현했는데 왜 @name.getter가 자동완성에 잡히지? 더 알아봐야겠다.

## classmethod, staticmethod

- 정적 메서드
- classmethod, staticmethod 모두 객체에서도 접근이 됨

### classmethod
- def my_class_method(cls, *args) 형태로 첫 인자로 cls 사용
- 상속받을 때 cls()는 cls인자를 활용하여 cls의 클래스속성 반환

### staticmethod
- def my_static_method(*args) 형태로 추가되는 인자 없음
- 상속받을 때 ClassName() 호출하면 부모클래스 클래스속성 반환

```Python
class Utils:
    @classmethod
    def my_class_method(cls):
        print(cls.__name__)

    @staticmethod
    def my_static_method(a, b):
        return a + b


if __name__ == '__main__':
    Utils.my_class_method()  # Utils
    print(Utils.my_static_method(1, 3))  # 4

```

## 추상클래스

- abc 모듈 사용
- 미구현(추상적) 메서드 한 개 이상인 클래스
- 자식클래스에서 추상 메서드 구현하지 않으면 에러
- 추상클래스로 객체 생성하려고 하면 에러

```Python
from abc import ABC, ABCMeta, abstractmethod
# class AbstractClass(metaclass=ABCMeta):  # metaclass 직접 정의
class AbstractClass(ABC):  # ABC 상속 (metaclass는 여전히 ABCMeta)
    @abstractmethod
    def my_abstract_method(self, *args):
        pass

class RealClass(AbstractClass):
    def my_abstract_method(self, *args):
        print("Implemented")

if __name__ == '__main__':
    obj = RealClass()
    obj.my_abstract_method()  # Implemented
```
