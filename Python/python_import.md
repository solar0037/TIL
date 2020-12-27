# import

> - 모듈
> - 패키지
> - sys.path, PYTHONPATH

- [Python Module and Packages](https://jun-choi-4928.medium.com/python-module-and-packages-afc6a7d870b)
- [sys.path, PYTHONPATH: 파이썬 파일 탐색 경로](https://www.bangseongbeom.com/sys-path-pythonpath.html)

## 모듈
### 모듈 (module)

- 함수, 클래스 등을 모아둔 파일
- .py 확장자

~~~Python
# main.py
# lib.py 모듈 사용
import lib
from lib import func
# 되도록이면 쓰지 말기 (PEP 8)
# 네임스페이스에 정확히 무엇이 있는지 모르게 됨
from lib import *
~~~

## 패키지
### 패키지 (package)

- 모듈을 디렉터리 형태로 구조화
- 모듈은 점(.)으로 구분됨
- 패키지도 하나의 모듈임
- 디렉터리마다 `__init__`.py 필요
- PEP420: python3.3부터 `__init__`.py 없어도 패키지로 인식

~~~
robot
  |  -  brain
  |  |  - __init__.py
  |  |  - neuron.py
  |  |  - network.py
  |  - body
  |  |  - __init__.py
  |  |  - arm.py
  |  |  - leg.py
  |  - __init__.py
  |  - poweron.py
~~~

~~~Python
# 가능한 임포트 방식
import robot
import robot.brain
import robot.brain.neuron
from robot import brain
from robot.brain import neuron
from robot.brain.neuron import Neuron
~~~

### 절대 임포트 vs 상대 임포트

- 절대 임포트 (absolute imports): 패키지 전체 경로
  - 어디서든지 사용 가능
  - 명확 but 복잡
- 상대 임포트 (relative imports): 점(.) 사용
  - 스크립트로 실행할 시 에러 (패키지 내부에서만 사용 가능)
  - 간결 but 불명확

~~~Python
# robot/brain/network.py
from robot.brain import neuron
from robot.brain.neuron import Neuron
~~~

~~~Python
# robot/brain/network.py
from . import neuron
from .neuron import Neuron
~~~

## sys.path, PYTHONPATH

### 모듈 찾기

- import시 인터프리터가 찾는 경로 필요
- 적절히 수정해 임의의 위치에서 import 가능

### sys.path

- 인터프리터: 현재 디렉터리
- 파일: 파일이 속한 절대경로

~~~Python
>>> import sys
>>> print(sys.path)
['', ...]
~~~

~~~Python
import sys
print(sys.path)
# ['/home/ubuntu', ...]
~~~

### sys.path 이용해 임포트하기

~~~
examples
|  - example_run.py

robot
  |  - brain
  |  |  - __init__.py
  |  |  - neuron.py
  |  |  - network.py
  |  __init__.py
  |  poweron.py
~~~

~~~Python
# examples/example_run.py
import os
import sys
# 프로젝트의 루트 디렉터리를 sys.path에 추가
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
from robot import poweron
~~~

### PYTHONPATH

- sys.path에 추가할 경로를 담은 환경변수

~~~Bash
PYTHONPATH=/learn:/til
export PYTHONPATH
~~~

~~~Python
import sys
print(sys.path)
# [..., '/learn', '/til', ...]
~~~
