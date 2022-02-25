---
title: born2beroot
tags: 42seoul
layout: posts
toc: true
toc_lable: "목차"
toc_icon: "cog"
---

# Born2beroot 배경지식 정리

## 네트워크 기본 개념 정리

[서버란?](https://lipcoder.tistory.com/514)

[네트워크 기초 지식](https://lipcoder.tistory.com/515?category=908023)

## 모르는 단어 나열하기

1. [VirtualBox vs. UTM](#vm)
1. [signature.txt](#sign)
1. [Debian vs. CentOS](#debianCentOS)
1. [apt vs. aptitude](#apt)
1. [SELinux vs. AppArmor](#lsm)
1. [lvm](#lvm)
1. [ssh vs. telnet](#ssh)
1. [UFW firewall](#ufw)
1. [cron](#cron)


---

<!--
#vm
-->
## VirtualBox vs. UTM
가상 머신 Virtual Machine



<!--
#sign
-->
## signature.txt

과제에서 평가를 위해 Virtual disk의 signature를 저장하라고 함.
일단 [lvm파트](#lvm)를 읽어보자.

<!--
#debianCentOS
-->
## Debian vs. CentOS
리눅스 배포판 Linux Distribution (Distro)

|리눅스 배포판 				|Debian			|CentOS			|
|:---------------------:	| :-----------: | :--------------:|
| 							|데비안 계열	|레드햇 계열	|
|리눅스 보안모듈 (LSM)		|AppArmor		|SELinux		|

데비안 계열의 배포판으로는 우분투Ubuntu가 있다.

레드햇 계열의 배포판으로는 Red Hat의 커뮤니티 버전 Fedora와 CentOS 등이 있다.
Red Hat이라는 기업이 관리하는 리눅스 배포판이 Red Hat Enterprise Linux다. 해당 리눅스를 이용하면 Red Hat에서 유지 보수를 지원하기 때문에 기업에서 선호한다고 한다.
센토스CentOS는 레드햇 리눅스를 분기fork하여 상표를 제거한 것이라고 한다.


<!--
#apt
-->
## apt vs. aptitude


<!--
#lsm
-->
## SELinux vs. AppArmor
리눅스 보안 모듈 Linux Security Modules (LSM)

응용프로그램Applications, 프로세스processes, 파일files에 접근할 수 있는 권한을 정의하고, 보안 정책security policies에 따라 접근을 제어하는 보안 프레임 워크.
리눅스 커널이 단일한 보안 구현이 아닌 다양한 보안 모델을 지원할 수 있도록 함.
AppArmor, SELinux, Smack, TOMOYO 리눅스 등이 있다.

### AppArmor와 SELinux의 공통점: MAC vs. DAC
강제적 접근 통제 (MAC, mandatory access control) 시스템을 리눅스에 제공한다.

전통적인 유닉스와 리눅스는 임의적 접근 통제 (DAC, discretionary access control) 방식을 사용해왔다. DAC 시스템에서는 파일과 프로세스들은 **'소유자owner'**가 존재한다. 각 소유자가 파일에 대한 권한을 변경할 수 있는 재량이 있다.

AppArmor와 SELinux는 리눅스에게 MAC 시스템을 제공한다.
MAC 시스템에서는, 접근에 대한 관리 **정책policy**이 존재한다. DAC에서 사용자와 소유권에만 기반하는 것에 반해, MAC에서는 정의된 정책을 활용하여 접근 권한을 제어한다. 이를 통해 파일의 유형, 사용자의 역할, 프로그램의 기능과 신뢰도, 데이터의 민감성 무결성 등을 고려하여 보안을 유지할 수 있게 된다.

### AppArmor와 SELinux의 차이점
- file system labels: 파일 경로 vs. 아이노드inode
AppArmor는 policy files에만 기초하는데 반해, SELinux는 polic-file과 right file system labels가 필요하다. SELinux는 경로 대신 아이노드inode(리눅스 파일 고유번호)에 기반하여 파일 시스템 객체들을 구별하지만, AppArmor는 파일 경로(file tree)를 이용한다.

- AppArmor가 더 관리하기 쉽다.
label 시스템의 차이 때문에, 배우는 것이 더 쉽고 더 적은 수정을 요구한다.

### 설치

추가 필요

[위키백과, 리눅스 보안 모듈](https://ko.wikipedia.org/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B3%B4%EC%95%88_%EB%AA%A8%EB%93%88)
[Red Hat, What is SELinux?](https://www.redhat.com/en/topics/linux/what-is-selinux)
[AppArmor wiki](https://gitlab.com/apparmor/apparmor/-/wikis/home)
[linuxreviews.org, AppArmor 설명](https://linuxreviews.org/AppArmor)
[linuxhint, Debian AppArmor 튜토리얼](https://linuxhint.com/debian_apparmor_tutorial/)

<!--
#lvm
-->
## lvm
LVM: Logical Volume Manager

[리눅스 LVM과 RAID 개념](https://wiseworld.tistory.com/32)


<!--
#ssh
-->
## ssh vs. telnet
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

[Seung Hyun, SSH란 무엇인가요](https://medium.com/@jamessoun93/ssh%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-87b58c521d6f)

[위키백과, 시큐어 셸](https://ko.wikipedia.org/wiki/%EC%8B%9C%ED%81%90%EC%96%B4_%EC%85%B8)

[위키백과, 텔넷](https://ko.wikipedia.org/wiki/%ED%85%94%EB%84%B7)


<!--
#ufw
-->
## UFW firewall
Uncomplicated Firewall (UFW)
데비안 계열 및 다양한 리눅스 환경에서 작동하는 방화벽 관리 프로그램.
UFW는 리눅스 커널 방화벽(넷필터 프레임워크, Netfilter)을 쉽게 관리할 수 있도록 해주는 프로그램이다.

UFW는 명령줄 인터페이스(CLI)를 사용하고, iptables를 사용한다. 


<!--
#cron
-->
## cron

## 참고

[born2beroot 삽질의 흔적](https://tbonelee.tistory.com/m/16)