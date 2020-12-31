# TensorFlow Transform

> - 개요
> - VSCode?

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

### Apache Beam Pipe | , >>

~~~Python
raw_data = [
      {'x': 1, 'y': 1, 's': 'hello'},
      {'x': 2, 'y': 2, 's': 'world'},
      {'x': 3, 'y': 3, 's': 'hello'}
  ]

raw_data_metadata = dataset_metadata.DatasetMetadata(
    dataset_schema.from_feature_spec({
        'y': tf.io.FixedLenFeature([], tf.float32),
        'x': tf.io.FixedLenFeature([], tf.float32),
        's': tf.io.FixedLenFeature([], tf.string),
    }))

with tft_beam.Context(temp_dir=tempfile.mkdtemp()):
    transformed_dataset, transform_fn = (
        (raw_data, raw_data_metadata) | tft_beam.AnalyzeAndTransformDataset(
            preprocessing_fn))

transformed_data, transformed_metadata = transformed_dataset  # pylint: disable=unused-variable

print('\nRaw data:\n{}\n'.format(pprint.pformat(raw_data)))
print('Transformed data:\n{}'.format(pprint.pformat(transformed_data)))

"""
Raw data:
[{'s': 'hello', 'x': 1, 'y': 1},
 {'s': 'world', 'x': 2, 'y': 2},
 {'s': 'hello', 'x': 3, 'y': 3}]

Transformed data:
[{'s_integerized': 0,
  'x_centered': -1.0,
  'x_centered_times_y_normalized': -0.0,
  'y_normalized': 0.0},
 {'s_integerized': 1,
  'x_centered': 0.0,
  'x_centered_times_y_normalized': 0.0,
  'y_normalized': 0.5},
 {'s_integerized': 0,
  'x_centered': 1.0,
  'x_centered_times_y_normalized': 1.0,
  'y_normalized': 1.0}]
"""
~~~

- result = pass_this | 'name this step' >> to_this_call
- to_this_call을 호출, pass_this 객체를 전달, name this step으로 참조됨

## VSCode?

- 스크립트나 일반 주피터 노트북으로는 잘 되는데 VSCode Interactive Python이나 VSCode 주피터 노트북으로 실행하면 온갖 에러가 뜬다...왜지?
