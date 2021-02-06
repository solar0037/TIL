# 테스트

> - `test` 패키지
> - 이름 규칙
> - 유닛테스트

- [단위 테스트 소개](https://flutter-ko.dev/docs/cookbook/testing/unit/introduction)

## `test` 패키지

pubspec.yaml

```YAML
dev_dependencies:
  test: any  # 버전 충돌이 안 일어나게 하는 법은 잘 모르겠다.
```

## 이름 규칙

- `lib`에는 코드, `test`에는 테스트
- [이름]_test.dart

```
awesome_app
├── lib
│   └── counter.dart
│       └── concept.md
└── test
    └── counter_test.dart
```

## 유닛테스트

```Dart
import 'package:test/test.dart';
import 'package:awesome_app/counter.dart';

void main() {
  test('Counter value should be incremented', () {
    final counter = Counter();
    counter.increment();
    expect(counter.value, 1);
  });
}
```

- `test(description, callbackFn)` 형태
- `expect` 사용 (==`assert`)
