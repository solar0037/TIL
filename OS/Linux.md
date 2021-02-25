# Arch Linux

> - 아치 리눅스
> - 설치
> - 패키지 관리자
>   - AUR
>   - debtap
> - 느낀 점

- [.deb 파일 아치 리눅스에 설치하기](https://blog.beom.dev/posts/install-dep-on-arch/)

## 아치 리눅스

- 가볍고, 커스터마이징이 쉬운 리눅스 배포판
- 설치부터 커맨드라인에서 하는 등 진입장벽이 높음
- [Arch Wiki](https://wiki.archlinux.org/)에 대부분의 내용이 상세하게 기록되어 있음

## 설치

- [Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide)를 따라하기
- 막히면 구글링 (오류 대부분은 검색 가능)
- 주의사항: usb로 부팅해 arch-chroot로 들어갈 때 패키지 미리 설치해 놓기

## 패키지 관리자

- pacman 이용
- `pacman -S [패키지]` - 설치
- `pacman -R [패키지]` - 삭제

### AUR

- [Arch User Repository](https://aur.archlinux.org/)
- 유저가 직접 올린 패키지가 있는 저장소
- yaourt, yay 등의 AUR 헬퍼 사용해 설치
- 바이러스 조심!

### debtap

- .deb 패키지를 Arch Linux에 설치할 수 있게 해주는 도구

```Bash
$ yay debtap
```

- yay를 이용해 debtap 설치

```Bash
$ debtap [package].deb
$ sudo pacman -S [package].pkg
```

- debtap을 이용해 .deb 패키지를 .pkg로 변환
- pacman으로 .pkg 패키지 설치

## 느낀 점

- 어렵다. 리눅스에 대해 아예 아무것도 모르는 상태라면 설치부터가 공포다.
- 그래도 Arch Wiki를 따라가다 보면 할 만 하다.
- Arch Wiki에 모든 과정에 링크를 달아놓는 등 위키 자체는 굉장히 친절하다.
- 리눅스 개발 환경이 빨리 필요한 상황이라서 Manjaro 쓰기로 결정

# PlayOnLinux

> - wine, PlayOnLinux
> - 카카오톡 설치
>   - xp버전 vs 신규버전
>   - PlayOnLinux 통해 설치하기
> - 결론

- [우분투 20.04 신 버전 카카오톡 설치하기 - XP버전 막히고 이후](https://tolovefeels.tistory.com/65)
- [우분투 카카오톡 설치](https://hiseon.me/linux/ubuntu/ubuntu-kakaotalk/)
- [Ubuntu 16.04 에서 카카오톡 설치하기](https://gist.github.com/BEMELON/70f173af95480389445b6f52c6ffada7)

## wine, PlayOnLinux

- wine: 윈도우 프로그램 사용 가능하게 하는 호환성 레이어 (가상 머신, 에뮬레이터 X)
- PlayOnLinux: wine을 위한 GUI

## 카카오톡 설치

### xp버전 vs 신규버전
- 현재 xp버전 사용 불가

### PlayOnLinux 통해 설치하기

- [우분투 20.04 신 버전 카카오톡 설치하기 - XP버전 막히고 이후](https://tolovefeels.tistory.com/65) 참고
- [우분투 카카오톡 설치](https://hiseon.me/linux/ubuntu/ubuntu-kakaotalk/) - 아이콘을 시스템 트레이에 정리
- [Ubuntu 16.04 에서 카카오톡 설치하기](https://gist.github.com/BEMELON/70f173af95480389445b6f52c6ffada7) - 50114(방화벽) 오류 해결

## 결론

- 돈을 벌어서 맥을 쓰자...

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

# SSH

> - 개요
> - 접속 형태
> - Key Pair
>   - 키페어 생성하기
> - 기타
>   - termux

- [SSH Key - 비밀번호 없이 로그인](https://opentutorials.org/module/432/3742)

## 개요

- SSH(Secure SHell): 원격지에 접속하게 해주는 프로그램/프로토콜
- Linux/macOS: ssh 이용
- Windows: PuTTY 이용 (최근에는 OpenSSH 기능 생김)
- SSH 포트: 22

## 접속 형태

- ssh [id]@[ip address] -p [port]

## Key Pair

- 비밀번호 대신 키페어 사용
- Public Key: 말 그대로 공개 키, 리모트 머신에 사용
- Private Key: 개인 키, 클라이언트에 사용
- 암호화 알고리즘을 rsa 등 선택 가능
- Linux/macOS: ssh-keygen
- Windows: PuTTYGen

### 키페어 생성하기

- 클라이언트

```Bash
$ ssh-keygen -t rsa  # id_rsa, id_rsa.pub 생성
$ scp id_rsa.pub [user]@[ip address]:id_rsa.pub  # id_rsa.pub을 원격지에 복사

$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/id_rsa
$ chmod 644 ~/.ssh/id_rsa.pub  # 원격지 authorized_keys에 넣는 것 이외에는 필요 없는 파일인 듯?
$ chmod 644 ~/.ssh/known_hosts
```

- 서버

```Bash
$ cat id_rsa.pub >> ~/.ssh/authorized_keys  # id_rsa.pub 대신에 authorized_keys에 있어야 함
$ chmod 700 ~/.ssh
$ chmod 644 ~/.ssh/authorized_keys
```

## 기타

### termux

- ``whoami``를 사용해서 u0_a***가 나옴 -> 접속 시도 -> 실패 (no known hosts)?
- ssh [ip 주소] -p 8022로 접속 가능 (termux 기본 포트: 8022)

# 시작 스크립트

> - GNOME

- [How to run scripts on start up?](https://askubuntu.com/questions/814/how-to-run-scripts-on-start-up)

## GNOME

- 스크립트 등록하기
- System > Preferences > Startup Applications (시작 애플리케이션)
- 명령 등록하기
