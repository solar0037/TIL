# 셔뱅 (Shebang)

> - 스크립트 언어
> - 셔뱅
> - /usr/bin/env

- [Unix/Linux: Shebang과 env에 대한 설명 (#!/usr/bin/env)](https://blog.gaerae.com/2015/10/what-is-the-preferred-bash-shebang.html)

## 스크립트 언어

- 컴파일하지 않고 인터프리터가 즉시 줄마다 해석하는 언어
- 개발 편의성은 높지만 속도가 느림
- 셸을 열어서 명령어를 칠 때도 인터프리터가 해석 (셸 스크립트도 존재)
- [인터프리터 이름] [파일 이름]으로 실행해야 하는데 더 간단한 방법은 없을까?

## 셔뱅

- 스크립트의 첫 줄에 `#!`로 시작하는 것
- 스크립트를 실행할 인터프리터 선택
- 즉, 스크립트를 실행할 때 앞에 인터프리터 이름 (python, bash, node, ...)을 붙일 필요가 없음

```Python
#!/usr/bin/python
```

- /usr/bin/python으로 실행할 스크립트
- ``python test.py`` 대신 ``./test.py``로 실행 가능
- 처음에 ``chmod +x test.py``로 실행 권한 부여 필요
- 각 시스템마다 환경이 다른데 /usr/bin/python으로 고정할 필요가 있을까?

## /usr/bin/env

- env: 시스템의 환경변수 출력
- env [인터프리터 이름]: 환경에 지정된 인터프리터 실행
- /usr/bin/env를 통해 환경마다 다르게 설정된 인터프리터 사용 가능

```Python
#!/usr/bin/env python
```
