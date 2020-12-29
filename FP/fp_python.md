# 함수형 프로그래밍

> - 함수형 프로그래밍
> - map, reduce, filter
> - lambda

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
- 타입: `<function <lambda>>
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
