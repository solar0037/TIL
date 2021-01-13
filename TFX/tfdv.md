# TensorFlow Data Validation

> - 개요
> - 얘제

- [TensorFlow 데이터 유효성 검사](https://www.tensorflow.org/tfx/tutorials/data_validation/tfdv_basic)

## 개요
### Data Validation

- 데이터의 특성 관찰하기 (프로덕션에서의 변화 예상)
- 데이터 검증, 학습-평가-서빙 데이터셋 비교

## 예제

### DatasetFeatureStatisticsList (prototype) 객체 생성

~~~Python
train_stats = tfdv.generate_statistics_from_csv(data_location=TRAIN_DATA)
~~~

### 시각화

~~~Python
tfdv.visualize_statistics(train_stats)
~~~

- 0이나 빠진 값이 높은 컬럼이 붉은색으로 표시됨

### 스키마 생성

~~~Python
schema = tfdv.infer_schema(statistics=train_stats)
tfdv.display_schema(schema=schema)
~~~

- 데이터셋을 기반으로 스키마 생성
- Type, Presence, Valency, Domain 정보 출력
- Domain의 경우에는 고유한 값 출력

### 평가 데이터 시각화

~~~Python
# Compute stats for evaluation data
eval_stats = tfdv.generate_statistics_from_csv(data_location=EVAL_DATA)

# Compare evaluation data with training data
tfdv.visualize_statistics(lhs_statistics=eval_stats, rhs_statistics=train_stats,
                          lhs_name='EVAL_DATASET', rhs_name='TRAIN_DATASET')
~~~

- eval_stats를 train_stats와 비교
- 그래프를 통해 직관적이고 interactive하게 검증 가능

### 평가 데이터 검사

~~~Python
# Check eval data for errors by validating the eval data stats using the previously inferred schema.
anomalies = tfdv.validate_statistics(statistics=eval_stats, schema=schema)
tfdv.display_anomalies(anomalies)
~~~

- 기존 schema와 다른 부분이 있다면 (범위 벗어남, 사전에 없는 값 등) anomaly로 표시

### 스키마 수정하기 ~~끼워 맞추기?~~

~~~Python
# Relax the minimum fraction of values that must come from the domain for feature company.
company = tfdv.get_feature(schema, 'company')
company.distribution_constraints.min_domain_mass = 0.9

# Add new value to the domain of feature payment_type.
payment_type_domain = tfdv.get_domain(schema, 'payment_type')
payment_type_domain.value.append('Prcard')

# Validate eval stats after updating the schema 
updated_anomalies = tfdv.validate_statistics(eval_stats, schema)
tfdv.display_anomalies(updated_anomalies)
~~~

### 스키마 환경

~~~Python
serving_stats = tfdv.generate_statistics_from_csv(SERVING_DATA)
serving_anomalies = tfdv.validate_statistics(serving_stats, schema)

tfdv.display_anomalies(serving_anomalies)
~~~

- 환경에 따라 스키마가 수정될 필요가 있음
- 예: 지도 학습: 서빙 환경에서는 레이블(정답) X
- 기본적으로는 anomaly로 표시됨 (예: column dropped)

### 스키마 수정

~~~Python
options = tfdv.StatsOptions(schema=schema, infer_type_from_schema=True)
serving_stats = tfdv.generate_statistics_from_csv(SERVING_DATA, stats_options=options)
serving_anomalies = tfdv.validate_statistics(serving_stats, schema)

tfdv.display_anomalies(serving_anomalies)
~~~

- 예: INT에서 FLOAT로 타입 변경 가능 -> schema에게 타입 추론하라고 알려주기

~~~Python
# All features are by default in both TRAINING and SERVING environments.
schema.default_environment.append('TRAINING')
schema.default_environment.append('SERVING')

# Specify that 'tips' feature is not in SERVING environment.
tfdv.get_feature(schema, 'tips').not_in_environment.append('SERVING')

serving_anomalies_with_env = tfdv.validate_statistics(
    serving_stats, schema, environment='SERVING')

tfdv.display_anomalies(serving_anomalies_with_env)
~~~

- default_environment, in_environment, not_in_environment로 접근
- TRAINING, SERVING 환경으로 나누기
- 예: SERVNG에서는 schema의 tips feature가 없음

### Drift, Skew

- Drift: 데이터가 다름 (이전 데이터와 비교하여)
- Skew: 학습과 서빙 데이터가 다름
  - Schema Skew: 학습 데이터와 서빙 데이터가 같은 스키마를 도출하지 않음
  - Feature Skew: 서빙 시 다른 값의 feature를 만남 (다른 전처리 코드 등)
  - Distribution Skew: 데이터의 분포가 완전히 다름

~~~Python
# Add skew comparator for 'payment_type' feature.
payment_type = tfdv.get_feature(schema, 'payment_type')
payment_type.skew_comparator.infinity_norm.threshold = 0.01

# Add drift comparator for 'company' feature.
company=tfdv.get_feature(schema, 'company')
company.drift_comparator.infinity_norm.threshold = 0.001

skew_anomalies = tfdv.validate_statistics(train_stats, schema,
                                          previous_statistics=eval_stats,
                                          serving_statistics=serving_stats)

tfdv.display_anomalies(skew_anomalies)
~~~

- drift: High Linfty distance between training and serving
- skew: High Linfty distance between current and previous
- 등의 메시지 출력

### 스키마 freezing

~~~Python
from tensorflow.python.lib.io import file_io
from google.protobuf import text_format

file_io.recursive_create_dir(OUTPUT_DIR)
schema_file = os.path.join(OUTPUT_DIR, 'schema.pbtxt')
tfdv.write_schema_text(schema, schema_file)
~~~

- 파일로 변환히여 상태를 freezing
