# TensorFlow Serving

> - 개요
> - 설치
> - REST API 예제

- [서빙 모델](https://www.tensorflow.org/tfx/guide/serving)
- [TensorFlow Serving으로 TensorFlow 모델 학습 및 제공](https://www.tensorflow.org/tfx/tutorials/serving/rest_simple)

## 개요

- 프로덕션 환경의 유연한, 고성능 서빙 시스템 필요
- TF-Serving을 통해 서버와 기존 아키텍처를 유지, 새로운 알고리즘과 실험을 쉽게 배포

## 설치

- 도커 컨테이너 (권장)
~~~Bash
docker pull tensorflow/serving
~~~

- 직접 설치 (Debian, root권한)
~~~Bash
echo "deb http://storage.googleapis.com/tensorflow-serving-apt stable tensorflow-model-server tensorflow-model-server-universal" | tee /etc/apt/sources.list.d/tensorflow-serving.list && \
curl https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg | apt-key add -
apt-get install tensorflow-model-server
~~~

## REST API 예제
### 모델 export

~~~Python
# Fetch the Keras session and save the model
# The signature definition is defined by the input and output tensors,
# and stored with the default serving key
import tempfile

MODEL_DIR = tempfile.gettempdir()
version = 1
export_path = os.path.join(MODEL_DIR, str(version))
print('export_path = {}\n'.format(export_path))

tf.keras.models.save_model(
    model,
    export_path,
    overwrite=True,
    include_optimizer=True,
    save_format=None,
    signatures=None,
    options=None
)

print('\nSaved model:')
!ls -l {export_path}
~~~

### saved_model_cli로 모델 탐색

~~~Bash
!saved_model_cli show --dir {export_path} --all
~~~

### 서버 시작
~~~Python
os.environ["MODEL_DIR"] = MODEL_DIR
~~~

~~~Bash
nohup tensorflow_model_server \
  --rest_api_port=8501 \
  --model_name=fashion_model \
  --model_base_path="${MODEL_DIR}" >server.log 2>&1
~~~

### 예측 API 사용
~~~Python
def show(idx, title):
    plt.figure()
    plt.imshow(test_images[idx].reshape(28,28))
    plt.axis('off')
    plt.title('\n\n{}'.format(title), fontdict={'size': 16})

import random
rando = random.randint(0,len(test_images)-1)
show(rando, 'An Example Image: {}'.format(class_names[test_labels[rando]]))

# An Example Image: Shirt
# (shirt image)
~~~
- show() 함수 정의

~~~Python
import json
data = json.dumps({"signature_name": "serving_default", "instances": test_images[0:3].tolist()})
print('Data: {} ... {}'.format(data[:50], data[len(data)-52:]))

# Data: {"signature_name": "serving_default", "instances": ...  [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]]]]}
~~~
- 데이터 준비

~~~Python
import requests
headers = {"content-type": "application/json"}
json_response = requests.post('http://localhost:8501/v1/models/fashion_model:predict', data=data, headers=headers)
predictions = json.loads(json_response.text)['predictions']

show(0, 'The model thought this was a {} (class {}), and it was actually a {} (class {})'.format(
  class_names[np.argmax(predictions[0])], np.argmax(predictions[0]), class_names[test_labels[0]], test_labels[0]))

# The model thougth this was a Ankle boot (class 9), and it was actually a Ankle boot (class 9)
# (ankle boot image)
# ...
~~~
- POST 메서드로 요청
