# UI

- [Introduction to Widgets](https://flutter-ko.dev/docs/development/ui/widgets-intro)

> - 위젯
>   - Text

## 위젯

```Dart
import 'package:flutter/material.dart';

// void main() => runApp(MyApp());  // 화살표 함수를 사용, return 생략
void main() {
  runApp(
    Center(
      child: Text(
        'Hello, World!',
        textDirection: TextDirection.ltr,
      )
    )
  )
}
```

- 플러터의 거의 모든 것은 위젯
- runApp() 함수 호출 -> 위젯을 트리의 최상단에 위치
- 위젯은 보통 StatelessWidget 또는 StatefullWidget을 상속 (이들은 Widget의 서브클래스)
- build() 함수 구현하기

### Text

- Directionality 위젯 필요 (이 상황에서는 `TextDirection.ltr`, 보통 MaterialApp, WidgetsApp 위젯에 의해 결정)
- 에러가 나면 시뻘건 바탕에 무섭게 에러를 띄운다...
