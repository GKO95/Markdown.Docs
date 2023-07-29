---
category: Sysinternals
title: ProcDump
---
# ProcDump
[ProcDump](https://aka.ms/procdump)는 [프로세스](ko.Process)의 [사용자 모드 덤프](ko.Dump.md#사용자-모드-덤프)를 수집하는 [CLI](https://ko.wikipedia.org/wiki/명령_줄_인터페이스) 기반의 [Sysinternals](ko.Sysinternals.md) 유틸리티 프로그램이다.

![ProcDump 유틸리티 프로그램](./images/sysinternals_procdump.png)

실행 중인 어플리케이션으로부터 곧바로 덤프를 수집할 수 있으며, [충돌](https://ko.wikipedia.org/wiki/충돌_(컴퓨팅)#애플리케이션_충돌) 및 [응답 없음](https://ko.wikipedia.org/wiki/프리징_(컴퓨팅)), 그리고 [CPU](ko.Processor) 점유율이나 [메모리](ko.Memory.md) 사용량 등의 [성능 카운터](https://learn.microsoft.com/en-us/windows/win32/perfctrs/performance-counters-portal)(performance counter) 수치를 기반으로 덤프를 수집하는 조건을 설정할 수 있다. 이는 서버 관리자나 프로그램 개발자가 비정상적인 [리소스](https://ko.wikipedia.org/wiki/시스템_리소스) 활동이 발생하였을 때 원인을 판단할 수 있는 자료가 된다. 덤프 수집이 완료되면 ProcDump는 종료된다.

ProcDump의 덤프 수집에는 두 가지의 치명적인 단점이 있다:

1. 현재 실행 중이거나, 혹은 ProcDump의 `-x` 매개변수로 실행된 프로세스에만 적용된다.
2. 동명의 프로세스가 여럿 존재하면 반드시 PID를 통해 덤프를 수집하고자 하는 프로세스를 지목해야 한다.

> 이러한 단점들에 의해 실행이 되자마자 금방 종료되는 경우의 프로세스 덤프는 ProcDump로 수집이 불가능하며, 대안으로 [WER](ko.WER.md) 그리고 [Debug Diagnostic Tool v2](https://www.microsoft.com/en-us/download/details.aspx?id=103453) 프로그램이 있다.

본 문서는 분량상 ProcDump에 대한 모든 설정을 다루지 않으며, 중요하다고 판단되는 내용들을 위주로 설명한다. 자세한 내용과 예시는 [마이크로소프트 공식 문서](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)를 참고한다.

## 덤프 종류
ProcDump는 아래의 덤프 종류를 생성할 수 있다.

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">ProcDump 덤프 유형 설정 매개변수</caption>
<colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 70%;"/></colgroup>
<thead><tr><th style="text-align: center;">매개변수</th><th style="text-align: center;">덤프 종류</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><code>-ma</code></td><td style="text-align: center;"><a href="ko.Dump.md#전체-덤프">전체 덤프</a></td><td>프로세스의 <a href="ko.Process.md#가상-주소-공간">사용자 공간</a> 전체를 수집한다.</td></tr>
<tr><td style="text-align: center;"><code>-mm</code></td><td style="text-align: center;"><a href="ko.Dump.md#미니-덤프">미니 덤프</a></td><td>프로세스의 사용자 공간 중에서 <a href="ko.Process.md#스레드">스레드</a> 및 레지스터 위주로 수집하며, ProcDump의 기본 덤프이다.</td></tr>
<tr><td style="text-align: center;"><code>-mp</code></td><td style="text-align: center;">미니플러스 덤프<br/>(MiniPlus Dump)</td><td>프로세스의 프로그램 이미지를 제외한 사용자 공간 전체를 수집한다. 덤프에서 제외된 프로그램 이미지 정보는 해당 프로그램 파일이 위치한 경로를 디버깅 도구(예. <a href="ko.WinDbg.md">WinDbg</a>)에 명시하여 보충할 수 있다.</td></tr>
<tr><td style="text-align: center;"><code>-mk</code></td><td style="text-align: center;">커널 덤프</td><td>프로세스에 관여한 커널 스택의 덤프를 추가로 생성한다. 단, 해당 덤프는 절대로 <a href="ko.Dump.md#커널-메모리-덤프">커널 메모리 덤프</a>가 아니다.</td></tr></tbody></table>

## 덤프 수집 조건
ProcDump에서 덤프를 생성하는 조건을 다음과 같이 설정할 수 있다.

### 예외 처리
어플리케이션에서 처리하지 못한 [예외](ko.Cpp.md#예외-처리)(exception)가 발생, 즉 충돌이 일어나면 프로그램이 종료되기 전에 덤프를 수집하도록 하는 `-e` 매개변수가 있다. 해당 매개변수의 설명을 자세히 읽어보면 "first chance exception"이란 용어를 발견할 수 있는데, 이를 통해 예외에도 종류가 있다는 점을 확인할 수 있다.

> 만일 예외가 무엇인지 잘 모르겠으면 대표적인 예시로 `0xC0000005` [STATUS_ACCESS_VIOLATION](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) 메모리 접근 오류가 존재한다.

[디버깅](https://ko.wikipedia.org/wiki/디버그) 관점에서 예외는 두 가지로 나뉘어진다:

* **1차 시도 예외(First chance exceptions)**

    어플리케이션이 예외를 가장 처음으로 마주하였을 때를 가리킨다. 1차 시도 예외는 프로그램에 치명적인 문제가 있다는 것을 의미하지 않다; [`try`](ko.Cpp.md#try-catch-예외-처리문)-[`catch`](ko.Cpp.md#try-catch-예외-처리문) 등으로 훌륭한 예외 처리 코드가 마련되었다면, 1차 시도 예외의 발생 내역만 남긴 채 아무런 문제 없이 어플리케이션 실행을 이어간다. 디버깅 도구 설정을 통해 1차 시도 예외 때 어플리케이션 실행을 중단시켜 디버그 모드로 진입할 수 있다.

* **2차 시도 예외(Second chance exceptions)**

    어플리케이션이 1차 시도에서 예외를 처리하지 못하였을 때, 디버깅 도구에서 이를 처리할 수 있도록 주어진 두 번째 기회를 가리킨다. 어플리케이션은 실행이 중단되고 디버그 모드에 진입하는 데, 디버거에서 2차 시도 예외를 무사히 처리하였으면 어플리케이션 실행이 재개될 수 있다. 허나, 디버깅 도구가 없다면 어플리케이션은 2차 시도 예외를 처리하지 못한 채 그래도 충돌로 이어져 종료된다.

### 응답 없음
[GUI](https://ko.wikipedia.org/wiki/그래픽_사용자_인터페이스) 창이 있는 어플리케이션이 최소 5초 동안 응답 없음에 놓여진 경우에 덤프를 수집하도록 하는 `-h` 매개변수가 있다.

### 성능 카운터
어플리케이션의 리소스 사용량을 성능 카운터 기준에서 지정한 한계치를 초과하거나 미달하면 덤프를 수집하도록 할 수 있다.

<table style="width: 70%; margin: auto;">
<caption style="caption-side: top;">ProcDump 덤프 생성 기준의 성능 카운터</caption>
<colgroup><col style="width: 20%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 60%;"/></colgroup>
<thead><tr><th rowspan="2" style="text-align: center;">성능 카운터</th><th colspan="2" style="text-align: center;">매개변수</th><th rowspan="2" style="text-align: center;">비고</th></tr><tr><th style="text-align: center;">초과</th><th style="text-align: center;">미달</th></tr></thead>
<tbody>
<tr><td style="text-align: center;">CPU 점유율</td><td style="text-align: center;"><code>-c</code></td><td style="text-align: center;"><code>-cl</code></td><td>퍼센트(%) 단위를 입력한다.</td></tr>
<tr><td style="text-align: center;"><a href="ko.Memory.md#커밋된-메모리">커밋된 메모리</a></td><td style="text-align: center;"><code>-m</code></td><td style="text-align: center;"><code>-ml</code></td><td>MB 단위를 입력한다.</td></tr>
<tr><td style="text-align: center;">기타</td><td style="text-align: center;"><code>-p</code></td><td style="text-align: center;"><code>-pl</code></td><td>예시: <code>"\Processor(_Total)\% Processor Time"</code></td></tr>
</tbody>
</table>
