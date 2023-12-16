# 윈도우 이벤트 추적
**[윈도우 이벤트 추적](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/event-tracing-for-windows--etw-)**(Event Tracing for Windows; ETW)은 [윈도우](Windows.md) [OS](https://en.wikipedia.org/wiki/Operating_system)의 [커널](Kernel.md) 또는 [프로그램](Process.md)이 수행한 작업들을 살펴볼 수 있도록 구현된 기술이다. 컴파일되어 운영 중인 환경에서 프로그램 동작을 진단할 수 있기 때문에 개발 및 디버깅 등의 목적으로 활용된다.

* **추적 이벤트**(trace event): 커널 또는 프로그램이 수행한 개별 동작
* **추적 메시지**(trace message): [세션](#추적-세션)이 구축되었을 때, 발생한 이벤트에 대응하여 보고되는 알림

> 프로그램을 제어하려는 목적으로 ETW의 이벤트를 활용하면 절대 안된다. 입출력 오류, 메모리 부족 등의 경우에서 이벤트가 소실될 수 있기 때문이다.

아래는 ETW의 구성 및 역할, 그리고 상호관계를 소개하여 전반적인 프레임워크 원리를 설명한다.

### 추적 제공자
**[추적 제공자](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-provider)**(trace provider)는 어플리케이션 및 드라이버의 코드로 내재되어 ETW의 추적 메시지 (또는 이벤트)를 생성하는 구성요소이다. 각 추적 제공자마다 32바이트의 [control GUID](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/control-guid)와 명칭이 함께 정의된다. 해당 [GUID](https://learn.microsoft.com/en-us/windows/win32/api/guiddef/ns-guiddef-guid)로부터 활성화 할 추적 제공자를 선택하여 메시지 (또는 이벤트)를 생성하도록 한다.

> 추적 제공자 목록은 [세션](#추적-세션)을 구축하는 [추적 제어자](#추적-제어자) 역할의 프로그램 또는 도구를 통해 확인할 수 있다.

<table style="width: 80%; margin: auto;"><caption style="caption-side: top;">추적 제공자의 control GUID 및 명칭 예시</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">GUID</th><th style="text-align: center;">제공자</th></tr></thead><tbody><tr><td style="text-align: center;">A0E9B465-B939-57D7-B27D-95D8E925FF57</td><td style="text-align: center;">Application Error</td></tr><tr><td style="text-align: center;">A68CA8B7-004F-D7B6-A698-07E2DE0F1F5D</td><td style="text-align: center;">Microsoft-Windows-Kernel-General</td></tr><tr><td style="text-align: center;">0EAD09BD-2157-539A-8D6D-C87F95B64D70</td><td style="text-align: center;">Windows Error Reporting</td></tr></tbody></table>

### 추적 소비자
**[추적 소비자](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-consumer)**(trace consumer)는 [제공자](#추적-제공자)로부터 수신한 추적 메시지를 사용자가 읽을 수 있는 서식으로 변환하는 어플리케이션 및 도구를 가리킨다. 다음은 추적 소비자 역할을 수행할 수 있는 프로그램 및 도구를 소개한다.

* [Tracefmt](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/tracefmt)
* [TraceView](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/traceview)
* [Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer)
* [Windows Performance Analyzer](WPA.md)

### 추적 제어자
**[추적 제어자](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-controller)**(trace controller)는 [세션](#추적-세션)을 관리하는 프로그램 및 도구를 가리킨다: [추적 제공자](#추적-제공자) 활성화, 세션을 구성하고 추적 시작 및 중단, 그리고 세션을 탐색하고 속성을 업데이트 할 수 있다. 다음은 추적 제어자 역할을 수행할 수 있는 프로그램 및 도구를 소개한다.

* [Tracelog](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/tracelog)
* [TraceView](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/traceview)
* [Logman](Performance_Monitor.md#logman)
* [Windows Performance Recorder](WPR.md) (또는 [Xperf](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/hh162920(v=win.10)))

## 추적 세션
**[추적 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-session)**(trace session)은 [추적 제공자](#추적-제공자)가 메시지를 생성하는 기간을 가리킨다. 그 동안 시스템은 [추적 로그](#이벤트-추적-로그) 또는 [소비자](#추적-소비자)로 전달("flushed")될 때까지 추적 메시지를 저장할 [버퍼](https://en.wikipedia.org/wiki/Data_buffer) 집합들을 운영한다.

윈도우 10, 버전 1709 이전에는 동시에 운영할 수 있는 추적 세션이 최대 64개로 고정되었다. 이후에 `HKLM\SYSTEM\CurrentControlSet\Control\WMI` 레지스트리 키의 *EtwMaxLoggers*란 REG_DWORD 값을 통해 32 - 256 범위 내로 변경할 수 있다. 만일 운영 중인 세션이 한계치에 도달하면 ([NT 커널 로거](#nt-커널-로거-추적-세션) 및 [전역 로거](#전역-로거-추적-세션) 추적 세션 포함) [ERROR_NO_SYSTEM_RESOURCES](https://learn.microsoft.com/en-us/windows/win32/api/evntrace/nf-evntrace-starttracea) 오류가 반환된다.

다음은 추적 세션의 기초적인 세 가지 유형을 소개한다:

* **[추적 로그 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-session#trace-log-sessions)**(trace log sessions)

    추적 세션의 기본 표준 유형으로, 버퍼에 담긴 메시지가 이진 형식으로 로그 파일로 작성된다.

* **[실시간 추적 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-session#real-time-trace-sessions)**(real-time trace sessions)

    추적 세션의 버퍼에 담긴 메시지는 로그 파일로 저장되지 않은 대신 (혹은 로그 파일로 저장되는 동시에) 곧바로 TraceView 또는 Tracefmt 등의 [추적 소비자](#추적-소비자)로 직접 전달된다.

* **[버퍼된 추적 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-session#buffered-trace-sessions)**(buffered trace sessions)

    추적 세션이 유지되는 동안 메시지는 버퍼에만 남아 있으며 외부(즉, 로그 파일 및 추적 소비자)로 전달되지 않는다. [원형 버퍼](https://en.wikipedia.org/wiki/Circular_buffer) 특성을 지니기 때문에, 용량이 찼다면 최신 메시지는 가장 오래된 메시지를 덮어씌우는 방식으로 관리된다. 때문에 오버헤드가 가장 작은 추적 세션 유형이다. [WinDbg](WinDbg.md) 등의 디버거를 통해 당장 버퍼에 담긴 메시지를 보거나([`!wmitrace`](https://learn.microsoft.com/en-us/windows-hardware/drivers/debuggercmds/wmi-tracing-extensions--wmitrace-dll-) 참고) 저장할 수 있다.

### NT 커널 로거 추적 세션
**[NT 커널 로거 추적 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/nt-kernel-logger-trace-session)**(NT Kernel Logger trace session)은 윈도우 [NT 커널](Kernel.md#nt-커널)의 이벤트를 추적하는 예약된, 즉 윈도우에 이미 내장되어 별도로 관리되는 세션이다. 커널 모드 [드라이버](Driver.md)나 사용자 모드의 [어플리케이션](Process.md)의 추적 제공자 로깅을 불허하며, 오로지 커널의 추적 제공자들만 허용한다 (참고: [NT 커널 로거 상수](https://learn.microsoft.com/en-us/windows/win32/etw/nt-kernel-logger-constants)). 한편, NT 커널의 추적 제공자는 타 추적 세션에 로깅될 수 없다. NT 커널 로거 추적 세션에 한하여 최대 여덟 개까지 동시에 운영할 수 있다.

해당 세션은 "NT Kernel Logger"란 이름으로 불리며, 제공자 GUID는 "[{9e814aad-3204-11d2-9a82-006008a86939}](https://learn.microsoft.com/en-us/windows/win32/etw/msnt-systemtrace)" SystemTraceControlGuid 상수이다. 그리고 [EVENT_TRACE_PROPERTIES](https://learn.microsoft.com/en-us/windows/win32/api/evntrace/ns-evntrace-event_trace_properties) 구조체의 EnableFlags 맴버에 플래그를 추가하는 것으로 SystemTraceControlGuid로부터 파생된 [프로세스](https://learn.microsoft.com/en-us/windows/win32/etw/process), [디스크](https://learn.microsoft.com/en-us/windows/win32/etw/diskio) 및 [파일 입출력](https://learn.microsoft.com/en-us/windows/win32/etw/fileio) 등 원하는 커널 제공자를 제어할 수 있다.

## 이벤트 추적 로그
[이벤트 추적 로그](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-log)(event trace log; ETL)는 하나 이상의 ETW 세션에서 생성된 메시지가 압축되어 저장된 .etl 확장자 파일이다. 텍스트 로그에 비해 용량이 작으며, 프로그램에 의해 간편하게 분석될 수 있는 장점을 지닌다. 하지만 직접 분석하려면 텍스트 형식으로 변환되어야 하며, 이때 해당 ETL 로그를 생성한 윈도우 구성요소의 [심볼](Symbol.md) 파일이 필요하다.

ETL을 분석할 수 있는 대표적인 도구 중 하나가 바로 [Windows Performance Analyzer](https://learn.microsoft.com/en-us/windows-hardware/test/wpt/windows-performance-analyzer)이다.
