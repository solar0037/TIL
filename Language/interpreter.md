# 인터프리터

> - 개요
> - 주요 개념

- [Let's Build A Simple Interpreter. Part 1.](https://ruslanspivak.com/lsbasi-part1/)

## 개요

- 인터프리터: 프로그래밍 언어를 실행시간에 직접 해석하는 방식
- 미리 실행 가능한 파일을 만들어 놓는 컴파일 방식보다 실행이 느림

## 주요 개념

### 코드 실행 구성 요소

- 토큰(token): 종류와 값으로 이루어진 객체
- 렉서(lexer): 코드를 토큰으로 분해하는 부분 (=scanner, tokenizer)
- 어휘소(lexeme): 토큰을 이루는 문자(들)
- 파서(parser): 토큰의 표현을 해석하는 부분

```
   1       +       2
[token] [token] [token]

<token>    |  <lexeme>
--------------------------------
[INTEGER]  |  1, 2, 3, 4, 5, ...
[PLUS]     |  +
[MINUS]    |  -

(INTEGER, 1), (PLUS, +), (INTEGER, 2)
-> 1 + 2
-> 3

```

### 분석 순서

- 토큰화(tokenize)
- 파싱(parse)
- 평가(?)(evaluation) - 무엇이라고 해야 할 지 모르겠다
