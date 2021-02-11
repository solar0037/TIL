# Flutter 웹개발

> - 개발 준비
> - 빌드하기
> - 특징

- [Flutter 에서 WEB을 추구하면 안되는걸까](https://zerogyun.dev/2020/02/07/Flutter%EC%97%90%EC%84%9C%20web%EC%9D%84%20%EC%B6%94%EA%B5%AC%ED%95%98%EB%A9%B4%20%EC%95%88%EB%90%98%EB%8A%94%EA%B1%B8%EA%B9%8C/)

## 개발 준비

- beta 채널로 전환하기

```Bash
$ flutter channel beta
$ flutter upgrade
```

- 웹 활성화

```Bash
$ flutter config --enable-web
```

- `flutter doctor [-v]`를 실행하면 devices 영역에 Chrome, Edge 등의 웹브라우저가 나타남

## 빌드하기

```Bash
$ flutter build web
$ flutter run --release  # 빌드 실행까지 하기
```

## 특징

- 아직 beta 채널이기 때문에 불안정
- 위젯 기반으로 SPA 웹개발 가능 (JS 외의 또다른 선택지가 생겼다)
