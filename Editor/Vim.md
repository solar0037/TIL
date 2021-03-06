# 플러그인

> - 플러그인
> - 플러그인 매니저
>   - Vundle
> - 언어
> - 기타

- [VIM and Python - A Match Made in Heaven](https://realpython.com/vim-and-python-a-match-made-in-heaven/)

## 플러그인

- Vim 본전 뽑는 법
- 자동완성, 린팅, 유틸리티 등 지원
- 깃허브에서 오픈소스 플러그인을 가져와 사용하기
- 자신이 직접 작성하는 것도 가능
- Vim Script로 작성. 하지만 단순한 매크로를 위한 언어. (Neovim은 Lua 사용)
- Emacs는 Emacs Lisp 사용하여 확장성 더 높음.

## 플러그인 매니저

- Vundle, Vim-Plug, Pathogen 등

### Vundle

- Vim의 플러그인 매니저
- ~/.vimrc를 수정하는 것으로 간단하게 플러그인 설치 가능
- 플러그인은 .vim 확장자 Vim Script로 작성됨
- [Quick Start](https://github.com/VundleVim/Vundle.vim#quick-start) 따라하기

## 언어

- Valloric/YouCompleteMe - C, Python, Java, JavaScript 등 자동완성 지원
- dense-analysis/ale - 비동기식 linting 지원

## 기타

- powerline/powerline - 하단부 상태바 꾸미기
  - set laststatus=2 - 창 하나 띄울 때도 powerline 띄우기
- iamcco/markdown-preview.nvim - 마크다운 브라우저로 미리보기
- preservim/nerdtree - tree 파일 탐색기
- tpope/vim-fugitive - git 지원

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

# 커맨드라인에서 vi 사용하기

- ~/.bash_profile, ~/.bashrc 등에

```Shell
set -o vi
```
