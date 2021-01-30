# docker-compose

> - 이전의 워크플로우
> - docker-compose

## 이전의 워크플로우

- `docker build`로 이미지 빌드
- `docker run`으로 이미지를 컨테이너에서 실행

```Shell
docker build -t [image] .
docker run [image] -p [port]:[port]
```

- 이러한 과정을 매번 직접 입력하기?
- 다른 컨테이너를 실행하기 위해 여러번 입력하기?

## docker-compose

- YAML 파일로 작성
- `services` 필드 아래에 빌드 정보 등 기술

```YAML
version: "3"
services:
  web:
    build: . # 소스에서 빌드하는 대신에 image를 직접 사용해도 되는 듯?
    ports:
      - "PORT:PORT"
```

- `docker-compose build`로 빌드
- `docker-compose up`로 실행 (-d: 데몬)
- `docker-compose down`로 종료
