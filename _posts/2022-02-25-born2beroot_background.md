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
1. ssh vs. telnet
1. UFW firewall
1. cron
1. IPv4 주소와 MAC(Media Access Control)주소


# VirtualBox vs. UTM
---
가상 머신 Virtual Machine


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

정리하면 
PE ⊂ PV ⊂ VG
LE ⊂ LV ⊂ VG


[리눅스 LVM과 RAID 개념](https://wiseworld.tistory.com/32)
[농심클라우드, LVM 개념](https://tech.cloud.nongshim.co.kr/2018/11/23/lvmlogical-volume-manager-1-%EA%B0%9C%EB%85%90/)
[LVM이란? 매우 쉽게](https://mamu2830.blogspot.com/2019/12/lvmpv-vg-lv-pe-lvm.html)


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
Uncomplicated Firewall (UFW)
데비안 계열 및 다양한 리눅스 환경에서 작동하는 방화벽 관리 프로그램.
UFW는 리눅스 커널 방화벽(넷필터 프레임워크, Netfilter)을 쉽게 관리할 수 있도록 해주는 프로그램이다.

UFW는 명령줄 인터페이스(CLI)를 사용하고, iptables를 사용한다. 


# cron
---
추가 예정

# 참고
---
[born2beroot 삽질의 흔적](https://tbonelee.tistory.com/m/16)<br>
[newmon, born2beroot overview](https://infinitt.tistory.com/390)<br>