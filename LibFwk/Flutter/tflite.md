# Flutter에서 TFLite 사용하기

> - 사용
> - mnist 예제
>   - `_predict()`
> - 느낀 점

- [tflite | Flutter Package](https://pub.dev/packages/tflite)
- [Flutter에서 TensorFlow Lite로 MINST 숫자 분류 앱 만들어보기](https://medium.com/@cmtyx2/flutter%EC%97%90-tensorflow-lite%EB%A1%9C-minst-%EC%88%AB%EC%9E%90-%EB%B6%84%EB%A5%98-%EC%95%B1-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0-c22a9feaf1a)

## 사용

- 루트 폴더 아래에 `assets`
- `assets` 폴더 아래에 `model.tflite`, `labels.txt`

```
assets
├── model.tflite
└── labels.txt
```

- 모델 불러오기

```Dart
String res = await Tflite.loadModel(
  model: "assets/mnist.tflite",
  labels: "assets/mnist.txt",
)
```

- 자원 정리하기 (모델을 사용한 뒤에 자원을 정리하는 듯?)

```Dart
await Tflite.close();
```

## mnist 예제

- [글](https://medium.com/@cmtyx2/flutter%EC%97%90-tensorflow-lite%EB%A1%9C-minst-%EC%88%AB%EC%9E%90-%EB%B6%84%EB%A5%98-%EC%95%B1-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0-c22a9feaf1a) 참고 - 캔버스에 글씨를 쓰고 손을 놓으면 예측한 결과를 보여주기
- 대략적인 순서

```Dart
Future recognize(List<Offset> points) async {  // 예측 시간 상 비동기로 진행
  final picture = _pointsToPicture(points);  // 캔버스의 점들을 이미지로 변환
  Uint8List bytes = await _imageToByteListUint8(
      picture, Constants.mnistImageSize);  // 이미지를 byte로 변환
  return _predict(bytes);  // byte를 이용해 예측
}
```

### `_predict()`

- byte를 이용해 `Tflite.runModelOnBinary`로 예측하는데 모델이 인수로 안 주어진다. 앞에서 전역으로 초기화한 모델을 사용하는 듯. 어차피 성능이 제한적인 모바일 기기에서 모델을 여러 개 돌릴 일도 없는 것 같다.

```Dart
Future _predict(Uint8List bytes) {
  return Tflite.runModelOnBinary(binary: bytes);
}
```

## 느낀 점

- 공식 문서가 부족하다. TFLite 자체는 꽤 인지도가 있지만 Flutter 패키지 [tflite의 공식 문서](https://pub.dev/documentation/tflite/latest/index.html)는 예제 코드만 있는 수준이다(애초에 예측만 진행하니까 상관 없나 모르겠다).
