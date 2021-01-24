# Bundler

> - 패키지 매니저 (의존성 관리자)
> - Bundler

- [Jekyll on Ubuntu](https://jekyllrb.com/docs/installation/ubuntu/)

## 패키지 매니저 (의존성 관리자)

- 프로젝트 별로 의존성이 다름
- 패키지를 전역에 설치 -> 충돌 가능성
- 프로젝트 별로 다른 버전을 설치할 수 있게 해주는 장치

## Bundler

- Ruby의 의존성 관리자
- 필수 패키지: ruby-full, build-essential, zlib1g-dev

```Bash
# Install Ruby Gems to ~/gems
export GEM_HOME="$HOME/gems"
# export PATH="$HOME/gems/bin:$PATH"
export PATH="$PATH:$HOME/gems/bin"
```

- 시스템(root권한)이 아니라 GEM_HOME에 설치
- GEM_HOME에 설치된 gem을 실행할 수 있게 PATH에 추가

```Bash
gem install bundler
```

- bundler 설치
