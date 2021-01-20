# DVC 개념

> - 개요
> - DVC (Data Version Control)
> - 설치
> - 기초 명렁어

- [DVC - Get Started](https://dvc.org/doc/start)
- [How do I use DVC with SSH remote](https://discuss.dvc.org/t/how-do-i-use-dvc-with-ssh-remote/279/2)
- [DVC - Data version control](https://inahjeon.github.io/dvc/)

## 개요

- git: 소스 코드 버전 관리 도구
- but 머신러닝은 데이터와 모델이 추가로 존재
- 어떤 식으로 버전 관리를 할 것인가?
- -> DVC 등장

## DVC (Data Version Control)

- 데이터용 git
- 데이터를 직접 git으로 관리하지 않음
- 대신에 .dvc 파일 생성해 git으로 관리
- 데이터는 /.gitignore에 추가됨
- git과는 별도로 관리됨 (.dvc 파일 commit 필요)

## 설치

- pip, conda 등으로 파이썬 라이브러리로 설치 가능
- 이외에도 chocolatey, snap, apt, brew 등

## 기초 명령어

### dvc init

- 현재 디렉터리에 dvc를 초기화
- .dvc 디렉터리 생성
- .dvc/.gitignore에 추가되는 파일도 있음 (lock, cache, config.local 등)

### dvc remote

- local, s3, gs, azure, ssh, hdfs, http 등 다양한 원격 저장소 지정 가능
- dvc remote add: dvc에 데이터 추가

ssh 예제

```Bash
dvc remote add -d ssh-storage ssh://example.com/data
dvc remote modify ssh-storage user solar0037
dvc remote modify ssh-storage port 22
dvc remote modify --local ssh-storage password asdf1234
```

- password는 --local로 지정해야 함 (아니면 git remote에 올라가 비밀번호 유출!?)

### dvc add

- 파일들은 .gitignore에 추가되고 dvc에서 관리
- .dvc 파일 추가됨 (이 파일로 dvc remote에서 데이터셋을 다운받을 수 있다)

### dvc push

- remote로 푸시

### dvc pull

- remote에서 받아오기
