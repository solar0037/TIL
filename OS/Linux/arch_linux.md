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
