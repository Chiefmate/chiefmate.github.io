---
title: "Born2beroot 배경지식 정리"
tags: 42seoul
layout: posts
---

# 네트워크 기본 개념 정리
---
[서버란?](https://lipcoder.tistory.com/514)<br>
[네트워크 기초 지식](https://lipcoder.tistory.com/515?category=908023)<br>


# 모르는 단어 나열하기
---
1. VirtualBox vs. UTM
1. signature.txt
1. Debian vs. CentOS
1. apt vs. aptitude
1. SELinux vs. AppArmor
1. lvm
1. 리눅스 사용자(User)
1. sudo
1. ssh vs. telnet
1. UFW firewall
1. cron
1. IPv4 주소와 MAC(Media Access Control)주소
1. GRUB 부트로더


# VirtualBox vs. UTM
---
가상 머신 Virtual Machine

## 가상머신 용어 정리
1. 에뮬레이션Emulation
모든 부품의 모든 기능을 소프트웨어적으로 구현함. 속도는 가장 느리지만 범용성은 가장 뛰어남
1. 가상화Virtualization
CPU 등 주요 부품의 기능에서 하드웨어의 기능을 지원 받음. 속도는 빠르지만 해당 하드웨어에 종속적이므로 범용성이 떨어짐. 예를 들어 CPU를 가상화하면 실제 CPU가 처리하는 기계어 세트(ISA)에서 크게 벗어날 수 없음. M1 맥에서 x86 윈도를 돌릴 수 없는 이유
1. 반가상화Paravirtualization
기반이 되는 하드웨어, 소프트웨어와 동일하지는 않지만 비슷한 가상 머신에 대한 소프트웨어 인터페이스를 제공하는 기술. 완전한 에뮬레이션/가상화를 포기하지만, 속도는 가장 빠름. 그러나 운영체제와 드라이버에 종속되어서 범용성은 가장 떨어짐. Hypervisor를 이용. 가상머신에 설치되는 OS를 수정하거나 전용 드라이버를 이용하여 하드웨어에 직접 접근함.

## 가상머신 사용 목적
1. 하나의 시스템에서 서로 다른 2개 이상의 운영체제 실행
1. 하나의 시스템 자원을 여러 사용자에게 나누어 주는 상황에서, 상호 간섭을 없애기 위함. 클라우드의 가상머신이 예시
1. 컴퓨터의 다른 부분에 영향을 주지 않는 독립 환경을 만들고자 할 때. 악성 코드 분석할 때도 사용함

출처: [가상머신을 사용하는 이유](https://inpages.tistory.com/86)

## VDI
Virtual Disk Image

버츄얼 박스는 3가지 포맷으로 하드디스크를 에뮬레이션함:
native VDI, VMware의 VMDK, MS 윈도우의 VHD


# signature.txt
---
과제에서 평가를 위해 Virtual disk의 signature를 저장하라고 함.
일단 [lvm파트](#lvm)를 읽어보자.


# Debian vs. CentOS
---
리눅스 배포판 Linux Distribution (Distro)

|리눅스 배포판 				|Debian					|CentOS			|
|:---------------------:	| :-----------: 		| :--------------:|
| 							|데비안 계열			|레드햇 계열	|
|리눅스 보안모듈 (LSM)		|AppArmor				|SELinux		|
|파일 시스템 기본값			|XFS					|EXT4			|
|패키지 관리				|패키지 포맷: RPM, 패키지 매니저: YUM/DNF	|패키지 포맷: DEB, 패키지 매니저: dpkg/APT|
|							|RHEL 포크 버전이므로 안정성 때문에 인기 많음	|커뮤니티가 오래되어 강점	|

데비안 계열의 배포판으로는 우분투Ubuntu가 있다. CentOS와 달리 우분투는 GUI를 지원하고 캐노니컬 재단에 의해 관리되어 많은 사람들이 사용하고 있다.

레드햇 계열의 배포판으로는 Red Hat의 커뮤니티 버전 Fedora와 CentOS 등이 있다.
Red Hat이라는 기업이 관리하는 리눅스 배포판이 Red Hat Enterprise Linux (RHEL)이다. 해당 리눅스를 이용하면 Red Hat에서 유지 보수를 지원하기 때문에 기업에서 선호한다고 한다.
CentOS는 레드햇 리눅스(RHEL)를 분기fork하여 상표를 제거한 것이다. 그러나 곧 CentOS 지원을 종료하고 CentOS Stream에 집중할 예정이라고 한다. 기존 CentOS가 RHEL을 바탕으로 포크하여 만들어졌던 것과 달리, CentOS Stream은 신기능을 빠르게 탑재하여 개발한 뒤 이것을 바탕으로 RHEL이 개발되게 된다. 이에 따라 기존 CentOS의 안정성과 신뢰성을 담보할 수 없게 되었다. 대안으로 Rocky Linux, Oracle Linux 등이 거론되고 있다.

[educba.com CentOS vs. Debian](https://www.educba.com/centos-vs-debian/)<br>
[리눅스 파일시스템 비교 ext4 vs. xfs](https://m.blog.naver.com/hymne/220976678541)


# apt vs. aptitude
---
APT: Advanced Packaging Tool<br>
데비안 계열의 시스템에서는 패키지 매니저 apt를 이용하여 소프트웨어를 설치한다.

apt = apt-get는 low-level vs. aptitude는 high-level

apt-get은 apt의 도구 중 하나다. 패키지를 설치, 제거, 업데이트 할 때 사용한다.
이외에도 apt-cache, dpkg, dselect 등이 있다.

그러나 종속성이 있는 패키지의 경우, apt-get으로 제거하게 되면 해당 패키지만 제거하고, 그것에 종속적인 패키지들(orphaned dependencies라고 한다)은 그냥 두게 된다. aptitude는 이를 해결해준다.
apt-get autoremove가 이를 해결하기 위해 등장했지만, 이외에도 aptitude는 인터페이스 등 다양한 기능들을 제공한다. apt가 16개의 tool이 존재하는데 반해, aptitude는 aptitude라는 하나의 명령어로 모든 기능을 작동할 수 있다.

[Linuxmint 커뮤니티, aptitude vs. apt vs. apt-get](http://linuxmint.kr/Etc/2062)


# SELinux vs. AppArmor
---
리눅스 보안 모듈 Linux Security Modules (LSM)

응용프로그램Applications, 프로세스processes, 파일files에 접근할 수 있는 권한을 정의하고, 보안 정책security policies에 따라 접근을 제어하는 보안 프레임 워크.
리눅스 커널이 단일한 보안 구현이 아닌 다양한 보안 모델을 지원할 수 있도록 함.
AppArmor, SELinux, Smack, TOMOYO 리눅스 등이 있다.


## AppArmor와 SELinux의 공통점: MAC vs. DAC
강제적 접근 통제 (MAC, mandatory access control) 시스템을 리눅스에 제공한다.<br>
전통적인 유닉스와 리눅스는 임의적 접근 통제 (DAC, discretionary access control) 방식을 사용해왔다. DAC 시스템에서는 파일과 프로세스들은 **'소유자owner'**가 존재한다. 각 소유자가 파일에 대한 권한을 변경할 수 있는 재량이 있다.

AppArmor와 SELinux는 리눅스에게 MAC 시스템을 제공한다.<br>
MAC 시스템에서는, 접근에 대한 관리 **정책policy**이 존재한다. DAC에서 사용자와 소유권에만 기반하는 것에 반해, MAC에서는 정의된 정책을 활용하여 접근 권한을 제어한다. 이를 통해 파일의 유형, 사용자의 역할, 프로그램의 기능과 신뢰도, 데이터의 민감성 무결성 등을 고려하여 보안을 유지할 수 있게 된다.

## AppArmor와 SELinux의 차이점
- file system labels: 파일 경로 vs. 아이노드inode<br>
AppArmor는 policy files에만 기초하는데 반해, SELinux는 polic-file과 right file system labels가 필요하다. SELinux는 경로 대신 아이노드inode(리눅스 파일 고유번호)에 기반하여 파일 시스템 객체들을 구별하지만, AppArmor는 파일 경로(file tree)를 이용한다.

- AppArmor가 더 관리하기 쉽다.<br>
label 시스템의 차이 때문에, 배우는 것이 더 쉽고 더 적은 수정을 요구한다.


[위키백과, 리눅스 보안 모듈](https://ko.wikipedia.org/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B3%B4%EC%95%88_%EB%AA%A8%EB%93%88)<br>
[Red Hat, What is SELinux?](https://www.redhat.com/en/topics/linux/what-is-selinux)<br>
[AppArmor wiki](https://gitlab.com/apparmor/apparmor/-/wikis/home)<br>
[linuxreviews.org, AppArmor 설명](https://linuxreviews.org/AppArmor)<br>
[linuxhint, Debian AppArmor 튜토리얼](https://linuxhint.com/debian_apparmor_tutorial/)<br>

<!--
#lvm
-->
# lvm
---
LVM: Logical Volume Manager

LVM은 리눅스 커널에 구현된 기능으로, 디스크를 볼륨 단위로 관리하여 효율적으로 사용할 수 있도록 한다.
유용한 용량 조절, 크기 조정이 가능한 스토리지 풀(Pool), 편의에 따른 장치 이름 지정, 디스크 스트라이핑, 미러 볼륨 등을 제공한다.

개인사용자의 경우 디스크의 공간이 부족하면, 디스크를 갈아끼우면 그만이지만, 서버의 경우에는 거대한 디스크의 데이터를 백업후 옮기는 일이 무척 어렵다. LVM을 사용하면 **기존 사용중인 디스크의 공간을 확장**하는 것이 훨씬 용이해진다.

## 파티션, 볼륨
물리 디스크는 파티션이라는 논리적 개념으로 분할/통합하여 사용한다.
1. 하나의 디스크 → 하나의 파티션
1. 하나의 디스크 → 여러개의 파티션
1. 여러개의 디스크 → 하나의 파티션
모두 가능하다.

파티션은 **고정적**이고 물리적인 개념이 강하다. OS는 각 파티션을 별도의 디스크처럼 인식하고, 파티션을 한번 정하면 변경하기 어렵다.

볼륨은 파일 시스템으로 포맷된 디스크 상의 저장 영역.

볼륨은 파티션보다 논리적이고 **유동적**인 개념이다.
1. 하나의 파티션 → 하나의 볼륨
1. 여러개의 파티션 → 하나의 볼륨
이 가능하다.


## PV, PE, VG, LV, LE

1. 물리 볼륨(PV, Physical Volume), PE(Physical Extent)
1. 볼륨 그룹(VG, Volume Group)
1. 논리 볼륨(LV, Logical Volume), LE(Logical Extent)

PV는 물리적 공간이다.
섹터 ⊂ 물리디스크인 것 처럼, PE ⊂ PV이다.
실제 물리 디스크가 각 섹터가 모여 하나의 디스크를 구성하듯이, LVM 상에서는 PE라는 단위로 모여 PV로 구성된다. 서로 다른 디스크를 합치기 위해 기본 단위를 PE로 통일하는 것이다.

VG는 PV를 합쳐 구성한다. 단순히 여러개의 PV를 그룹으로 모으면 VG가 된다.

이후 VG를 필요에 따라 나누어 LV로 만들어 사용한다.
LV에서 사용되는 디스크 기본단위는 PE가 아니라 LE가 된다.

현재 사용되는 LVM은 LVM2 인데, PE와 LE 모두 기본값이 4MB라고 한다.

정리하면<br>
```
PE ⊂ PV ⊂ VG<br>
LE ⊂ LV ⊂ VG
```

[리눅스 LVM과 RAID 개념](https://wiseworld.tistory.com/32)<br>
[농심클라우드, LVM 개념](https://tech.cloud.nongshim.co.kr/2018/11/23/lvmlogical-volume-manager-1-%EA%B0%9C%EB%85%90/)<br>
[LVM이란? 매우 쉽게](https://mamu2830.blogspot.com/2019/12/lvmpv-vg-lv-pe-lvm.html)<br>

# 리눅스 사용자 (User)
---
리눅스를 포함하여 유닉스 계열 OS의 장점 중 하나는 멀티 유저 환경을 제공한다는 것이다. 유닉스 계열 OS를 통해 한 시스템을 여러명의 사용자가 동시에 사용할 수 있다.

Debian의 경우 `adduser`와 `deluser`를 `adduser`패키지를 통해 제공하고 있다. 이 명령어들은 Debian의 특성을 고려하여 좀 더 편리하게 유저를 관리할 수 있게 해주지만, 다른 리눅스에서는 대부분 제공하지 않는다. Ubuntu는 Debian 계열 OS라서 `adduser` 등을 제공한다.

아래의 명렁어들은 low-level에서 유저의 id와 암호를 관리할 수 있게 해준다. 직접 유저 데이터베이스를 다룰 수 있게 해준다.
1. 비밀번호 바꾸기 Change Password (passwd)
1. 계정 만들기 Create an Account (useradd)
1. 계정 삭제하기 Delete an Account (userdel)
1. 계정 속성 수정하기 Modify an Account's properties (usermod)
1. 로그인 쉘 바꾸기 Change login shell (chsh)
1. 사용자 정보 바꾸기 Change user information (chfn) (fn means "full name")

## 현재 유저 확인하기
```sh
$ whoami
```
`whoami`를 통해 간단하게 유저 이름을 확인할 수 있다.

```sh
$ echo $USER
```
환경변수 `$USER`를 호출하여 확인할 수도 있다. 

```sh
$ w
```
`w` 명령어는 더 자세한 내용을 알려준다. 각 내용에 대한 자세한 설명은 [여기](https://www.howtogeek.com/410423/how-to-determine-the-current-user-account-in-linux/)를 참고.

## 유저 목록 확인하기
```sh
cat /etc/passwd
```
`etc/passwd`를 확인하는 방법.

```sh
cut -fl -d: /etc/passwd
```
아이디만 잘라서 보여준다.


```sh
grep /bin/bash /etc/passwd
```
```sh
grep /bin/bash /etc/passwd | cut -fl -d:
```

이런 방법도 있다.
[참고](https://overcode.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%82%AC%EC%9A%A9%EC%9E%90-%EB%AA%A9%EB%A1%9D-%ED%99%95%EC%9D%B8-Linux-User-List)

## 다른 유저로 로그인하기

```sh
su username
```
으로 `username`계정으로 로그인 할 수 있다.

```sh
su
```
와 같이 유저를 지정하지 않는 경우 `root`유저로 로그인 시도한다. 

## 유저 추가하기
```sh
useradd user1
passwd user1
```
를 통해 `user1`이라는 유저를 만들고  `user1`의 암호를 설정해줄 수도 있다.
해당 비밀번호는 `/etc/passwd`파일에 저장된다.

옵션을 이용하여 한번에 사용해보자.
```sh
useradd -m -g sudo user2
```
`-m`옵션으로 홈 디렉터리를 자동으로 생성해준다. 이 옵션이 없으면 디렉터리 경로만 지정되고 만들어주지는 않는다.
`-g sudo`옵션을 통해 `sudo`그룹에 `user2`를 포함시킨다.



[DebianWiki, UserAccounts](https://wiki.debian.org/UserAccounts)<br>
[howtogeek, how to determine the current user account in linux](https://www.howtogeek.com/410423/how-to-determine-the-current-user-account-in-linux/)<br>
[위드코딩, 리눅스 사용자 관리 명령어](https://withcoding.com/101)<br>
[여행을 개발하다, linux useradd 정리](https://tragramming.tistory.com/85)<br>
[리눅스/유닉스 사용자 관리](https://jhnyang.tistory.com/10)




# `sudo`
---

`sudo`는 **Su**per-**u**ser **do**의 줄임말이라고 여겨지는 유틸리티 프로그램이다.
시스템 관리자가 유저들에게 몇몇 명령어들을 `root`유저(혹은 다른 유저) 권한으로 실행할 수 있도록 하는 프로그램이다.
`sudo`를 통해 유저들에게 몇 안되는 명령어만 허용하면서도 유저들이 일하는데 문제가 없도록 하는 것이 기본 철학.
언제, 어떤 명령어를 실행했는지 기록하는 효과적인 방법이기도 하다.

## 왜 `sudo`를 쓸까?
`sudo`는 root 유저로 세션을 여는 것 대신 더 안전하고 더 나은 방법을 제공한다.

1. root유저의 암호를 알 필요가 없다(`sudo`는 해당 유저의 암호를 입력 요구함). 몇 가지의 명령어만 유저에게 주어졌다가 바로 회수된다.
1. `sudo`를 통해 몇 개의 특별히 허용된 명령어만 사용하는 것이 더 쉽다. 나머지 시간에는 권한이 없는 사용자로 일하면서, 실수로 만들 수 있는 데미지를 막을 수 있다.
1. 감시/기록: `sudo`가 실행될 때, 이용한 유저 이름과 명령어가 기록된다.

`sudo -i` 혹은 `sudo su`를 통해 root 유저로 로그인해서 세션을 여는 것은, 위의 장점들을 없애기 때문에 보통 더 이상 사용되지 않는다.

## `sudo` 설치
```sh
apt-get install sudo
```

## 유저와 `sudo`

Debian에서 기본 설정default은 `sudo` 그룹의 모든 유저가 모든 명령어를 실행 가능할 수 있게 되어있다.

### `sudo` 그룹 만들기


### 유저가 `sudo` 그룹에 포함되어 있는지 확인하기

해당 유저로 로그인 한 후,
```sh
$ groups
```
를 입력한 후 결과를 확인하거나,
```sh
$ foo sudo
```
를 통해 `id=foo`라는 유저가 `sudo` 그룹에 포함되어 있는지 확인할 수 있다.

### 유저를 `sudo` 그룹에 포함시키기

## `visudo`로 `/etc/sudoers` 수정하기

```sh
Defaults	passwd_tries=3
Defaults	badpass_message="wrong password!"
Defaults	log_input, log_output, iolog_dir="/var/log/sudo"
Defaults	requiretty
```
를 앞부분에 추가해준다.


[Debian wiki, sudo](https://wiki.debian.org/sudo?action=show&redirect=Sudo)


# ssh vs. telnet
---
컴퓨터간 통신 Protocol.
SSH와 TELNET ⊂ 응용 계층application layer ⊂ 인터넷 프로토콜 스위트Internet Protocol Suite

SSH: Secure Shell
telnet: Teletype Over Network Protocol
telnet은 1969년 개발 되었고, 보안문제로 인하여 SSH로 대부분 대체됨.
SSH는 단순히 telnet을 대체할 뿐만 아니라, 다양한 추가 기능 제공.
SSH는 1995년 처음 소개되었고, OpenSSH가 1999년 OpenBSD를 통해 공개.

SSH를 통해 **암호화된 통신**으로:
1. 네트워크 상의 다른 컴퓨터에 로그인
1. 원격으로 명령 실행
1. 다른 시스템으로 파일 복사
를 할 수 있다.

[Seung Hyun, SSH란 무엇인가요](https://medium.com/@jamessoun93/ssh%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-87b58c521d6f)<br>
[위키백과, 시큐어 셸](https://ko.wikipedia.org/wiki/%EC%8B%9C%ED%81%90%EC%96%B4_%EC%85%B8)<br>
[위키백과, 텔넷](https://ko.wikipedia.org/wiki/%ED%85%94%EB%84%B7)<br>


# UFW firewall
---
Uncomplicated Firewall (UFW)<br>
데비안 계열 및 다양한 리눅스 환경에서 작동하는 방화벽 관리 프로그램.<br>
UFW는 리눅스 커널 방화벽(넷필터 프레임워크, Netfilter)을 쉽게 관리할 수 있도록 해주는 프로그램이다.

UFW는 명령줄 인터페이스(CLI)를 사용하고, iptables를 사용한다. 


# cron
---
유닉스 계열 컴퓨터 운영체제의 시간 기반 잡 스케쥴러
`cron`을 사용하면 작업을 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링할 수 있음.

`crontab`(cron table)파일에 주기적 일정에 셸 명령어를 실행하도록 규정되어있음.
유저들은 각각 자신의 `crontab` 파일을 가질 수 있음.
`/etc` 또는 그 하위 디렉터리에 시스템 관리자들만 편집할 수 있는, 시스템 전부에 영향을 미치는 `crontab` 파일 존재할 수도 있음.

```sh
$ crontab -e
```
를 사용하여 유저를 위한 `crontab` 파일을 편집할 수 있음.

`crontab` 파일의 문법
```
 # ┌───────────── min (0 - 59)
 # │ ┌────────────── hour (0 - 23)
 # │ │ ┌─────────────── day of month (1 - 31)
 # │ │ │ ┌──────────────── month (1 - 12)
 # │ │ │ │ ┌───────────────── day of week (0 - 6) (0 to 6 are Sunday to Saturday, or use names; 7 is Sunday, the same as 0)
 # │ │ │ │ │
 # │ │ │ │ │
 # * * * * *  command to execute
```

[위키백과, cron](https://ko.wikipedia.org/wiki/Cron)<br>
[크론 표현식](https://madplay.github.io/post/a-guide-to-cron-expression)

# IPv4 주소와 MAC(Media Access Control)주소
---
추가 예정

# GRUB 부트로더
---

추가 예정

# 참고
---
[born2beroot 삽질의 흔적](https://tbonelee.tistory.com/m/16)<br>
[newmon, born2beroot overview](https://infinitt.tistory.com/390)<br>
[Born2beroot 설치 및 세팅만 정리](https://techdebt.tistory.com/18?category=833728)
[Born2beroot 아카이브](https://parkseunghan.notion.site/born2beroot-afdb78d74995456d9c91a4ae1be9874f)
