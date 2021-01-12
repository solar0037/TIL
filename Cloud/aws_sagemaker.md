# AWS SageMaker

> - 개요
> - 노트북 인스턴스
> - 결론

- [Amazon SageMaker로 기계 학습 모델을 구축, 훈련 및 배포하는 방법 | AWS](https://aws.amazon.com/ko/getting-started/hands-on/build-train-deploy-machine-learning-model-sagemaker/)

## 개요

- 머신러닝 프로젝트의 데이터 준비부터 학습, 배포까지 한번에 이루어지는 AWS의 플랫폼
- [boto3](https://github.com/boto/boto3) - AWS Python SDK
- [sagemaker](https://github.com/aws/sagemaker-python-sdk) - SDK를 사용하여 SageMaker 개발

## 노트북 인스턴스

- 위의 자습서 따라해보기

### 자습서에서 고칠 점

- SageMaker SDK가 버전이 2로 바뀜
- 급격한 변화들 (심지어 자습서에 있는 코드도 돌아가지 않음)

### 4a

- sagemaker.s3_input (error) 이 사라짐
- sagemaker.session.s3_input (deprecated?) - 이름 변경 (바꾸지 않아도 돌아는 간다)
- sagemaker.inputs.TrainingInput으로 대체 (함수를 사용하기보다는 객체를 직접 만드는 방향으로 간 것 같다)

```Python
s3_input_train = sagemaker.inputs.TrainingInput(s3_data='s3://{}/{}/train'.format(bucket_name, prefix), content_type='csv')
```

### 4b

- sagemaker.estimator.Estimator의 인자 바뀜 (간결함 추구?)
- train_instance_count -> instance_count
- train_instance_type -> instance_type

```Python
xgb = sagemaker.estimator.Estimator(containers[my_region],role, instance_count=1, instance_type='ml.m4.xlarge',output_path='s3://{}/{}/output'.format(bucket_name, prefix),sagemaker_session=sess)
```

### 5b

- xgb_predictor.content_type = 'text/csv' 라인 삭제 (AttributeError: can't set attribute 발생, 삭제하는 것으로 해결했는데 마음이 찜찜하다)
- sagemaker.predictor.csv_serializer 대신 sagemaker.predictor.CSVSerializer 객체 사용 (4a와 비슷?)

```Python
from sagemaker.serializers import CSVSerializer

test_data_array = test_data.drop(['y_no', 'y_yes'], axis=1).values #load the data into an array
# xgb_predictor.content_type = 'text/csv' # set the data type for an inference  # 삭제
xgb_predictor.serializer = CSVSerializer() # set the serializer type
predictions = xgb_predictor.predict(test_data_array).decode('utf-8') # predict!
predictions_array = np.fromstring(predictions[1:], sep=',') # and turn the prediction into an array
print(predictions_array.shape)
```

### 7a

- xgb_predictor.endpoint 대신 xgb_predictor.endpoint_name 사용 (endpoint 객체가 아니라 이름이라서 그런 듯?)

```Python
sagemaker.Session().delete_endpoint(xgb_predictor.endpoint_name)
bucket_to_delete = boto3.resource('s3').Bucket(bucket_name)
bucket_to_delete.objects.all().delete()
```

## 결론

- 머신러닝 워크플로우 대신 구글링 능력을 얻었다.
- AWS 일해라! (자습서의 오류 때문에 진입장벽이 높아짐)
- 다음에는 Google AI Platform도 써보자.
- sagemaker.estimator.Estimator로 생성한 XGBoost 모델 외에도 TensorFlow, PyTorch 등의 프레임워크를 사용할 수 있다고 하니 해봐야겠다.
