# TensorFlow Transform

> - 개요

- [TensorFlow Transform 시작하기](https://www.tensorflow.org/tfx/transform/get_started)
- [TensorFlow Extended (TFX) (TensorFlow Dev Summit 2018)](https://www.youtube.com/watch?v=vdG7uKQ2eKk&feature=youtu.be&t=199)

## 개요
### tf.Transform

- 모델 훈련과 서빙 시 같은 전처리 함수를 사용해야 함
- 파이프라인을 재설계하거나 여러가지 배포 플랫폼이 있음 -> 난감
- tf.Transform을 사용해 학습 시에는 preprocessing_fn 정의, 배포 시에는 tf.Graph로 변환
- 프로덕션에서 데이터 전처리에 코드를 사용하지 않아도 되어서 이득

### 전처리 함수 예제

~~~Python
import tensorflow as tf
import tensorflow_transform as tft
import tensorflow_transform.beam as tft_beam

def preprocessing_fn(inputs):
  x = inputs['x']
  y = inputs['y']
  s = inputs['s']
  x_centered = x - tft.mean(x)
  y_normalized = tft.scale_to_0_1(y)
  s_integerized = tft.compute_and_apply_vocabulary(s)
  x_centered_times_y_normalized = x_centered * y_normalized
  return {
      'x_centered': x_centered,
      'y_normalized': y_normalized,
      'x_centered_times_y_normalized': x_centered_times_y_normalized,
      's_integerized': s_integerized
  }
~~~

- tft.mean(x): x의 평균
- tft.scale_to_0_1(y): 최소, 최대값 계산하여 y값 조정
- tft.compute_and_apply_vocabulary(s): 문자열 s를 정수로 매핑
