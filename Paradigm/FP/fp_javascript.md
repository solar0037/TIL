# JavaScript 함수형 프로그래밍

> - 배열 내장 함수
>   - map()
>   - reduce()
>   - filter()

## 배열 내장 함수

- 자바스크립트의 배열에 내장되어 있는 함수를 이용해 함수형 프로그래밍 실현
- 원본 배열을 수정하지 않고(부작용 X) 새로운 값을 만들어 사용한다.

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
