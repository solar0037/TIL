# Airflow

> - 개요
> - 파이프라인 컴포넌트
> - 설치

- [TFX Airflow Tutorial](https://www.tensorflow.org/tfx/tutorials/tfx/airflow_workshop?hl=en)

## 개요

- 머신러닝 파이프라인을 구축하는 데 도움을 주는 프레임워크
- Apache Airflow, Apache Beam, Kubeflow 등
- DAG (Directed Acyclic Graph) == TFX 파이프라인

## 파이프라인 컴포넌트

- ExampleGen
- -> StatisticsGen
- -> SchemaGen
- -> ExampleValidator
- -> Transform
- -> Trainer
- -> Evaluator
- -> InfraValidator
- -> Pusher

## 설치

- 기본 패키지 설치
- Linux (WSL에서도 동작)
~~~Bash
sudo apt-get install \
    build-essential libssl-dev libffi-dev \
    libxml2-dev libxslt1-dev zlib1g-dev \
    python3-pip git software-properties-common
~~~

- TFX, Apache Airflow 설치

~~~Bash
cd
virtualenv -p python3 tfx-env
source ~/tfx-env/bin/activate

git clone https://github.com/tensorflow/tfx.git
cd ~/tfx
# These instructions are specific to the 0.21 release
git checkout -f origin/r0.21
cd ~/tfx/tfx/examples/airflow_workshop/setup
./setup_demo.sh
# 스크립트로 제대로 설치되지 않으면 pip로 tfx, apache-airflow 직접 설치하기
# pip install tfx apache-airflow
~~~
