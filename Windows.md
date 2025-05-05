# 윈도우 NT
**[윈도우 NT](https://en.wikipedia.org/wiki/Windows_NT)**(Windows NT)는 [마이크로소프트](https://www.microsoft.com/)에서 개발한 [윈도우](https://en.wikipedia.org/wiki/Microsoft_Windows)(Windows) 계열 중 하나이며, 전세계적으로 가장 널리 사용되고 있는 현역 [운영체제](https://en.wikipedia.org/wiki/Operating_system)(operating system; OS)이다. 본래 NT는 기존 운영체제인 [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS)를 대체할 "신기술(New Technology)"을 염두하여 붙인 이름이지만, 현재는 아무런 의미가 없는 제품군 명칭으로 자리잡았다. 이름은 다소 생소할 수 있으나 사실상 배포되고 있는 거의 모든 윈도우 제품이 윈도우 NT 계열에 해당한다.

다음은 2025년 5월 5일 기준에 작성된 윈도우 NT 제품 목록이다.

<table style="margin-bottom: 16px; table-layout: fixed; width: 100%; margin-left: auto; margin-right: auto;">
<thead><tr><th colspan="7" style="text-align: center;">윈도우 NT 클라이언트 (2000년 ~ 현재)</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://en.wikipedia.org/wiki/Windows_XP">윈도우 XP</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_Vista">윈도우 비스타</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_7">윈도우 7</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_8">윈도우 8</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_8.1">윈도우 8.1</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_10">윈도우 10</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_11">윈도우 11</a></td></tr></tbody>
</table>

<table style="margin-top: 16px; table-layout: fixed; width: 100%; margin-left: auto; margin-right: auto;"><thead><tr><th colspan="7" style="text-align: center;">윈도우 NT 서버 (2000년 ~ 현재)</th></tr></thead><tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2003">윈도우 서버 2003</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2008">윈도우 서버 2008</a> (<a href="https://en.wikipedia.org/wiki/Windows_Server_2008_R2">R2</a>)</td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2012">윈도우 서버 2012</a> (<a href="https://en.wikipedia.org/wiki/Windows_Server_2012_R2">R2</a>)</td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2016">윈도우 서버 2016</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2019">윈도우 서버 2019</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2022">윈도우 서버 2022</a></td><td><a href="https://en.wikipedia.org/wiki/Windows_Server_2025">윈도우 서버 2025</a></td></tr></tbody></table>

> 윈도우 NT와 함께 서비스 중인 운영체제로 [윈도우 IoT](https://en.wikipedia.org/wiki/Windows_IoT)가 있으며, 단종된 윈도우 제품으로 [MS-DOS](https://github.com/microsoft/MS-DOS), [윈도우 9x](https://en.wikipedia.org/wiki/Windows_9x), [윈도우 모바일](https://en.wikipedia.org/wiki/Windows_Mobile), 그리고 [윈도우 폰](https://en.wikipedia.org/wiki/Windows_Phone)이 있다.

비록 윈도우 NT는 클라이언트와 서버로 운영체제가 구분되지만 동일한 빌드의 커널을 사용한다. 예시로 윈도우 8.1과 윈도우 서버 2012 R2 운영체제의 커널은 빌드 9600으로 일치한다. 핵심 커널은 같지만 두 운영체제로 나뉘어진 이유는 성능 최적화 대상과 목적, 그리고 이에 따른 기술 사양과 기능에 차이가 존재하기 때문이다.

마이크로소프트의 운영체제가 윈도우 NT로 전환한 것은 다음 기술적 의의를 가진다:

* 완전한 [32비트](https://en.wikipedia.org/wiki/32-bit_computing) 아키텍처 지원
* 다양한 하드웨어 [이식성](#하드웨어-이식성) 및 소프트웨어 [호환성](#환경-서브시스템) 지원
* 신뢰성을 보장하도록 [커널](Processor.md#권한-수준) 및 [사용자 모드](Processor.md#권한-수준)의 구분 그리고 [메모리](Memory.md)의 [가상화](Process.md#가상-주소-공간)

## 아키텍처
아래 그림은 윈도우 7 (또는 윈도우 서버 2008 R2) 운영체제의 아키텍처를 나타낸 다이어그램이다.

![윈도우 7 및 서버 2008 R2 아키텍처 (Windows Internals, 6th Edition)](https://raw.githubusercontent.com/LordNoteworthy/windows-internals/master/windows-internals-6th-ed/assets/windows-architecture.png)

다음은 일부 윈도우 아키텍처 구성 중에서 개별 문서로 작성된 것을 나열한다:

* [윈도우 서비스](Service.md)
* [환경 서브시스템](#환경-서브시스템) 및 [시스템 서비스](WinAPI.md#시스템-서비스)
* [윈도우 커널](Kernel.md#NT-커널) 및 [드라이버](Driver.md)

# 환경 서브시스템
[윈도우 NT](#윈도우-nt)의 초창기 목표는 다양한 플랫폼 어플리케이션을 지원할 수 있는 호환성을 제공하는 것으로, 이를 위해 고안된 시스템 요소가 바로 환경 서브시스템이다. 윈도우 NT 커널을 그대로 사용하되, 어플리케이션 이미지 헤더에 지정된 설정에 따라 윈도우, [POSIX](https://en.wikipedia.org/wiki/POSIX), OS/2 중 한 개의 환경 서브시스템에서 [프로세스](Process.md)가 실행된다. 다음은 윈도우 NT에서 지원하는 (또는 지원하였던) 환경 서브시스템이다:

* **[Windows](#윈도우-서브시스템)**

* **[POSIX](https://en.wikipedia.org/wiki/Windows_Services_for_UNIX)** (지원 종료: ~ 윈도우 XP)

    윈도우 7 얼티밋 및 엔터프라이즈, 그리고 윈도우 서버 2008 R2 한정으로 SUA(Subsystem for UNIX-based Application)라는 개선된 POSIX 서브시스템이 잠시동안 지원하였다. 단, 윈도우 10부터 등장한 [WSL](#리눅스용-윈도우-서브시스템)은 POSIX 서브시스템을 계승하였으나 기존 서브시스템과 전혀 다른 새로운 기술이 적용되었다.

* **[OS/2](https://en.wikipedia.org/wiki/OS/2)** (지원 종료: ~ 윈도우 2000)

    IBM의 기획 하에 마이크로소프트와 함께 공동 개발되었던 OS/2 운영체제 환경을 지원하였다.

환경 서브시스템은 크게 EXE 및 DLL 확장자의 프로세스와 라이브러리 유형으로 나뉘어진다.

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">환경 서브시스템 프로세스 및 라이브러리 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">환경 서브시스템 프로세스 (<code>EXE</code>)</th><th style="text-align: center;">환경 서브시스템 라이브러리 (<code>DLL</code>)</th></tr></thead><tbody style="text-align: center;"><tr><td>해당 환경 서브시스템으로 링크된 프로그램들의 상태를 관리하는 사용자 모드 프로세스이다.</td><td>해당 환경 서브시스템의 프로그램이 기능 및 리소스를 요청할 수 있는 인터페이스(<a href="WinAPI.md">윈도우 API</a> 등)를 제공한다.</td></tr><tr><td>예시: <a href="https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem">Csrss.exe</a>(Client/Server Runtime Subsystem)</td><td>예시: <a href="https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#KERNEL32.DLL">Kernel32.dll</a>, <a href="https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#USER32.DLL">User32.dll</a>, <a href="https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#GDI32.DLL">Gdi32.dll</a> 등</td></tr></tbody></table>

만일 사용자 프로세스가 서브시스템 라이브러리(일명 서브시스템 DLL)에게 어떠한 기능를 요청할 시, 다음 작업 처리 방식이 서브시스템 DLL에서 이루어진다:

1. 서브시스템 DLL 내에서 전부 처리된다.
1. 커널 운영자(Executive)로부터 최소 하나 이상의 [시스템 서비스](WinAPI.md#시스템-서비스)를 호출한다.
1. [ALPC](https://en.wikipedia.org/wiki/Local_Inter-Process_Communication)를 통해 해당 환경 서브시스템 프로세스에게 일부 작업 처리를 요청한다.

환경 서브시스템은 [세션 관리자](Process.md#세션-관리자)에 의해 관리되며, `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\SubSystems` 레지스트리 키에 설정되어 있다. `Required` 값은 생성되는 각 세션마다 반드시 초기화되는 환경 서브시스템을 나열하며, `Option` 값의 서브시스템은 필요에 따라 초기화된다.

> `Debug` 레지스트리 값에는 아무런 환경 서브시스템가 기입되어 있지 않고 윈도우 XP 이후부터 필요가 없어졌으나 호환성을 위해 유지되고 있다.

![세션 관리자가 실행하는 서브시스템 정보에 대한 레지스트리](./images/windows_subsystems_registry.png)

## 윈도우 서브시스템
**윈도우 서브시스템**(Windows subsystem)은 윈도우 NT의 핵심 요소 중 하나이며, 현재로써 유일하게 남은 환경 서브시스템이다. 프로그램 창을 포함한 화면 및 장치 입출력(마우스 커서 움직임, 키보드 자판 입력 등)을 처리하고 관리하는 기초적이지만 필수적인 역할을 수행하며, 다른 환경 서브시스템도 이러한 기능을 사용하려면 반드시 윈도우 서브시스템을 거쳐야 한다.

윈도우 서브시스템은 csrss.exe 프로세스와 서브시스템 DLL 외의도 다음 구성원이 존재한다.

* Csrss.exe는 해당하는 세션에 다음 네 개의 DLL을 로드한다: Basesrv.dll, winsrv.dll, sxssrv.dll 그리고 csrsrv.dll. 이들은 프로세스 및 스레드 생성과 종료를 처리하거나 윈도우 메시지를 전달하는 등의 세션 운영에 필요한 기초적인 기능들을 제공한다.

* Win32k.sys는 윈도우 서브시스템에서 핵심되는 기능이라고 소개한 장치 입력 및 프로그램 창을 처리하는 데 사용되는 커널 모드 장치 드라이버이다. 아래는 해당 장치 드라이버에 포함된 구성들을 소개한다.

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Win32k.sys 장치 드라이버의 구성</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">구성</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Window_manager">창 관리자</a><br/>(Windows Manager)</td><td>바탕화면, 프로그램 창을 포함한 GUI 요소들을 생성하고 화면에 표시한다(콘솔 제외). 마우스 및 키보드 등의 장치 입력을 수신받아 GUI 요소에 상호작용 이벤트 처리에도 관여한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Graphics_Device_Interface">그랙픽 장치 인터페이스</a><br/>(Graphics Device Interface; GDI)</td><td>직선, 곡선, 도형 등의 그래픽 요소의 생성 및 조작, 폰트 렌더링, 색상 관리 등을 담당하는 함수나 서비스르 제공한다.</td></tr></tbody></table>

* Conhost.exe, 일명 "콘솔 호스트 프로세스"는 윈도우 7부터 소개되어 콘솔 창을 생성 및 관리를 담당한다. 본래 csrss.exe 직접 처리하였으나, 로컬 시스템 계정으로 실행된 서브시스템 프로세스로부터 일반 사용자에게 부여되어서 안 될 권한이 주어질 수 있는 보안 문제를 야기할 수 있어 소개되었다.

    > 한편, 윈도우 8부터 소개된 콘솔 드라이버 condrv.sys의 역할은 키보드 입출력을 곧바로 conhost.exe로 전달한다. 이전에는 csrss.exe 및 win32k.sys를 거쳐 이벤트를 전달하는 형식으로 키보드 입력이 반영되었던 절차를 대폭 간소화하여 성능 개선에 이바지한다.

* [데스크탑 창 관리자](https://en.wikipedia.org/wiki/Desktop_Window_Manager)(Desktop Windows Manager; dwm.exe)는 데스크탑의 모든 그래픽 요소들을 관리하는 윈도우 운영체제의 구성 중 하나이다. 여기서 "데스크탑"이란, 사용자가 컴퓨터를 시작할 때 화면에 접하는 GUI를 가리키며 파일 탐색기, 작업 표시줄, 시작 메뉴 등으로 이루어져 있다.

위와 같은 구성과 기능을 갖춘 윈도우 서브시스템은 결국 모든 세션에서 반드시 필요한 존재로, 작업 관리자에서는 열린 세션 개수만큼 csrss.exe가 실행되고 있는 걸 찾아볼 수 있다. 심지어 [윈도우 서비스](Service.md) 전용인 ID 0의 세션도 콘솔 작업이 동반되므로 윈도우 서브시스템이 필요하다. 또한 대부분의 핵심 기능들은 커널 모드에서 동작하므로, 윈도우 어플리케이션을 실행하는 거 자체에서는 [컨텍스트 교환](Processor.md#컨텍스트-교환)이 거의 이루어지지 않는다.

### 네이티브 이미지
**[네이티브 이미지](https://en.wikipedia.org/wiki/Native_(computing)#Applications)**(native image)는 어떠한 환경 서브시스템에도 종속되지 않은 실행 프로그램을 가리킨다. 다시 말해, [윈도우 서브시스템](#윈도우-서브시스템)의 kernel32.dll 혹은 User32.dll 등을 필요하지 않으며 ntdll.dll에서 제공하는 네이티브 API를 곧바로 호출한다. 네이티브 API는 대체로 문서화되어 있지 않으므로, 네이티브 이미지는 일반적으로 마이크로소프트에서 빌드한 프로그램이다.

네이티브 이미지의 대표적인 예시는 바로 윈도우에서 가장 최초로 실행되는 [프로세스](Process.md)인 smss.exe 세션 관리자이다. 부팅 과정 중 초반에는 윈도우 서브시스템이 실행되지 않았기 때문에 일부 프로세스는 네이티브 이미지로 설계되어야 할 필요가 있다.

## 리눅스용 윈도우 서브시스템
**[리눅스용 윈도우 서브시스템](https://learn.microsoft.com/en-us/windows/wsl/)**(Windows Subsystem for Linux), 일명 [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux)은 아래에 서술된 기존 [POSIX](https://en.wikipedia.org/wiki/POSIX) [환경 서브시스템](#환경-서브시스템)이 가진 기술적 문제들을 극복하기 위해 새롭게 설계된 서브시스템이다.

1. 어플리케이션이 POSIX 서브시스템에서 실행되어야 하는 걸 윈도우가 인지하기 위해 [PE 포맷](https://en.wikipedia.org/wiki/Portable_Executable)으로 새롭게 빌드되어야 한다.
2. 타 운영체제 프로그램을 윈도우 서브시스템으로 [래핑](https://en.wikipedia.org/wiki/Wrapper_library)(wrapping)하는 방식을 택하였으나, 알아채기 힘든 호환성 결함이 야기될 수 있다.

[윈도우 10](Windows.md) 모바일 운영체제에서 안드로이드 앱을 실행할 수 있도록 추진된 [아스토리아 프로젝트](https://en.wikipedia.org/wiki/Windows_10_Mobile#Project_Astoria)(이후 Windows Subsystem for Android로 소개)를 발판으로 POSIX/UNIX가 아닌 "순수" [리눅스](https://en.wikipedia.org/wiki/Linux) 어플리케이션을 지원하고자 [우분투](https://en.wikipedia.org/wiki/Ubuntu)(Ubuntu) 배포판을 개발한 [캐노니컬](https://en.wikipedia.org/wiki/Canonical_(company))과 합작한 결과물이다.

![WSL2로 설치된 <a href="https://en.wikipedia.org/wiki/Debian">데비안</a> 배포판의 터미널과 파일 탐색기](./images/wsl_terminal_debian.png)

WSL은 두 종류의 아키텍처 설계가 있어 각각 장단점을 가진다. 자세한 내용은 [마이크로소프트 문서](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)를 참고한다.

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">WSL 버전 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">WSL1</th><th style="text-align: center;">WSL2</th></tr></thead><tbody style="text-align: center;"><tr><td>리눅스 시스템 서비스를 처리할 Pico 제공자(Pico provider)란 특수한 커널 모드 드라이버를 사용하지만, 완전한 호환성을 제공하지 못한다.</td><td>경량화된 가상머신을 사용하여 완전한 리눅스 커널 및 GUI 지원이 가능하나, 윈도우 NTFS 접근에는 다소 성능 저하가 나타난다.</td></tr></tbody></table>
