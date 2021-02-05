# UI

- [Introduction to Widgets](https://flutter-ko.dev/docs/development/ui/widgets-intro)
- [Flutter 레이아웃](https://flutter-ko.dev/docs/development/ui/layout)
- [Flutter: ListView & GridView](https://medium.com/flutterfly-tech/flutter-listview-gridview-ce7177812b1d)

> - 위젯
>   - Text
> - 레이아웃
>   - Container
>   - Row, Column
>     - 정렬하기
>   - ListView
>   - GridView

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
  );
}
```

- 플러터의 거의 모든 것은 위젯
- `runApp()` 함수 호출 -> 위젯을 트리의 최상단에 위치
- 위젯은 보통 `StatelessWidget` 또는 `StatefullWidget`을 상속 (이들은 `Widget`의 추상 서브클래스)
- `build()` 함수 구현하기

### Text

- 방향성 프로퍼티 필요 (이 상황에서는 `TextDirection.ltr`, 보통 상위 `MaterialApp`, `WidgetsApp` 위젯에 의해 결정)
- 에러가 나면 시뻘건 바탕에 무섭게 에러를 띄운다...

## 레이아웃

- 레이아웃도 결국 위젯 (그냥 위에 통합시킬까)

### Container

- 상위 위젯을 한번 더 감싸기
- `decoration` 프로퍼티: 배경 색상 등 설정
- `margin`, `padding`, `constraint` 등의 설정 -> 자식 위젯은 부모에 맞춰짐
- `child` 프로퍼티 (자식 위젯 한 개)

```Dart
Widget _buildTextContainer() => Container(
  decoration: BoxDecoration(color: Colors.white),
  child: Text(
    'This is a text',
    textDirection: TextDirection.ltr,
  ),
);

```

### Row, Column

- `Row`: 가로 정렬
- `Column`: 세로 정렬
- `mainAxis`: 주축
- `crossAxis`: 횡축 (주축에 수직)
- `children` 프로퍼티 (자식 위젯 여러 개, nesting도 가능!)

#### 정렬하기

- `mainAxisAlignment`: 주축 정렬 설정
  - `MainAxisAlignment.center`, `end`, `start`, `spaceEvenly`, ...
- `crossAxisAlignment`: 횡축 정렬 설정
  - `CrossAxisAlignment.baseline`, `center`, `end`, `start`, `stretch`, ...
- 주의: `mainAxisAlignment`와 `crossAxisAligment`에 사용할 수 있는 값들이 약간 다르다. 왜 그런지는 아직 잘 모르겠다.

### ListView

- children 안의 것들을 리스트 형태로 보여주기
- `children` 프로퍼티: `List<Widget>` 타입 (`ListTitle`, `Container`, ...)

```Dart
Widget _buildList() => ListView(
  children: <Widget>[
    ListTile(
      leading: FlutterLogo(),  // 타일 맨 앞
      trailing: Icon(Icons.more_vert),  // 타일 맨 뒤
      title: Text('One-line with leading & trailing widget'),
    ),
    ListTile(
      leading: FlutterLogo(),
      trailing: Icon(Icons.more_vert),
      title: Text('Hello Flutter again!'),
    ),
    ListTile(
      leading: FlutterLogo(),
      trailing: Icon(Icons.more_vert),
      title: Text('Hello Flutter again again!'),
    ),
  ],
);
```

### GridView

- `children` 안의 것들을 그리드 형태로 보여주기
- `GridView.extent`: 크기 기준
- `GridView.count`: 개수 기준

```Dart
Widget _buildGrid() => GridView.count(
  crossAxisCount: 5,  // 가로줄에 최대 5개
  // 50개의 Container(Card()) 생성
  children: List.generate(50, (index) => Container(
    child: Card(
      color: Colors.teal,
    ),
  )),
);
```
