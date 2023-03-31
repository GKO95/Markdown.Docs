---
category: 운영체제
title: 서브시스템
---
# 환경 서브시스템
[윈도우 NT](ko.Windows.md)의 초창기 목표는 다양한 플랫폼 어플리케이션을 지원할 수 있는 호환성을 제공하는 것으로, 이를 위해 고안된 시스템 요소가 바로 환경 서브시스템이다. 윈도우 NT 커널을 그대로 사용하되, 어플리케이션 이미지 헤더에 지정된 설정에 따라 윈도우, POSIX, OS/2 중 한 개의 환경 서브시스템에서 [프로세스](ko.Process.md)가 실행된다. 다음은 윈도우 NT에서 지원하는 (또는 지원하였던) 환경 서브시스템이다:

* **[윈도우](#윈도우-서브시스템)**

* **[POSIX](https://ko.wikipedia.org/wiki/POSIX)** (지원 종료: ~ 윈도우 XP)

    윈도우 7 얼티밋 및 엔터프라이즈, 그리고 윈도우 서버 2008 R2 한정으로 SUA(Subsystem for UNIX-based Application)라는 개선된 POSIX 서브시스템이 잠시동안 지원하였다. 단, 윈도우 10부터 등장한 [WSL](#windows-subsystem-for-linux)은 POSIX 서브시스템을 계승하였으나 기존 서브시스템과 전혀 다른 새로운 기술이 적용되었다.

* **[OS/2](https://ko.wikipedia.org/wiki/OS/2)** (지원 종료: ~ 윈도우 2000)

    IBM의 기획 하에 마이크로소프트와 함께 공동 개발되었던 OS/2 운영체제 환경을 지원하였다.

환경 서브시스템은 크게 `.EXE` 프로세스와 `.DLL` 라이브러리 유형으로 나뉘어진다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">환경 서브시스템 프로세스 및 라이브러리 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">환경 서브시스템 프로세스 (<code>.EXE</code>)</th><th style="text-align: center;">환경 서브시스템 라이브러리 (<code>.DLL</code>)</th></tr></thead>
<tbody style="text-align: center;"><tr><td>해당 환경 서브시스템으로 링크된 프로그램들의 상태를 관리하는 사용자 모드 프로세스이다.</td><td>해당 환경 서브시스템의 프로그램이 기능 및 리소스를 요청할 수 있는 인터페이스(<a href="ko.WinAPI.md">윈도우 API</a> 등)를 제공한다.</td></tr>
<tr><td>예시: <a href="https://ko.wikipedia.org/wiki/클라이언트/서버_런타임_하위_시스템"><code>Csrss.exe</code></a>(Client/Server Runtime Subsystem)</td><td>예시: <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#KERNEL32.DLL"><code>Kernel32.dll</code></a>, <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#USER32.DLL"><code>User32.dll</code></a>, <a href="https://ko.wikipedia.org/wiki/윈도우_라이브러리_파일#GDI32.DLL"><code>Gdi32.dll</code></a> 등</td></tr></tbody>
</table>

만일 사용자 프로세스가 서브시스템 라이브러리(일명 서브시스템 DLL)에게 어떠한 기능를 요청할 시, 다음 작업 처리 방식이 서브시스템 DLL에서 이루어진다:

1. 서브시스템 DLL 내에서 전부 처리된다.
1. 커널 운영자(Executive)로부터 최소 하나 이상의 시스템 서비스를 호출한다.
1. [ALPC](https://ko.wikipedia.org/wiki/로컬_프로시저_호출)를 통해 해당 환경 서브시스템 프로세스에게 일부 작업 처리를 요청한다.

환경 서브시스템은 세션 관리자 `Smss.exe`에 의해 관리되며, `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\SubSystems` 레지스트리 키에 설정되어 있다. `Required` 값은 생성되는 각 세션마다 반드시 초기화되는 환경 서브시스템을 나열하며, `Option` 값의 서브시스템은 필요에 따라 초기화된다.

> `Debug` 레지스트리 값에는 아무런 환경 서브시스템가 기입되어 있지 않고 윈도우 XP 이후부터 필요가 없어졌으나 호환성을 위해 유지되고 있다.

![세션 관리자가 실행하는 서브시스템 정보에 대한 레지스트리](./images/windows_subsystems_registry.png)

## 윈도우 서브시스템
윈도우 서브시스템(Windows subsystem)은 윈도우 NT의 핵심 요소 중 하나이며, 현재로써 유일하게 남은 환경 서브시스템이다. 프로그램 창을 포함한 화면 및 장치 입출력(마우스 커서 움직임, 키보드 자판 입력 등)을 처리하고 관리하는 기초적이지만 필수적인 역할을 수행하며, 다른 환경 서브시스템도 이러한 기능을 사용하려면 반드시 윈도우 서브시스템을 거쳐야 한다.

윈도우 서브시스템은 `Csrss.exe` 프로세스와 서브시스템 DLL 외의도 다음 구성원이 존재한다.

* `Csrss.exe`는 해당하는 세션에 다음 네 개의 DLL을 로드한다: `Basesrv.dll`, `Winsrv.dll`, `Sxssrv.dll` 그리고 `Csrsrv.dll`. 이들은 프로세스 및 스레드 생성과 종료를 처리하거나 윈도우 메시지를 전달하는 등의 세션 운영에 필요한 기초적인 기능들을 제공한다.

* `Win32k.sys`은 윈도우 서브시스템에서 핵심되는 기능이라고 소개한 장치 입력 및 프로그램 창을 처리하는 데 사용되는 커널 모드 장치 드라이버이다. 아래는 해당 장치 드라이버에 포함된 구성들을 소개한다.

    <table style="width: 95%; margin: auto;"><caption style="caption-side: top;"><code>Win32k.sys</code> 장치 드라이버의 구성</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">구성</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/창_관리자">창 관리자</a><br/>(Windows Manager)</td><td>바탕화면, 프로그램 창을 포함한 GUI 요소들을 생성하고 화면에 표시한다(콘솔 제외). 마우스 및 키보드 등의 장치 입력을 수신받아 GUI 요소에 상호작용 이벤트 처리에도 관여한다.</td></tr><tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/그래픽_장치_인터페이스">그랙픽 장치 인터페이스</a><br/>(Graphics Device Interface; GDI)</td><td>직선, 곡선, 도형 등의 그래픽 요소의 생성 및 조작, 폰트 렌더링, 색상 관리 등을 담당하는 함수나 서비스르 제공한다.</td></tr></tbody></table>

* `Conhost.exe`, 일명 "콘솔 호스트 프로세스"는 윈도우 7부터 소개되어 콘솔 창을 생성 및 관리를 담당한다. 본래 `Csrss.exe` 직접 처리하였으나, 로컬 시스템 계정으로 실행된 서브시스템 프로세스로부터 일반 사용자에게 부여되어서 안 될 권한이 주어질 수 있는 보안 문제를 야기할 수 있어 소개되었다.

    > 한편, 윈도우 8부터 소개된 콘솔 드라이버 `Condrv.sys`의 역할은 키보드 입출력을 곧바로 `Conhost.exe`로 전달한다. 이전에는 `Csrss.exe` 및 `Win32k.sys`를 거쳐 이벤트를 전달하는 형식으로 키보드 입력이 반영되었던 절차를 대폭 간소화하여 성능 개선에 이바지한다.

* [데스크탑 창 관리자](https://ko.wikipedia.org/wiki/데스크톱_창_관리자)(Desktop Windows Manager; `Dwm.exe`)는 데스크탑의 모든 그래픽 요소들을 관리하는 윈도우 운영체제의 구성 중 하나이다. 여기서 "데스크탑"이란, 사용자가 컴퓨터를 시작할 때 화면에 접하는 GUI를 가리키며 파일 탐색기, 작업 표시줄, 시작 메뉴 등으로 이루어져 있다.

위와 같은 구성과 기능을 갖춘 윈도우 서브시스템은 결국 모든 세션에서 반드시 필요한 존재로, 작업 관리자에서는 열린 세션 개수만큼 `Csrss.exe`가 실행되고 있는 걸 찾아볼 수 있다. 심지어 [윈도우 서비스](ko.Service.md) 전용인 ID 0의 세션도 콘솔 작업이 동반되므로 윈도우 서브시스템이 필요하다. 또한 대부분의 핵심 기능들은 커널 모드에서 동작하므로, 윈도우 어플리케이션을 실행하는 거 자체에서는 문맥 교환이 거의 이루어지지 않는다.

## Windows Subsystem for Linux
[WSL](https://ko.wikipedia.org/wiki/리눅스용_윈도우_하위_시스템)(Windows Subsystem for Linux), 일명 리눅스용 윈도우 서브시스템은 아래에 서술된 기존 환경 서브시스템이 가진 기술적 문제들을 극복하기 위해 새롭게 설계된 서브시스템이다.

1. 윈도우 [PE](https://ko.wikipedia.org/wiki/PE_포맷) 실행 파일, 즉 `.EXE` 확장자로 새로 빌드되어야 윈도우 운영체제에서 서브시스템 정보를 추출하고 실행할 수 있다.
2. 타 운영체제 프로그램을 윈도우 서브시스템으로 [래핑](https://ko.wikipedia.org/wiki/래퍼_라이브러리)(wrapping)하는 방식을 택하였으나, 알아채기 힘든 호환성 결함이 야기될 수 있다.

윈도우 10 모바일 운영체제에서 안드로이드 앱을 실행할 수 있도록 추진된 [아스토리아 프로젝트](https://en.wikipedia.org/wiki/Windows_10_Mobile#Project_Astoria)(이후 Windows Subsystem for Android로 소개)를 발판으로 POSIX/UNIX가 아닌 "순수" 리눅스 어플리케이션을 지원하고자 [우분투](https://ko.wikipedia.org/wiki/우분투)(Ubuntu) 배포판을 개발한 [캐노니컬](https://ko.wikipedia.org/wiki/캐노니컬)과 합작한 결과물이다.

![WSL2로 설치된 <a href="https://ko.wikipedia.org/wiki/데비안">데비안</a> 배포판의 터미널과 파일 탐색기](./images/wsl_terminal_debian.png)

WSL은 두 종류의 아키텍처 설계가 있어 각각 장단점을 가진다. 자세한 내용은 [마이크로소프트 문서](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)를 참고한다.

<table style="table-layout: fixed; width: 95%; margin: auto;">
<caption style="caption-side: top;">WSL 버전 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">WSL1</th><th style="text-align: center;">WSL2</th></tr></thead>
<tbody style="text-align: center;"><tr><td>리눅스 시스템 서비스를 처리할 Pico 제공자(Pico provider)란 특수한 커널 모드 드라이버를 사용하지만, 완전한 호환성을 제공하지 못한다.</td><td>경량화된 가상머신을 사용하여 완전한 리눅스 커널 및 GUI 지원이 가능하나, 윈도우 NTFS 접근에는 다소 성능 저하가 나타난다.</td></tr></tbody>
</table>