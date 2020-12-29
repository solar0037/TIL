# 설치

> - Ruby
> - 설치하기
> - IDE

- [위키백과 - 루비 (프로그래밍 언어)](https://ko.wikipedia.org/wiki/%EB%A3%A8%EB%B9%84_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4))
- [rbenv/rbenv](https://github.com/rbenv/rbenv)
- [rbenv/rbenv-installer](https://github.com/rbenv/rbenv-installer)
- [rbenv/ruby-build](https://github.com/rbenv/ruby-build)

## Ruby

- 마츠모토 유키히로가 개발한 동적 객체 지향 스크립트 프로그래밍 언어
- 문법이 간결 (파이썬 형제 언어?)
- Ruby on Rails 웹 프레임워크 (다른 용도로는 잘 사용되지 않는 듯)

## 설치하기

### rbenv
- rbenv: 루비 버젼 관리자

- rbenv-installer 이용하기
~~~Bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer | bash  # 안 써봐서 bash를 다른 셸로 바꿔도 되는지는 모르겠습니다
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
~~~

- 직접 설치하기
~~~Bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src  # 선택사항 (성능 향상)
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc  # ~/.bash_profile, ~/.zshrc 등으로 바꾸기
~/.rbenv/bin/rbenv init  # 출력된 설명 따르기
# 셸 재시작
~~~

~~~Bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash  # zsh를 써도 bash로 실행하기
~~~

### ruby-build
- ruby-build: ruby install로 루비를 쉽게 설치할 수 있도록 하는 rbenv 플러그인
- gcc, make, libssl-dev (Fedora: openssl-devel) 필요

~~~Bash
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
~~~

- ruby-build로 루비 설치하기

~~~Bash
rbenv install -l  # 설치 가능한 버전 목록
rbenv install [version]  # [version]의 루비 설치하기
~~~

## IDE / 코드 에디터

- RubyMine - JetBrains 개발, 유료, 가장 기능이 풍부
- Visual Studio Code - Microsoft 개발, 오픈 소스, Ruby와 Ruby Solargraph 확장
- Atom
- Vim
- Emacs
- 등등
