# TFX Pipeline

> - MLOps
> - TFX Pipeline

## MLOps
### 머신러닝 모델 프로덕션으로 옮기기
- 데이터 준비
- 데이터 분석
- 데이터 전처리
- 모델 학습
- 모델 평가
- 모델 배포
- 수많은 과정들...효율적으로 할 방법은 없을까?

### MLOps
- MLOps: ML + DevOps
- 데이터 과학 + 소프트웨어 엔지니어링
- 머신러닝 모델의 데이터 준비부터 배포까지 효율적으로 할 수 있는 솔루션 필요
- TFX (TensorFlow Extended), Apache Airflow, Apache Beam, Kubeflow pipeline 등 프레임워크, 도구 등장

## TFX Pipeline
### 개요
- TFX 컴포넌트들을 쉽게 orchestration할 수 있는 프레임워크
- ExampleGen, SchemaGen, Transform, Trainer, Pusher 등 pipeline으로 합치기 (orchestration)

### 템플릿
- 미리 정의된 템플릿 사용 (penguin, taxi)

~~~Bash
tfx template list
tfx template copy --model=[template] --pipeline_name=[pipeline-name] --destination_path=[destination-path]
~~~

~~~
.
├── __init__.py
├── beam_dag_runner.py
├── data
│   └── data.csv
├── data_validation.ipynb
├── kubeflow_dag_runner.py
├── model_analysis.ipynb
├── models
│   ├── __init__.py
│   ├── estimator
│   │   ├── __init__.py
│   │   ├── constants.py
│   │   ├── model.py
│   │   └── model_test.py
│   ├── features.py
│   ├── features_test.py
│   ├── keras
│   │   ├── __init__.py
│   │   ├── constants.py
│   │   ├── model.py
│   │   └── model_test.py
│   ├── preprocessing.py
│   └── preprocessing_test.py
└── pipeline
    ├── __init__.py
    ├── configs.py
    └── pipeline.py
~~~

- data: data.csv
- models: 전처리 코드, 모델 구현
- pipeline: 파이프라인 구현
- pipeline/configs.py: 파이프라인과 컴포넌트 함수에서 사용하는 설정
- pipeline/pipeline.py: TFX 파이프라인 생성
- beam_dag_runner.py: 파이프라인 run 생성

### 파이프라인 Run 생성하기

~~~Bash
python beam_dag_runner.py
~~~

- Apache Beam을 사용한 파이프라인 생성
~~~
├── tfx_metadata
│   └── temp-pipeline
│       └── metadata.db
└── tfx_pipeline_output
    └── temp-pipeline
        └── CsvExampleGen
            └── examples
                └── 1
                    ├── eval
                    │   └── data_tfrecord-00000-of-00001.gz
                    └── train
                        └── data_tfrecord-00000-of-00001.gz
~~~
- tfx_metadata: ML 메타데이터 DB
- tfx_pipeline_output: 파이프라인의 출력

### 파이프라인 커스터마이징

- 직접 파일을 작성
- 예시

~~~Python
import os
from typing import Optional, Text, List

from absl import logging
from ml_metadata.proto import metadata_store_pb2

from tfx.orchestration import metadata
from tfx.orchestration import pipeline
from tfx.orchestration.beam.beam_dag_runner import BeamDagRunner

from tfx.components import CsvExampleGen
from tfx.utils.dsl_utils import external_input


PIPELINE_NAME = 'my_pipeline'
PIPELINE_ROOT = os.path.join('.', 'my_pipeline_output')
DATA_PATH = os.path.join('.', 'data')
METADATA_PATH = os.path.join('.', 'tfx_metadata', PIPELINE_NAME, 'metadata.db')
ENABLE_CACHE = True


def create_pipeline(
    pipeline_name: Text,
    pipeline_root:Text,
    data_path: Text,
    enable_cache: bool,
    metadata_connection_config: Optional[
      metadata_store_pb2.ConnectionConfig] = None,
    beam_pipeline_args: Optional[List[Text]] = None
    ):
    components = []

    example_gen = CsvExampleGen(input=external_input(data_path))
    components.append(example_gen)
  
    return pipeline.Pipeline(
        pipeline_name=pipeline_name,
        pipeline_root=pipeline_root,
        components=components,
        enable_cache=enable_cache,
        metadata_connection_config=metadata_connection_config,
        beam_pipeline_args=beam_pipeline_args,
    )

def run_pipeline():
    my_pipeline = create_pipeline(
        pipeline_name=PIPELINE_NAME,
        pipeline_root=PIPELINE_ROOT,
        data_path=DATA_PATH,
        enable_cache=ENABLE_CACHE,
        metadata_connection_config=metadata.sqlite_metadata_connection_config(METADATA_PATH)
    )
  
    BeamDagRunner().run(my_pipeline)

if __name__ == '__main__':
    logging.set_verbosity(logging.INFO)
    run_pipeline()
~~~

~~~Bash
mkdir data
vim data/data.csv  # 샘플 데이터 추가
~~~

- CsvExampleGen 컴포넌트 추가
- 경로: data/
- create_pipeline(): tfx.orchestration.pipeline.Pipeline 생성
- run_pipeline(): 파이프라인 구동

~~~Bash
python run_pipeline.py
~~~
