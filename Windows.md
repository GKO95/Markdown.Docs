# 윈도우 NT
[윈도우 NT](https://ko.wikipedia.org/wiki/윈도우_NT)(Windows NT)는 [마이크로소프트](https://ko.wikipedia.org/wiki/마이크로소프트)에서 개발한 [윈도우](https://ko.wikipedia.org/wiki/마이크로소프트_윈도우)(Windows) 계열 중 하나이며, 전세계적으로 가장 널리 사용되고 있는 현역 [운영체제](https://ko.wikipedia.org/wiki/운영체제)(operating system; OS)이다. 본래 NT는 기존 운영체제인 [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS)를 대체할 "신기술(New Technology)"을 염두하여 붙인 이름이지만, 현재는 아무런 의미가 없는 제품군 명칭으로 자리잡았다. 이름은 다소 생소할 수 있으나 사실상 배포되고 있는 거의 모든 윈도우 제품이 윈도우 NT 계열에 해당한다.

다음은 2023년 3월 21일 기준에 작성된 윈도우 NT 제품 목록이다.

<table style="margin-bottom: 16px; table-layout: fixed; width: 100%; margin-left: auto; margin-right: auto;">
<thead><tr><th colspan="7" style="text-align: center;">윈도우 NT 클라이언트 (2001년 ~ 현재)</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://ko.wikipedia.org/wiki/윈도우_XP">윈도우 XP</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_비스타">윈도우 비스타</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_7">윈도우 7</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_8">윈도우 8</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_8.1">윈도우 8.1</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_10">윈도우 10</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_11">윈도우 11</a></td></tr></tbody>
</table>

<table style="margin-top: 16px; table-layout: fixed; width: 100%; margin-left: auto; margin-right: auto;">
<thead><tr><th colspan="6" style="text-align: center;">윈도우 NT 서버 (2001년 ~ 현재)</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word; text-align: center;"><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2003">윈도우 서버 2003</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2008">윈도우 서버 2008</a> (<a href="https://ko.wikipedia.org/wiki/윈도우_서버_2008_R2">R2</a>)</td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2012">윈도우 서버 2012</a> (<a href="https://ko.wikipedia.org/wiki/윈도우_서버_2012#윈도우_서버_2012_R2">R2</a>)</td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2016">윈도우 서버 2016</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2019">윈도우 서버 2019</a></td><td><a href="https://ko.wikipedia.org/wiki/윈도우_서버_2022">윈도우 서버 2022</a></td></tr></tbody>
</table>

> 윈도우 NT와 함께 서비스 중인 운영체제로 [윈도우 IoT](https://ko.wikipedia.org/wiki/윈도우_IoT)가 있으며, 단종된 윈도우 제품으로 [MS-DOS](https://github.com/microsoft/MS-DOS), [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x), [윈도우 모바일](https://ko.wikipedia.org/wiki/윈도우_모바일), 그리고 [윈도우 폰](https://ko.wikipedia.org/wiki/윈도우_폰)이 있다.

비록 윈도우 NT는 클라이언트와 서버로 운영체제가 구분되지만 동일한 빌드의 커널을 사용한다. 예시로 윈도우 8.1과 윈도우 서버 2012 R2 운영체제의 커널은 빌드 9600으로 일치한다. 핵심 커널은 같지만 두 운영체제로 나뉘어진 이유는 성능 최적화 대상과 목적, 그리고 이에 따른 기술 사양과 기능에 차이가 존재하기 때문이다.

마이크로소프트의 운영체제가 윈도우 NT로 전환한 것은 다음 기술적 의의를 가진다:

* 완전한 [32비트](https://ko.wikipedia.org/wiki/32비트) 아키텍처 지원
* 다양한 하드웨어 [이식성](#하드웨어-이식성) 및 소프트웨어 [호환성](#환경-서브시스템) 지원
* 신뢰성을 보장하도록 [커널](Processor.md#권한-수준) 및 [사용자 모드](Processor.md#권한-수준)의 구분 그리고 [메모리](Memory.md)의 [가상화](Process.md#가상-주소-공간)

# 윈도우 아키텍처
아래 그림은 윈도우 7 (또는 윈도우 서버 2008 R2) 운영체제의 아키텍처를 나타낸 다이어그램이다.

![윈도우 7 및 서버 2008 R2 아키텍처 (Windows Internals, 6th Edition)](https://raw.githubusercontent.com/LordNoteworthy/windows-internals/master/windows-internals-6th-ed/assets/windows-architecture.png)

다음은 일부 윈도우 아키텍처 구성 중에서 개별 문서로 작성된 것을 나열한다:

* [윈도우 서비스](Service.md)
* [환경 서브시스템](Subsystem.md#환경-서브시스템) 및 [시스템 서비스](WinAPI.md#시스템-서비스)
* [윈도우 커널](Kernel.md#NT-커널) 및 [드라이버](Driver.md)

# 운영체제 리소스
비록 본 문서에서 소개하는 내용들은 윈도우 NT 운영체제를 위주로 다루고 있으나, 전반적인 이해를 돕기 위해 아래에 나열된 시스템의 핵심 리소스에 대하여 살펴보기를 권장한다.

* [프로세서](Processor.md)
* [메모리](Memory.md)
* [저장소](Storage.md)
* [네트워크](Network.md)

# 관리 프로그램
다음은 시스템 상태를 모니터링 및 관리할 수 있는 프로그램을 나열한다.

* [성능 모니터](Perfmon.md)
* [이벤트 뷰어](Event_Viwer.md)
* [파워셸](PowerShell.md)
