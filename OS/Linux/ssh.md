# SSH

> - 개요
> - 접속 형태
> - Key Pair
> - 기타

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

## 기타

### termux

- ``whoami``를 사용해서 u0_a***가 나옴 -> 접속 시도 -> 실패 (no known hosts)?
- ssh [ip 주소] -p 8022로 접속 가능 (termux 기본 포트: 8022)
