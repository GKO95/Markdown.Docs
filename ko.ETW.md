---
category: 윈도우
title: 윈도우 이벤트 추적
---
# 윈도우 이벤트 추적
[윈도우 이벤트 추적](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/event-tracing-for-windows--etw-)(Event Tracing for Windows; ETW)은 운영체제 또는 프로그램을 진단할 수 있도록 설계된 윈도우 OS에 구현된 체계이다. 진단 대상 프로그램에 구축된 ETW 세션을 통해 이벤트가 로그 파일로 저장되는 방식으로 동작한다. ETW는 전체적인 시스템 성능에 미치는 영향을 최소화하도록 최적화가 되었다.

> ETW에서 소개하는 이벤트는 [윈도우 이벤트 로그](https://learn.microsoft.com/en-us/windows/win32/wes/windows-event-log)와 상관없는 전혀 다른 존재이다.

다음은 ETW 구조를 구성하는 요소들을 소개한다:

* **[이벤트 제공자](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-provider)**(trace provider)

    ETW 이벤트를 생성하는 구성요소를 가리킨다. [`EventWrite`](https://learn.microsoft.com/en-us/windows/win32/api/evntprov/nf-evntprov-eventwrite) 등의 API를 활용하여 이벤트를 제공하며, ETW 런타임은 제공자로부터 전달받은 이벤트를 이진 데이터로 취급한다. 어떠한 데이터라도 ETW 이벤트에 기록될 수 있지만, 마이크로소프트에서 정의한 인코딩 규칙을 준수하지 않은 자체적인 인코딩 방식을 채택하면 이에 따른 자체적인 디코딩 도구를 활용해야 한다.

* **[이벤트 세션](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-session)**(trace session)

    어떤 ETW 제공자와 이벤트를 수용할 건지, 이들을 어떤 방식으로 저장할 건지 (파일과 [메모리](ko.Memory.md#메모리) [버퍼](https://ko.wikipedia.org/wiki/버퍼_(컴퓨터_과학)) 중 선택, [순환형](https://ko.wikipedia.org/wiki/원형_버퍼) 여부 등), 그리고 저장되는 [로그 파일](#이벤트-추적-로그) 경로는 어디인지 등의 ETW 구성을 정의한 [세션](https://ko.wikipedia.org/wiki/세션_(컴퓨터_과학))이다. 다음은 ETW 이벤트 세션의 두 가지 유형을 소개한다:

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">ETW 이벤트 세션의 유형</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">개인 세션(private session)</th><th style="text-align: center;">전역 세션(global session)</th></tr></thead><tbody><tr><td><a href="ko.Processor.md#보호-링">사용자 모드</a>의 <a href="ko.Process.md#프로세스">프로세스</a>에 국부적인 ETW 세션, 즉 프로세스 내의 이벤트만 다루며 개당 최대 4개로 제한된다.</td><td>아무런 제공자의 이벤트를 수신받을 수 있는 ETW 세션이며, 실행될 수 있는 전역 세션이 최대 64개로 제한된다.</td></tr></tbody></table>

    상당수의 ETW 세션은 윈도우 OS에 의해 시작 및 제어되어 시스템에 항상 실행되지만, 그 외의 나머지는 필요에 따라 세션이 시작된다. 아래의 레지스트리 키를 추가하여 OS 부팅 시 자동으로 ETW 세션을 생성하도록 요청할 수 있다.

    ```terminal
    HKLM\SYSTEM\CurrentControlSet\Control\WMI\Autologger
    ```

* **[이벤트 제어자](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-controller)**(trace controller)

    ETW 세션을 생성하고 제어할 수 있는 도구를 가리키며, 대표적으로 마이크로소프트의 [Xperf](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/hh162920(v=win.10)), [Logman](#logman), [Tracelog](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/tracelog) 등이 있다. ETW 제어자는 이벤트 세션을 생성할 때 저장 방식이나 로그 파일 경로 등의 구성을 지정할 수 있다.

주의해야 할 점은 ETW를 기반하여 시스템을 제어하려는 목적으로 사용되어서는 절대 안된다. 이벤트가 너무 빨리 일어나거나, 입출력 오류가 발생하거나, 시스템 메모리가 부족하는 등의 특정 상황에서 이벤트가 소실될 수 있기 때문이다.

## 이벤트 추적 로그
[이벤트 추적 로그](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-log)(event trace log; ETL)는 하나 이상의 ETW 세션에서 생성된 메시지가 압축되어 저장된 .etl 확장자 파일이다. 텍스트 로그에 비해 용량이 작으며, 프로그램에 의해 간편하게 분석될 수 있는 장점을 지닌다. 하지만 직접 분석하려면 텍스트 형식으로 변환되어야 하며, 이때 해당 ETL 로그를 생성한 윈도우 구성요소의 [심볼](ko.Symbol.md) 파일이 필요하다.

ETL을 분석할 수 있는 대표적인 도구 중 하나가 바로 [Windows Performance Analyzer](https://learn.microsoft.com/en-us/windows-hardware/test/wpt/windows-performance-analyzer)이다.
