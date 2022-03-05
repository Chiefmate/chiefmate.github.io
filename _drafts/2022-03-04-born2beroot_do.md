---
title: "Born2beroot 과제수행"
tags: 42seoul
layout: posts
---

Born2beroot 과제 수행과정에 따라 적는다.

---
# 과제 명세

1. `VirtualBox` 또는 `UTM` 사용 필수
1. 전체 가상머신의 checksum을 가지는 `signature.txt` 파일 제출

1. 그래픽 인터페이스 (X.org 등) 설치 금지
1. 최신 Stable 버전의 `Debian` 또는 `CentOS` 설치 (testing/unstable 금지)
1. `LVM` 사용하여 최소 2개의 암호화된 파티션 생성해야 함
1. `SSH`는 포트 4242만을 사용해야함
1. `SSH`를 사용하여 root에 접근하는 것은 금지되어야 함
1. `UFW 방화벽`을 이용하여 포트 4242만을 남겨놓아야 함
1. 가상머신의 `hostname`은 intraID 뒤에 42가 붙는 이름으로 정한다.
1. 가상머신의 `hostname`을 수정할 줄 알아야 한다.
1. `강한 암호 정책`을 적용해야 한다.

1. 엄격한 규칙에 따라 `sudo`를 설치하고 설정하여야 한다.
1. `root user` 뿐만 아니라, 본인의 intraID를 username으로 가지는 유저도 존재해야 한다.
1. 이 유저는 user42그룹과 sudo 그룹에 속한다.
1. 새로운 유저를 만들고 그룹에 배정할 줄 알아야 한다.

## 강한 암호 정책 요구사항
1. 암호는 30일마다 만료되어야 함
1. 암호 변경에 필요한 최소 기간은 2일로 지정
1. 암호 만료 7일전에 유저가 경고 메시지를 받도록 해야함
1. 암호는 최소 10자 이상. 대문자와 숫자를 반드시 포함. 같은 글자는 연속하여 3번 이상 올 수 없음
1. 유저의 이름 포함해서는 안됨
1. 새로운 암호는 이전 암호의 일부가 아닌 부분을 7자 이상 가져야함: 이 규칙은 `root user`에게는 적용되지 않음.

[VirtualBox 매뉴얼, Security Guide](https://www.virtualbox.org/manual/ch13.html)

## `sudo` 그룹 설정 요구사항
1. `sudo`를 이용한 인증에서, 암호가 틀리는 것은 최대 3번까지로 제한됨
1. `sudo`를 이용할 때 암호가 틀릴 경우, 본인이 선택한 문구의 에러 메시지가 표시되어야 함
1. `sudo`를 이용한 각 행동은 입력과 출력 모두 기록될 것. 로그 파일은 `/var/log/sudo/` 폴더에 저장됨.
1. 보안을 위하여 `TTY` 모드가 활성화 되어야 함
1. 보안을 위하여 `sudo`에 의해 사용될 수 있는 경로는 제한되어야 함. 예시: `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`

## `monitoring.sh` 작성
1. `bash`로 개발된 스크립트 `monitoring.sh`를 작성해야 함
서버가 시작할 때, 아래의 정보들을 10분마다 모든 터미널에 표시할 것임. (`wall` 참고)
배너는 선택사항임. 에러는 표시되어서는 안 됨.

스크립트는 아래의 정보들을 표시하여야 함.
1. OS의 아키텍처와 커널 버전
1. 물리적 프로세서 개수
1. 가상 프로세서 개수
1. 서버의 현재 가용한 RAM과 활용중인 용량비율(%)
1. 서버의 현재 가용한 메모리와 활용중인 용량비율(%)
1. 현재 프로세서의 활용 비율(%)
1. 가장 최근의 재부팅 날짜와 시각
1. LVM이 활성화되어있는지 여부
1. 활성화된 연결의 개수
1. 서버를 사용중인 유저의 수
1. 서버의 IPv4 주소와 MAC(Media Access Control) 주소
1. `sudo`로 실행된 명령의 수

아래는 예시.
```
Broadcast message from root@wil (tty1) (Sun Apr 25 15:45:00 2021):
#Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (39%)
#CPU load: 6.7%
#Last boot: 2021-04-25 14:45
#LVM use: yes
#Connexions TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 42 cmd
```

## 요구사항 일부를 체크하기 위한 명령어
CentOS의 경우:
```sh
head -n 2 /etc/os-release   # OS
sestatus                    # SELinux
ss -tunlp                   # network
ufw status                  # UFW firewall
```

Debian의 경우:
```sh
head -n 2 /etc/os-release   # OS
/usr/sbin/aa-status         # AppArmor
ss-tunlp                    # network
/usr/sbin/ufw status        # ufw check
```

---
# 진행하기

VirtualBox 실행
상단 New 버튼

## VirtualBox 조작 팁
디스플레이 사이즈 조정
`cmd + c` 로 스케일 모드로 들어가면 됨

`/goinfre/(intraID)` 에 설치

[한국 데비안 사용자 모임, 버추얼 박스에 데비안 설치하기](https://wiki.debianusers.or.kr/index.php?title=%EB%B2%84%EC%B6%94%EC%96%BC%EB%B0%95%EC%8A%A4%EC%97%90_%EB%8D%B0%EB%B9%84%EC%95%88_%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

Debian OS iso파일 다운받은 후, 마운트하고 부팅시키기
[iso파일 마운트하기](https://pureinfotech.com/mount-iso-virtual-machine-virtualbox/)

hostname 원하는대로 `hyunhole42` 해주고,
domain은 빈 칸
루트 암호는 `Intel123`으로 해줬다

새로운 유저 과제에 제시된대로 `hyunhole` 유저도 만들었다. 암호는 `Intel123`

파티션

