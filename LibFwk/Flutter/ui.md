# UI

- [Introduction to Widgets](https://flutter-ko.dev/docs/development/ui/widgets-intro)
- [Flutter 레이아웃](https://flutter-ko.dev/docs/development/ui/layout)
- [Flutter: ListView & GridView](https://medium.com/flutterfly-tech/flutter-listview-gridview-ce7177812b1d)
- [Adding interactivity to your Flutter app](https://flutter-ko.dev/docs/development/ui/interactive)
- [새로운 화면으로 이동하고, 되돌아오기](https://flutter-ko.dev/docs/cookbook/navigation/navigation-basics)

> - 위젯
>   - Text
> - 레이아웃
>   - Container
>   - Row, Column
>     - 정렬하기
>   - ListView
>   - GridView
> - StatelessWidget vs StatefulWidget
> - 상태 관리의 주체
>   - 스스로 관리
>   - 부모가 관리
>   - 섞어 사용
> - 화면 전환
>   - Named route
>   - Named route로 인자 넘기기
>     - 위젯에서 인자를 추출하기
>     - onGenerateRoute를 이용해 인자 추출하기

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

## StatelessWidget vs StatefulWidget

- `StatelessWidget`: 상태가 없는 위젯. 단순한 아이콘, 배경사진 등
- `StatefulWidget`: 상태가 있는 위젯. 사용자의 입력, 데이터, 또는 애니메이션을 통해 변함.

## 상태 관리의 주체

### 스스로 관리

- 위젯이 상태 변화에 대한 콜백함수를 내장
- 외부에서 상태를 변화시킬 필요가 없음
- 예: `ListView`의 스크롤 함수

main.dart

```Dart
Scaffold(
  child: TapboxA(),  // 인자 없이 사용
)
```

TapboxA.dart

```Dart
class TapboxA extends StatefulWidget {
  TapboxA({Key key}) : super(key: key);

  @override
  _TapboxAState createState() => _TapboxAState();
}

class _TapboxAState extends State<TapboxA> {
  bool _active = false;  // 상태: _active (bool)

  void _handleTap() {  // 탭 동작을 관리하는 함수
    setState(() {  // 직접 변경하면 아무것도 일어나지 않음
      _active = !_active;
    });
  }

  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: _handleTap,  // _handleTap() 함수로 GestureDetector 위젯 생성
      child: Container(
        child: Center(
          child: Text(
            _active ? 'Active' : 'Inactive',  // _active에 따라 변하는 부분
            style: TextStyle(fontSize: 32.0, color: Colors.white),
          ),
        ),
        width: 200.0,
        height: 200.0,
        decoration: BoxDecoration(
          color: _active ? Colors.lightGreen[700] : Colors.grey[600],  // _active에 따라 변하는 부분
        ),
      ),
    );
  }
}
```

### 부모가 관리

- 부모는 `StatefulWidget`, 자식은 `StatelessWidget`이 됨
- 적절한 콜백 함수를 인자로 넘겨주기

ParentWidget.dart

```Dart
class ParentWidget extends StatefulWidget {
  @override
  _ParentWidgetState createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget> {
  bool _active = false;

  void _handleTapboxChanged(bool newValue) {  // _active를 설정하는 콜백 함수
    setState(() {
      _active = newValue;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: TapboxB(
        active: _active,  // 자식 위젯으로 상태 넘겨주기
        onChanged: _handleTapboxChanged,  // onChanged 인자로 콜백함수 넘겨주기
      ),
    );
  }
}
```

TapboxB.dart

```Dart
class TapboxB extends StatelessWidget {  // onChanged 함수를 통해 구현하기
  TapboxB({Key key, this.active: false, @required this.onChanged})
      : super(key: key);

  final bool active;  // 상태
  final ValueChanged<bool> onChanged;  // 콜백함수

  void _handleTap() {  // 생성자로 주어진 onChanged 콜백함수를 이용해 active 상태 조절
    onChanged(!active);
  }

  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: _handleTap,
      child: Container(
        child: Center(
          child: Text(
            active ? 'Active' : 'Inactive',  // active에 따라 변하는 부분
            style: TextStyle(fontSize: 32.0, color: Colors.white),
          ),
        ),
        width: 200.0,
        height: 200.0,
        decoration: BoxDecoration(
          color: active ? Colors.lightGreen[700] : Colors.grey[600],  // active에 따라 변하는 부분
        ),
      ),
    );
  }
}
```

### 섞어 사용

- 융통성 있게 관리!
- 사실 위의 것도 이해가 잘 안 가서 다음에 정리해야겠다

## 화면 전환

- `Navigator` 클래스를 이용해 화면 전환
- `Widget build()` 함수에서 `BuildContext context` 객체를 이용해 현재의 문맥?을 판단하는 듯 (화면 전환 시 왼쪽 위에 돌아가기 화살표가 뜬다)

MainRoute.dart

```Dart
ElevatedButton(
  child: Text('Open Route'),
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => SubRoute()),
    );
  },
)
```

SubRoute.dart

```Dart
ElevatedButton(
  child: Text('Go back!'),
  onPressed: () {
    Navigator.pop(context);
  },
)
```

### Named route

main.dart

```Dart
MaterialApp(
  title: 'Navigation Basics',
  initialRoute: '/',  // initialRoute를 사용하면 home은 설정 X
  routes: {
    '/': (context) => FirstScreen(),
    '/second': (context) => SecondScreen(),
  },
)
```

routes.dart

```Dart
onPressed: () {
  Navigator.pushNamed(context, '/second');  // Navigator.pop()은 동일
}
```

### Named route로 인자 넘기기

- 인자 정의(공통)

```Dart
class ScreenArguments {
  final String title;
  final String message;

  ScreenArguments(this.title, this.message);
}
```

#### 위젯에서 인자를 추출하기

- 인자를 추출하는 위젯 만들기

```Dart
...
final ScreenArguments args = ModalRoute.of(context).settings.arguments;

return Scaffold(
  appBar: AppBar(
    title: Text(args.title),
  ),
  body: Center(
    child: Text(args.message),
  ),
);
```

- 위젯 등록하기

```Dart
MaterialApp(
  routes: {
    // /extractArguments: (context) => ExtractArgumentsScreen()
    ExtractArgumentsScreen.routeName: (context) => ExtractArgumentsScreen(),
  },     
);
```

- 위젯으로 이동하기

```Dart
// 인자를 RouteSettings의 일부로 넘김
Navigator.pushNamed(
  context,
  ExtractArgumentsScreen.routeName,  // /extractArguments
  arguments: ScreenArguments(  // 위젯에서 파싱할 인자
    'Extract Arguments Screen',
    'This message is extracted in the build method.',
  ),
);
```

#### onGenerateRoute를 이용해 인자 추출하기

- 생성자를 통해 인자를 전달받는 위젯 만들기

```Dart
class PassArgumentsScreen extends StatelessWidget {
  static const routeName = '/passArguments';

  final String title;
  final String message;

  // onGenerateRoute 함수에서 추출된 인자들이 사용됨
  const PassArgumentsScreen({
    Key key,
    @required this.title,
    @required this.message,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Text(message),
      ),
    );
  }
}
```

- onGenerateRoute 함수를 통해 인자 추출하기

```Dart
MaterialApp(
  // named route를 처리할 함수
  onGenerateRoute: (settings) {
    // /passArguments 라우트를 푸시할 때만 정의. 그런데 이렇게 하면 반환값이 없다고 IDE가 화내서 else에 기본값을 적어 주었다(??)
    if (settings.name == PassArgumentsScreen.routeName) {  // /passArguments
      // args: {title, message}
      final ScreenArguments args = settings.arguments;

      // 인자 넘기기
      return MaterialPageRoute(
        builder: (context) => PassArgumentsScreen(
          title: args.title,
          message: args.message,
        );
      );
    }
  },
);
```

- 위젯으로 이동하기

```Dart
Navigator.pushNamed(
  context,
  PassArgumentsScreen.routeName,
  arguments: ScreenArguments(
    'Accept Arguments Screen',
    'This message is extracted in the onGenerateRoute function.',
  ),
);
```
