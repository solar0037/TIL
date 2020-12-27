# 윈도우 파티션

> - 삽질
> - 우분투 파티션과 GRUB 찌거기 제거하기

- [Windows 10/Linux Dual Boot에서 Linux 완전히 삭제하기](https://m.blog.naver.com/PostView.nhn?blogId=zeta0807&logNo=221527180909&proxyReferer=https:%2F%2Fwww.google.com%2F)

## 삽질
### 우분투 사망

- 우분투를 사용하던 중 Gnome 사망
- 복구 불가 (실력이 안 되기도 하고 XWindow는 잘 몰랐다)
- 어쩔 수 없이 포맷 ㅠ
- 문제는 끝나지 않았다

### 우분투 재설치

- GRUB을 설치할 수 없다는 메세지
- EFI 파티션이 없다는 메세지
- '서드파티 소프트웨어 함께 설치'를 해제하고 나서야 비로소 성공 (블루투스 등 동작하지 않는 점 때문에 사용 포기)

## 우분투 파티션과 GRUB 찌거기 제거하기
### 우분투 파티션 제거

- 윈도우 검색 ([Win]+s) -> 하드 디스크 파티션 만들기 및 포맷 -> 우분투 파티션 우클릭 -> 볼륨 삭제

### EFI 파티션

- (아직은 운영체제 공부가 더 필요하다. 나중에 채울 지도?)
- 부팅시 사용하는 것(?). 'EFI 시스템 파티션'이라고 적혀 있음
- 우분투를 설치하고 나니 EFI 파티션에 ubuntu 디렉터리 남아 있음
- Windows에서는 diskpart를 이용해 제거

### diskpart

- cmd 또는 PowerShell에서 diskpart 실행
- list disk/partition: 디스크나 파티션 보여주기
- select disk/partition [num]: [num]번 디스크나 파티션 선택
- assign letter=[char]: 파티션에 [char] 문자 할당 (cmd나 PowerShell에서 파티션으로 이동할 수 있게 해준다)
- remove letter=[char]: 파티션에서 [char] 문자 제거
- EFI 파티션으로 이동하려면 터미널을 관리자 권한으로 실행

~~~PowerShell
C:> diskpart
DISKPART> list disk

  디스크 ###  상태           크기     사용 가능     Dyn  Gpt
  ----------  -------------  -------  ------------  ---  ---
  디스크 0    온라인        238 GB       1024 KB        *

DISKPART> select disk 0

0 디스크가 선택한 디스크입니다.

DISKPART> list partition

  파티션 ###  종류              크기     오프셋
  ----------  ----------------  -------  -------
  파티션 1    시스템                260 MB  1024 KB
  파티션 2    예약됨                 16 MB   261 MB
  파티션 3    주                  220 GB   277 MB
  파티션 4    복구                 850 MB   220 GB
  파티션 5    복구                  16 GB   221 GB
  파티션 6    복구                1024 MB   237 GB

DISKPART> select partition 1

1 파티션이 선택한 파티션입니다.

DISKPART> assign letter=x

DiskPart에서 드라이브 문자 또는 탑재 지점을 할당했습니다.

DISKPART> exit

DiskPart 마치는 중...
C:> x:
X:> dir


    디렉터리: X:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2020-12-27   오전 3:13                EFI

X:> cd .\EFI\
X:\EFI> dir


    디렉터리: X:\EFI


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2020-11-25   오후 8:06                Microsoft
d-----      2020-12-21   오후 1:05                Boot
d-----      2020-12-25   오전 11:32               ubuntu


X:\EFI> Remove-Item .\ubuntu\ -recurse
X:\EFI> exit
~~~
