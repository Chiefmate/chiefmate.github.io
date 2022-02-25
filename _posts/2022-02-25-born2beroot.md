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

1. VirtualBox vs. UTM
1. signature.txt
1. Debian vs. CentOS
1. apt vs. aptitude
1. SELinux vs. AppArmor
1. lvm
1. ssh vs. telnet
1. UFW firewall
1. cron

## VirtualBox vs. UTM
가상 머신Virtual Machine.

## signature.txt
과제에서 평가를 위해 Virtual disk의 signature를 저장하라고 함.
일단 [lvm파트](#lvm)를 읽어보자.

## Debian vs. CentOS
리눅스 배포판Linux Distribution (Distro)

|리눅스 배포판			|Debian			|CentOS			|
|:--------------------:	| :-----------: | :--------------:|
| 						|데비안 계열	|레드햇 계열	|
|Security Framework		|AppArmor		|SELinux		|

데비안 계열의 배포판으로는 우분투Ubuntu가 있다.

레드햇 계열의 배포판으로는 Red Hat의 커뮤니티 버전 Fedora와 CentOS 등이 있다.
Red Hat이라는 기업이 관리하는 리눅스 배포판이 Red Hat Enterprise Linux다. 해당 리눅스를 이용하면 Red Hat에서 유지 보수를 지원하기 때문에 기업에서 선호한다고 한다.
센토스CentOS는 레드햇 리눅스를 분기fork하여 상표를 제거한 것이라고 한다.


## apt vs. aptitude

## SELinux vs. AppArmor
Security Framework.
응용프로그램Applications, 프로세스processes, 파일files에 접근할 수 있는 권한을 정의하고, 보안 정책security policies에 따라 접근을 제어함.


[Red Hat, What is SELinux?](https://www.redhat.com/en/topics/linux/what-is-selinux)

[AppArmor wiki](https://gitlab.com/apparmor/apparmor/-/wikis/home)

#lvm
## lvm
LVM: Logical Volume Manager

[리눅스 LVM과 RAID 개념](https://wiseworld.tistory.com/32)

## ssh vs. telnet
컴퓨터간 통신 Protocol.
SSH와 TELNET ⊂ 응용 계층application layer ⊂ 인터넷 프로토콜 스위트Internet Protocol Suite

SSH: Secure Shell
telnet: Teletype Over Network Protocol
telnet은 1969년 개발 되었고, 보안문제로 인하여 SSH로 대부분 대체됨.
SSH는 1995년 처음 소개되었고, OpenSSH가 1999년 OpenBSD를 통해 공개.

SSH를 통해 **암호화된 통신**으로:
1. 네트워크 상의 다른 컴퓨터에 로그인
1. 원격으로 명령 실행
1. 다른 시스템으로 파일 복사

[Seung Hyun, SSH란 무엇인가요](https://medium.com/@jamessoun93/ssh%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-87b58c521d6f)

[위키백과, 시큐어 셸](https://ko.wikipedia.org/wiki/%EC%8B%9C%ED%81%90%EC%96%B4_%EC%85%B8)

[위키백과, 텔넷](https://ko.wikipedia.org/wiki/%ED%85%94%EB%84%B7)

## UFW firewall

## cron

## 참고

[born2beroot 삽질의 흔적](https://tbonelee.tistory.com/m/16)