# 설치

> - Arch Linux

- [Install Visual Studio Code Arch Linux](https://linuxhint.com/install_visual_studio_code_arch_linux/)

## Arch Linux

- 아치 리눅스용 오픈소스 배포판이 존재 (Code - OSS (code))
- 대신에 AUR로 Microsoft 브랜드 버전 설치 가능

```Bash
$ git clone https://AUR.archlinux.org/visual-studio-code-bin.git
$ cd visual-studio-code-bin
$ makepkg -s
$ sudo pacman -U visual-studio-code-bin-*.pkg.tar.xz  # .pkg.tar.zst
```

- AUR에서 클론 후 makepkg로 패키지, pacman으로 설치
