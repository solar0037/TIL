# Neovim

> - 개요
> - 특징

- [MacOS 환경에서 vim 환경 설정](https://woonizzooni.tistory.com/entry/MacOS-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-vim-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95)

## 개요

- Vim이라는 오래된 에디터
- 소스코드를 정리하고 기능을 개선하자!
- Neovim 탄생

## 특징

- neovim 패키지 설치

```Bash
sudo apt install neovim
```

- nvim 명령어로 실행
- ~/.vimrc 대신 ~/.config/nvim/init.vim 파일 사용
- ~/.local/share/nvim에 플러그인 등 저장, `echo stdpath('data')`으로 구할 수도 있음
- Vim Script 대신 Lua도 사용 가능

```bash
alias vim=nvim
alias vi=nvim
alias vimdiff="nvim -d"
export EDITOR=/usr/bin/nvim  # $EDITOR가 설정되어 있는 경우
```

- alias 등록해 vim 대신 nvim 쓰기
