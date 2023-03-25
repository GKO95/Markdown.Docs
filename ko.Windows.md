---
category: 운영체제
title: 윈도우 NT
---
# 윈도우 NT
[윈도우 NT](https://ko.wikipedia.org/wiki/윈도우_NT)(Windows NT)는 [마이크로소프트](https://ko.wikipedia.org/wiki/마이크로소프트)에서 개발한 [윈도우](https://ko.wikipedia.org/wiki/마이크로소프트_윈도우)(Windows) 계열 중 하나이며, 전세계적으로 가장 널리 사용되고 있는 현역 [운영체제](https://ko.wikipedia.org/wiki/운영체제)(operating system; OS)이다. 본래 NT는 기존 운영체제인 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS)를 대체할 "신기술(New Technology)"을 염두하여 붙인 이름이지만, 현재는 아무런 의미가 없는 제품군 명칭으로 자리잡았다. 이름은 다소 생소할 수 있으나 사실상 배포되고 있는 거의 모든 윈도우 제품이 윈도우 NT 계열에 해당한다.

다음은 2023년 3월 21일 기준에 작성된 윈도우 NT 제품 목록이다.

<table style="margin-bottom: 16px; table-layout: fixed; width: 100%; margin: auto;">
<thead><tr><th colspan="7" style="text-align: center;">윈도우 NT 클라이언트 (2001년 ~ 현재)</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://ko.wikipedia.org/wiki/윈도우_XP">윈도우 XP</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_비스타">윈도우 비스타</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_7">윈도우 7</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_8">윈도우 8</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_8.1">윈도우 8.1</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_10">윈도우 10</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_11">윈도우 11</a></td></tr></tbody>
</table>

<table style="margin-top: 16px; table-layout: fixed; width: 100%; margin: auto;">
<thead><tr><th colspan="6" style="text-align: center;">윈도우 NT 서버 (2001년 ~ 현재)</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2003">윈도우 서버 2003</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2008">윈도우 서버 2008</a> (<a href="https://ko.wikipedia.org/wiki/윈도우_서버_2008_R2">R2</a>)</td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2012">윈도우 서버 2012</a> (<a href="https://ko.wikipedia.org/wiki/윈도우_서버_2012#윈도우_서버_2012_R2">R2</a>)</td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2016">윈도우 서버 2016</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2019">윈도우 서버 2019</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2022">윈도우 서버 2022</a></td></tr></tbody>
</table>

> 윈도우 NT와 함께 서비스 중인 운영체제로 [윈도우 IoT](https://ko.wikipedia.org/wiki/윈도우_IoT)가 있으며, 서비스 종료된 윈도우 제품으로 MS-DOS, [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x), [윈도우 모바일](https://ko.wikipedia.org/wiki/윈도우_모바일), 그리고 [윈도우 폰](https://ko.wikipedia.org/wiki/윈도우_폰)이 있다.

비록 윈도우 NT는 클라이언트와 서버로 운영체제가 구분되지만 동일한 빌드의 커널을 사용한다. 예시로 윈도우 8.1과 윈도우 서버 2012 R2 운영체제의 커널은 빌드 9600으로 일치한다. 핵심 커널은 같지만 두 운영체제로 나뉘어진 이유는 성능 최적화 대상과 목적, 그리고 이에 따른 기술 사양과 기능에 차이가 존재하기 때문이다.

마이크로소프트의 운영체제가 윈도우 NT로 전환한 것은 다음 기술적 의의를 가진다:

* 완전한 [32비트](https://ko.wikipedia.org/wiki/32비트) 아키텍처 지원
* 다양한 하드웨어 [이식성](#하드웨어-이식성) 및 소프트웨어 [호환성](#환경-서브시스템) 지원
* 신뢰성을 보장하도록 [커널](ko.Processor.md#보호-링) 및 [사용자 모드](ko.Processor.md#보호-링)의 구분 그리고 [메모리](ko.Memory.md)의 [가상화](ko.Process.md#가상-주소-공간)

## 윈도우 아키텍처
아래 이미지는 윈도우 7 (또는 윈도우 서버 2008 R2) 운영체제의 아키텍처를 나타낸 다이어그램이다.

![윈도우 7 및 서버 2008 R2 아키텍처<br/><sub><i>출처: <a href="https://www.oreilly.com/library/view/windows-internals-sixth/9780735671294/">Windows Internals, 6th Edition</a> / 파일: <a href="https://github.com/LordNoteworthy/windows-internals/blob/master/windows-internals-6th-ed.md">LordNoteworthy/windows-internals</a></i></sub>](./images/windows_architecture_overview.png)

### 하드웨어 이식성
[이식성](https://en.wikipedia.org/wiki/Software_portability)(portability), 즉 특정 하드웨어 및 명령어 집합에 대한 종속을 최소화하여 타 플랫폼으로 쉽게 설치하여 사용할 수 있는 성질은 초창기 윈도우 NT의 핵심 설계 목적이었다. 현재는 [i386](https://ko.wikipedia.org/wiki/IA-32)(단, 윈도우 11부터 제외), [x86-64](https://ko.wikipedia.org/wiki/X86-64), 그리고 [ARM64](https://ko.wikipedia.org/wiki/ARM_아키텍처) 아키텍처를 지원한다. 윈도우 NT는 이식성을 위해 다음 두 가지 설계를 채택하였다:

* **계층화 설계**

    윈도우 NT는 프로세서 아키텍처 및 하드웨어(예. [메인보드](https://ko.wikipedia.org/wiki/메인보드), [PCI](https://ko.wikipedia.org/wiki/PCI_버스) 장치 등)에 특정된 저급 시스템 코드, 그리고 이와 상관없이 공통된 운영체제의 기능들을 다루는 고급 시스템 코드로 계층을 두 개로 나눈다. 여기서 이식성에 기여하는 저급 시스템 코드에는 (1) 아키텍처 특화된 `Ntoskrnl.exe`의 [커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅)) 그리고 (2) 하드웨어 특화된 `hal.dll`의 [하드웨어 추상화 계층](https://ko.wikipedia.org/wiki/https://ko.wikipedia.org/wiki/하드웨어_추상화#하드웨어_추상화_계층)이 있다.

* **[C 프로그래밍 언어](ko.C.md)**

    윈도우 NT의 대부분 소스 코드는 이식성에 유리한 C 프로그래밍 언어로 작성되었다. 일부는 [C++](ko.Cpp.md) 프로그래밍 언어로 작성되었으며, 특히 하드웨어와 직접 통신이 필요하거나(예. [인터럽트 핸들러](ko.Processor.md#인터럽트)) 성능에 매우 민감한 운영체제 코드(예. [문맥 교환](ko.Processor.md#문맥-교환))는 [어셈블리](ko.Assembly.md)로 구현되었다.

## 환경 서브시스템
환경 서브시스템(environment subsystems)은 어플리케이션이 윈도우 커널단으로부터 시스템 서비스를 사용할 수 있도록 한다. 환경 서브시스템은 크게 `.EXE` 그리고 `.DLL` 유형으로 나뉘어진다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">환경 서브시스템 프로세스 및 라이브러리 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">환경 서브시스템 프로세스 (<code>.EXE</code>)</th><th style="text-align: center;">환경 서브시스템 라이브러리 (<code>.DLL</code>)</th></tr></thead>
<tbody style="text-align: center;"><tr><td>해당 환경 서브시스템으로 링크된 프로그램들의 상태를 관리하는 사용자 모드 프로세스이다.</td><td>프로그램이 필요한 기능을 사용자 모드에서 요청할 수 있도록 <a href="ko.WinAPI.md">윈도우 API</a>와 같은 인터페이스를 제공한다.</td></tr>
<tr><td>예시: <a href="https://ko.wikipedia.org/wiki/클라이언트/서버_런타임_하위_시스템"><code>Csrss.exe</code></a>(Client/Server Runtime Subsystem)</td><td>예시: <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#KERNEL32.DLL"><code>Kernel32.dll</code></a>, <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#USER32.DLL"><code>User32.dll</code></a>, <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#GDI32.DLL"><code>Gdi32.dll</code></a> 등</td></tr></tbody>
</table>

위에 나열된 서브시스템 프로세스 및 라이브러리는 전부 윈도우 서브시스템의 일환이다: 콘솔 및 GUI 창 관련 작업, 그리고 디스플레이 입출력을 처리하는 기본적이면서 핵심적인 역할을 수행한다. 다른 환경 서브시스템도 이러한 기능을 윈도우 서브시스템의 개입이 있어야만 구현할 수 있다. 만일 `csrss.exe`가 종료되면 시스템은 치명적이라 판단하여 의도적으로 오류 [0xEF CRITICAL_PROCESS_DIED](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/bug-check-0xef--critical-process-died) [블루스크린](ko.BSOD.md)을 일으킬 정도로 매우 중요한 구성요소이다.

윈도우 7부터 각 콘솔창을 관리하는 `conhost.exe`(일명 콘솔 윈도우 호스트) 서버 프로세스가 소개되었으나, 해당 프로세스의 생성 및 키보드 입력 수신에는 여전히 `csrss.exe` 역할이 요구된다.

> 윈도우 8부터 다시 한 번 변경된 콘솔 아키텍처에서 `conhost.exe` 프로세스 생성 및 키보드 입력 수신은 `ConDrv.sys` 드라이버에 의해 커널에서 처리되어 절차가 간략해졌다.

서브시스템은 세션 관리자 `smss.exe` 프로세스에 의해 실행되며 해당 내역은 `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\SubSystems` 레지스트리에서 찾아볼 수 있다. 그리고 Required 레지스트리 값은 시스템 부팅 시 불러올 서브시스템 프로세스들이 나열되어 있다.

![세션 관리자가 실행하는 서브시스템 정보에 대한 레지스트리](./images/windows_subsystems_registry.png)

사용자 모드 어플리케이션은 직접 시스템 서비스를 호출하지 않는다. 환경 서브시스템 라이브러리(줄여서 "서브시스템 DLL")의 인터페이스를 통해서 필요한 기능을 제공받으며, 이를 호출할 시 함수는 다음 중 하나 이상 방식으로 처리된다:

1. 윈도우 커널로부터 최소 하나 이상의 시스템 서비스를 호출한다.
2. 윈도우 커널로부터 단 하나의 호출이 필요없이, 서브시스템 DLL에서 함수가 전부 처리된다.
3. 서브시스템 DLL은 [ALPC](https://ko.wikipedia.org/wiki/로컬_프로시저_호출)를 통해 환경 서브시스템 프로세스에서 함수의 일부 작업을 처리하도록 요청한다.

### 타 서브시스템
윈도우 NT 운영체제는 초창기에 총 세 개의 환경 서브시스템을 지원하였으나, 현재 (2022년 11월 6일) 가장 최신 운영체제인 윈도우 10 및 11 기점에서 윈도우 서브시스템만 남은 상태이다. 프로세스는 오로지 한 개의 서브시스템에만 종속될 수 있으며, 프로그램이 실행될 때 이미지 헤더 정보로부터 확인된 서브시스템의 프로세스에 연동된다.

* **[POSIX](https://ko.wikipedia.org/wiki/POSIX)**

    윈도우 XP 이후 지원 종료된 POSIX 서브시스템 `psxss.exe` 및 `psxdll.dll`은 UNIX 기반의 운영체제의 어플리케이션을 지원하였다. 이후 윈도우 7 얼티밋 및 엔터프라이즈 그리고 윈도우 서버 2008 R2 한정으로 SUA(Subsystem for UNIX-based Application)라는 개선된 POSIX 서브시스템을 잠시동안 지원하였다.

    > 윈도우 10부터 등장한 [WSL](#windows-subsystem-for-linux)은 POSIX 서브시스템을 계승하였으나 기존 서브시스템과 전혀 다른 새로운 기술이 적용되었다.

* **[OS/2](https://ko.wikipedia.org/wiki/OS/2)**

    IBM의 기획 하에 마이크로소프트와 함께 공동 개발되었던 OS/2 운영체제 환경을 지원하기 위한 서브시스템이었으며, 윈도우 2000 이후 지원이 종료되었다.

    > IBM과 마이크로소프트 간의 [OS/2 운영체제 이야기](https://techland.time.com/2012/04/02/25-years-of-ibms-os2-the-birth-death-and-afterlife-of-a-legendary-operating-system/)는 매우 흥미로운 배경과 전개가 있어, IT 산업의 발전 과정에 관심이 있다면 내용이나 기사를 접해 보는 걸 추천한다.

### Windows Subsystem for Linux
[WSL](https://ko.wikipedia.org/wiki/리눅스용_윈도우_하위_시스템)(Windows Subsystem for Linux), 일명 리눅스용 윈도우 서브시스템은 아래에 서술된 기존 환경 서브시스템이 가진 기술적 문제들을 극복하기 위해 새롭게 설계된 서브시스템이다.

1. 윈도우 [PE](https://ko.wikipedia.org/wiki/PE_포맷) 실행 파일, 즉 `.EXE` 확장자로 새로 빌드되어야 윈도우 운영체제에서 서브시스템 정보를 추출하고 실행할 수 있다.
2. 타 운영체제 프로그램을 윈도우 서브시스템으로 [래핑](https://ko.wikipedia.org/wiki/래퍼_라이브러리)(wrapping)하는 방식을 택하였으나, 알아채기 힘든 호환성 결함이 야기될 수 있다.

윈도우 10 모바일 운영체제에서 안드로이드 앱을 실행할 수 있도록 추진된 [아스토리아 프로젝트](https://en.wikipedia.org/wiki/Windows_10_Mobile#Project_Astoria)(이후 Windows Subsystem for Android로 소개)를 발판으로 POSIX/UNIX가 아닌 "순수" 리눅스 어플리케이션을 지원하고자 [우분투](https://ko.wikipedia.org/wiki/우분투)(Ubuntu) 배포판을 개발한 [캐노니컬](https://ko.wikipedia.org/wiki/캐노니컬)과 합작한 결과물이다.

![WSL2로 설치된 <a href="https://ko.wikipedia.org/wiki/데비안">데비안</a> 배포판의 터미널과 파일 탐색기](./images/wsl_terminal_debian.png)

WSL은 두 종류의 아키텍처 설계가 있어 각각 장단점을 가진다. 자세한 내용은 [마이크로소프트 문서](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)를 참고한다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">WSL 버전 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">WSL1</th><th style="text-align: center;">WSL2</th></tr></thead>
<tbody style="text-align: center;"><tr><td>리눅스 시스템 서비스를 처리할 Pico 제공자(Pico provider)란 특수한 커널 모드 드라이버를 사용하지만, 완전한 호환성을 제공하지 못한다.</td><td>경량화된 가상머신을 사용하여 완전한 리눅스 커널 및 GUI 지원이 가능하나, 윈도우 NTFS 접근에는 다소 성능 저하가 나타난다.</td></tr></tbody>
</table>

## 운영체제 리소스
비록 본 문서에서 소개하는 내용들은 윈도우 NT 운영체제를 위주로 다루고 있으나, 전반적인 이해를 돕기 위해 아래에 나열된 시스템의 핵심 리소스에 대하여 살펴보기를 권장한다.

* [프로세서](ko.Processor.md)
* [메모리](ko.Memory.md)
* [디스크](ko.Storage.md)
* [네트워크](ko.Network.md)

## 관리 프로그램
다음은 시스템 상태를 모니터링 및 관리할 수 있는 프로그램을 나열한다.

* [성능 모니터](ko.Performance_Monitor.md)
* [이벤트 뷰어](ko.Event_Viwer.md)
* [파워셸](ko.PowerShell.md)

# 같이 보기
* [윈도우 서비스 (프로그램)](ko.Service.md)
* [윈도우 API](ko.WinAPI.md)
* [블루스크린](ko.BSOD.md)
