# JavaScript 함수형 프로그래밍

> - 배열 내장 함수
>   - map()
>   - reduce()
>   - filter()
> - 화살표 함수
> - currying
> - 재귀함수

- [Functional Programming in Javascript](https://dev-momo.tistory.com/entry/Functional-Programming-in-Javascript)
- [람다, 익명 함수, 클로저](https://hyunseob.github.io/2016/09/17/lambda-anonymous-function-closure/)
- [함수형 프로그래밍의 Currying](https://velog.io/@kmp1007s/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98-Currying)

## 배열 내장 함수

- 자바스크립트의 배열에 내장되어 있는 함수를 이용해 함수형 프로그래밍 실현
- 원본 배열을 수정하지 않고(부작용 X) 새로운 값을 만들어 사용한다.
- ES6에 추가된 문법들로 함수형 프로그래밍 실현 가능

### map()

```JavaScript
arr.map(f(x))
```
- 배열에 함수를 적용시켜 새로운 배열을 반환

```JavaScript
console.log([1, 2, 3].map((x) => x * 2));  // [ 2, 4, 6 ]
```

### reduce()

```JavaScript
arr.reduce(f(acc, x))
```
- 배열의 각 원소에 리듀서(reducer) 함수를 실행하여 결과값을 반환

```JavaScript
const reducer = (acc, x) => acc + x;
console.log([1, 2, 3].reduce(reducer));  // 6
```

- 리듀서(reducer) 함수의 반환값: 누산기(acc)에 누적
- 누산기는 유지됨 -> 배열이 하나의 값으로

### filter()

```JavaScript
arr.filter(f(x))
```
- 배열에서 함수에 맞는 것만 골라 새로운 배열 반환

```JavaScript
console.log([1, 2, 3, 4, 5, 6].filter((x) => x % 2 === 0))  // [ 2, 4, 6 ]
```

## 화살표 함수

- 함수의 작성을 더 간단하게
- 인수 한 개 -> 괄호 생략 가능
- 즉시 반환 -> 괄호 & return 생략 가능

```JavaScript
const arr = ["John Doe", "Guido Van Rossum", "Linus Torvalds"];
console.log(arr.map(function(s) { return s.length }));
console.log(arr.map(s => s.length));
```

## currying

- 여러 개의 인자를 받는 함수를 f(1, 2)
- 단일 인자를 받는 함수의 체인으로 f(1)(2)

```JavaScript
function f(x) {
  return function(y) {
    return x + y;
  };
};
// const f = (x) => {
//   return (y) => {
//     return x + y;
//   };
// };
console.log(f(1)(2));
```

## 재귀함수

- 함수 내에서 자기 자신을 호출하는 함수
- 자바스크립트는 [꼬리 재귀 최적화](https://velog.io/@yesdoing/%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94Tail-Call-Optimization%EB%9E%80-2yjnslo7sr)를 제공

```JavaScript
function fact(n) {
  if (n == 1) return 1;
  else return n * fact(n - 1);
}
console.log(fact(5));  // 120
```
