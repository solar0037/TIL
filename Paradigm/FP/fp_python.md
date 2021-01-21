# 함수형 프로그래밍

> - 함수형 프로그래밍
> - map, reduce, filter
> - lambda
> - 재귀함수
> - 꼬리재귀
> - Iterator
> - Generator

- [Functional Programming HOWTO](https://docs.python.org/3/howto/functional.html)
- [Tail Recursion in Python](https://chrispenner.ca/posts/python-tail-recursion)

## 함수형 프로그래밍

- 여러 순수함수를 정의하고 함수들을 조합해 사용하는 프로그래밍 패러다임
- 순수함수: 같은 값을 입력받으면 항상 같은 결과를 반환하는 함수
- 부작용 (입출력, 결과 달라짐 등) 적음
- if/else, for/while 피하기
- 대신에 map, reduce, filter 등 사용

## map, reduce, filter
### map

- map(func, *iterables): iterables에 func 함수를 각각 적용한 map 객체 생성
- list(map()): map 객체를 다시 리스트로 변환

~~~Python
arr = [1, 2, 3, 4, 5]
def multiply_two(x):
    return x * 2

print(list(map(multiply_two, arr)))  # (2, 4, 6, 8, 10)
~~~

### reduce

- reduce(function, sequence[, initial]) -> value: sequence에 인자가 두 개인 function을 누적해 적용
- 배열 차원 줄이기 (벡터 -> 스칼라)
- initial: 초기값
- functools 모듈 필요

~~~Python
from functools import reduce
arr = [1, 2, 3, 4, 5]
def sum(a, b):
    return a + b

print(reduce(sum, arr))  # 15 (arr의 값 전부 더하기)
~~~

### filter

- filter(function or None, iterable): iterable 중에서 function의 값이 참인 것만 골라 filter 객체 생성
- function이 None이면 iterable 중 참인 것만 반환
- list(filter()): filter 객체를 다시 리스트로 변환

~~~Python
arr = [1, 2, 3, 4, 5]
def is_odd(x):
    return x % 2

print(list(filter(is_odd, arr)))  # [1, 3, 5]
~~~

## lambda

- 익명 함수
- lambda [매개변수]: [함수식]
- 타입: ``<function <lambda>>``
- calllable (호출 가능한) 함수 반환

~~~Python
f = lambda x: x * 2
print(f(10))  # 20
~~~

### lambda와 map 함께 사용하기

~~~Python
arr = [1, 2, 3, 4, 5]
print(list(map(lambda x: x * 2, arr)))  # [2, 4, 6, 8, 10]
~~~

### 재귀함수

- 함수 내에서 자기 자신을 호출하는 함수
- 무한 재귀에 빠지지 않게 주의
- 파이썬 제한: 1000번 (스택 오버플로우 방지)
- while 대신 재귀함수로 구현하기

~~~Python
# while
def fact(n):
    res = 1
    while n > 0:
        res *= n
        n -= 1
    return res

# walrus operator 사용 (Python ^3.8)
def fact(n):
    res = n
    while n > 1:
        res *= (n := n - 1)
    return res

# 재귀함수
def fact(n):
    if n == 1:
        return 1
    return n * fact(n - 1)
~~~

### 꼬리재귀

- 재귀호출: 하위 호출이 끝나고 다시 상위 호출까지 올라가야 함 -> Stack에 쌓아 놔야 해서 비효율적
- 꼬리재귀: 함수를 호출해서 값을 반환받은 후 아무것도 하지 않게 하기 위해 재귀함수의 가장 마지막(꼬리) 위치에서 반환
- 파이썬은 꼬리재귀 최적화를 지원하지 않음 (recursion보다는 iteration에 초점을 맞춘 언어)
- [Tail Recursion in Python](https://chrispenner.ca/posts/python-tail-recursion) 글을 보면 '할 수'는 있다 (권장되지는 않음)

~~~Python
def fact(n, acc=1):
    if n == 0:
        return acc
    return fact(n - 1, n * acc)
~~~

### Iterator

- iter(iterable[, sentinel]) -> iterator
- list 등의 iterable 객체를 받아 iterator 생성
- next(iterator[, default]): iterator의 다음 원소 반환 (iterator의 카운트 += 1)
- list(iterator), tuple(iterator): iterator를 list나 tuple로 변환
- 사실 max(), min(), map(), filter() 등도 list/tuple이 아니라 iterable을 인자로 받음

~~~Python
l = [1, 2, 3]
it = iter(l)
print(type(it))  # list_iterator
print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # 3
print(next(it))  # Exception: StopIteration
~~~

### Generator

- yield 키워드를 사용하는 특별한 형태의 함수
- iterator를 더 쉽게 작성할 수 있음

~~~Python
def gen_ints(n):
    for i in range(n):
        yield i

gen = gen_ints(3)
print(type(it))  # generator object gen_ints
print(next(it))  # 0
print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # Exception: StopIteration
~~~
