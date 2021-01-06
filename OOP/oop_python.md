# 객체지향형 프로그래밍

> - 객체지향형 프로그래밍
> - class, ``__init__``, ``__del__``
> - getter, setter
> - classmethod, staticmethod
> - 추상클래스

- [객체 지향 프로그래밍(OOP) 개념](https://victorydntmd.tistory.com/117)
- [Python 밑줄 문자와 던더 (name mangling)](https://blog.naver.com/reisei11/221749496623)
- [44. class 정리 - 정적메소드 @classmethod와 @staticmethod의 정리](https://wikidocs.net/16074)

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

## getter, setter

- 외부에서 객체의 정보에 접근하는 것을 신중하게
- 파이썬에는 private 제한자가 없기 때문에 강제되지는 않음
- _method 형태로 쓰면 내부용으로 쓰일 것이라는 표시 (protected?)
- __method 형태로 쓰면 (강한) 접근하지 말라는 표시 (private), name mangling

~~~Python
class Account:
    def __init__(self, id_, password):
        self.id_ = id_
        self.password = password
    
    def get_password(self):
        encrypted = "*" * len(self.password)
        return encrypted
    
    def set_password(self, password):
        self.password = password

acc = Account(21, "1234asdf")
print(acc.get_password())  # ********
acc.set_password("12345678")
~~~

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
class AnotherLock(Lock):
    def __init__(self):
        self.__key = "overriden"

print(dir(Lock()))
# ['_AnotherLock__key', ...]
print(AnotherLock().__key)
# AttributeError  # __key는 애초에 name mangling되어 존재하지도 않음
~~~

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
