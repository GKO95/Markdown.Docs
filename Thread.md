# 스레드
**[스레드](https://en.wikipedia.org/wiki/Thread_(computing))**(thread)는 [프로세스](Process.md)에 로드된 프로그램 이미지의 코드를 실행하기 위해 [프로세서](Processor.md)에서 처리하는 작업 흐름의 단위이다. 프로세스는 두 개 이상의 스레드를 활용할 수 있으며, 동일한 [가상 주소 공간](Process.md#가상-주소-공간)에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 단, 스레드 추가 여부는 프로그램 개발 당시 설계에 따라 결정되고 배포된 이후로 임의 추가할 수 있는 게 아니다.

프로세스가 생성되면 코드 실행을 위해 기본적으로 한 개의 스레드가 함께 생성되는데, 이를 **기본 스레드**(primary thread)라고 부른다. 기본 스레드는 프로그램을 본격적으로 시작하는 [진입 함수](C.md#진입점)를 실행하기 때문에, 정상적인 기본 스레드의 종료는 결과적으로 프로세스 종료를 초래한다.

### 스레드 컨텍스트
**스레드 컨텍스트**(thread context)

## 스레드 로컬 스토리지
**[스레드 로컬 스토리지](https://en.wikipedia.org/wiki/Thread-local_storage)**(thread local storage; TLS)

## 호출 스택
**[호출 스택](https://en.wikipedia.org/wiki/Call_stack)**(call stack; 간단히 "**스택**")은 각 [스레드](#스레드)마다 실행 중인 프로그램의 [함수](C.md#함수)와 관련된 정보를 저장하는 [스택](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) 데이터 구조이다. 스택의 핵심 목적은 실행한 함수가 반환되었을 때 어느 코드에서 재개되어야 하는지 추적한다. 그리고 각 함수를 중심으로 연관된 데이터들의 묶음을 **스택 프레임**(stack frame)이라 부르며, 호출된 함수의 개수만큼 스택 프레임이 존재한다.

아래는 상향(↑)하는 호출 스택의 레이아웃이며, 다음 순서대로 예시의 *DrawLine* 함수와 연관된 데이터가 호출 스택에 푸쉬된다.

![호출 스택 레이아웃](https://upload.wikimedia.org/wikipedia/commons/d/d3/Call_stack_layout.svg)

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">스택 프레임의 구성 및 설명</caption><colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">스택 프레임</th><th style="text-align: center;">데이터</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td rowspan="3" style="text-align: center;"><i>DrawLine</i> 함수</td><td>1. 함수 매개변수</td><td><i>DrawLine</i> 함수를 호출한 <i>DrawSquare</i> 함수로부터 전달받은 매개변수의 인자(들)</td></tr><tr><td>2. 귀환 주소</td><td><i>DrawLine</i> 함수가 종료하면 <i>DrawSquare</i> 함수의 어느 코드에서 재개되어야 할 지 가리키는 <a href="C.md#포인터">포인터</a></td></tr><tr><td>3. 로컬 스토리지</td><td><i>DrawLine</i> 함수 내에 정의된 <a href="C.md#변수">지역 변수</a> 등</td></tr></tbody></table>

<sup>_† 실제 메모리상 호출 스택은 (위의 테이블처럼) 최상단 주소에서부터 시작하여 아래로 내려가며 쌓이는 하향(↓) 스택이다._</sup>

위의 레이아웃 표기된 추가 내용에 부연 설명은 다음과 같다.

* 스택 포인터: *호출 스택의 최상위 주소를 가리키며, 데이터가 스택에 푸쉬 혹은 팝이 되면 함께 변동된다.*
* 프레임 포인터: *스택 프레임에 해당하는 함수가 반환될 시, 스택 포인터가 되돌아올 메모리 주소를 가리킨다.*

스택 포인터는 CPU의 [SP 레지스터](Assembly.md#포인터-레지스터)에 의해 추적된다. 만일 *DrawLine* 함수의 반환으로 프레임 포인터로 되돌아올 시, 단 한번의 팝 연산으로 [IP 레지스터](Assembly.md#명령어-포인터-레지스터)는 귀환 주소를 획득하여 *DrawSquare* 함수의 코드 실행을 재개할 수 있다. 다만, 자세한 스택 레이아웃은 아키텍처별 [호출 규약](Assembly.md#호출-규약)을 살펴보도록 한다.

### 스택 오버플로우
**[스택 오버플로우](https://en.wikipedia.org/wiki/Stack_overflow)**(stack overflow)는 스택 포인터가 정해진 스택 경계를 벗어날 때 발생하는 [예외](C.md#예외-처리)이다. 기본적으로 컴파일된 프로그램의 스택 크기는 1 MB로 제한되며, 이는 4 KB 페이지 256개와 일치한다. 컴파일 옵션에서 스택의 크기를 임의로 변경할 시, 실행 파일의 [PE 포맷](https://en.wikipedia.org/wiki/Portable_Executable) 헤더에 정보가 저장된다. 그리고 [프로세스](Process.md)가 실행 파일을 [가상 주소 공간](Process.md#가상-주소-공간)으로 불러오면 PE 헤더에 저장된 스택 크기만큼 [가상 메모리](Memory.md#가상-메모리)를 [예약](Memory.md#페이지)하여 [스레드](#스레드)의 스택 영역으로 제공한다.

[Windows OS](Windows.md)는 스택 오버플로우를 아래와 같이 설명한다.

> [NTSTATUS](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) 0xC00000FD STATUS_STACK_OVERFLOW *"A new guard page for the stack cannot be created."*

만일 1 MB의 가상 메모리가 PAGE_READWRITE [보호 권한](Memory.md#페이지)으로 예약되었을 시, 스택이 시작하는 최상단 페이지와 바로 밑 주소의 [가드 페이지](Memory.md#페이지)(guard page)가 최초로 커밋된다. 데이터가 쌓여 가드 페이즈를 침범하게 되면, 알림을 받은 운영체제는 그 다음 페이지를 가드 페이지로 커밋하는 방식을 반복하여 호출 스택은 성장한다. 단, 스택의 최하단 페이지는 타 가상 메모리까지 침범할 수 있는 위험을 방지하기 위해 커밋을 아예 불허한다.

스택 오버플로우 예외는 두 번째로 낮은 주소의 가드 페이지가 침범되었을 때 발생한다. 최하단 페이지는 가드 페이지로 커밋될 수 없기 때문에, 더 이상의 가드 페이지가 생성될 수 없어 예외를 프로그램에게 전달한다. 만일 해당 예외가 발생하여도 적절한 조치가 이루어지면 프로그램은 무사히 재개될 수 있다. 그러나 예외 처리가 되어도 호출 스택이 최하단 페이지까지 침범하게 될 경우, [윈도우 오류 보고](WER.md) [서비스](Service.md)에 의해 프로세스 전체가 종료된다.

## 비동기 프로시저 호출
**[비동기 프로시저저 호출](https://learn.microsoft.com/en-us/windows/win32/sync/asynchronous-procedure-calls)**(asynchronous procedure call; APC)은 [스레드 컨텍스트](#스레드-컨텍스트)로 전환될 때 [비동기식](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming))으로 실행될 [콜백 함수](C.md#콜백-함수)들을 가리킨다. 각 스레드마다 전용 [큐](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))를 가지고 있어, APC 함수가 대기할 경우 [소프트웨어 인터럽트](Processor.md#인터럽트)를 일으켜 다음에  [스케줄링](Processor.md#스케줄링)될 때 이들을 순서대로 실행시킨다. [시스템](Kernel.md)에 의해 생성된 APC를 *커널 모드 APC*, 그리고 어플리케이션에 의해 생성되면 *사용자 모드 APC*라고 부른다.

# 스레드 풀
> 본 장은 [Windows Vista](Windows.md)부터 소개된 최신 [스레드 풀 API](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pool-api) 위주로 소개하며, 이전 윈도우 OS의 [옛 스레드 풀 API](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pooling) 내용과 혼돈하지 않도록 유의한다.

추가 스레드가 필요하면 Win32의 [CreateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread) 함수로 생성할 수 있지만, 이러한 행위는 성능적으로 부담되는 작업이다. 본 장에서 소개할 **[스레드 풀](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pools)**(thread pool)은 이를 해소하고자 미리 모아둔 "작업자 스레드"들을 관리하며, Windows OS는 각 [프로세스](Process.md)마다 한 개의 기본 스레드 풀을 제공한다. 즉, 프로그램은 [CreateThreadpool](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpool) 함수를 호출하지 않아도 시스템에서 기본적으로 제공하는 스레드 풀을 활용할 수 있다. 그러나 기본 스레드 풀을 활용하는 순간부터 해당 커널 리소스를 초기화하고 프로세스가 종료될 때까지 메모리를 점유하며 잔여하기 때문에 상황에 따라 사용 여부를 적절히 판단하도록 한다.

### 작업자 스레드
작업자 스레드(worker thread)는 스레드 풀의 [큐](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))에서 대기 중인 [작업 항목](#작업-항목)을 비동식으로 처리한다. 작업 항목의 요청을 완수한 작업자 스레드는 곧바로 파괴되지 않고 다음 요청을 기다리며 재활용된다. 다만, 경우에 따라 작업자 스레드를 관리하는 작업자 팩토리(worker factory)가 성능 효율을 위해 스레드 재활용이 아닌 파괴 및 재생성을 택할 수 있다. 작업자 팩토리 동작을 남용하는 걸 방지하기 위해 마이크로소프트는 알고리즘을 비공개하며, 작업자 스레드의 생성과 파괴는 임의로 이루어질 수 없다.

### 작업 항목
**작업 항목**(work item)은 작업자 스레드에게 처리를 요청하는 개체이며, 이를 수행하기 위해 필요한 아래 정보들을 포함한다.

1. [콜백 함수](C.md#콜백-함수) 및 전달인자: *작업자 스레드가 실행할 함수이다.*
1. [콜백 환경](#사용자-지정-스레드-풀): *어느 스레드 풀에서 작업을 처리할 지 지정한다.*
1. [입출력 완료 포트](IO.md#입출력-완료-포트) [핸들](Process.md#핸들) <sub>(TP_IO 한정)</sub>

Windows는 총 네 개의 작업 항목 유형이 존재하며, 이들의 설명은 다음과 같다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">스레드 풀의 작업 항목 유형별 설명</caption><colgroup><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 65%;"/></colgroup><thead><tr><th style="text-align: center;">작업 항목</th><th style="text-align: center;">개체명</th><th style="text-align: center;">콜백 함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">TP_WORK</td><td style="text-align: center;">작업 개체</td><td><a href="https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms687396(v=vs.85)">WorkCallback</a></td><td>아무런 조건 없이 콜백 함수를 작업자 스레드가 실행하도록 요청한다.

* [CreateThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolwork)
* [SubmitThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-submitthreadpoolwork)
* [WaitForThreadpoolWorkCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpoolworkcallbacks)
* [CloseThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolwork)

</td></tr><tr><td style="text-align: center;">TP_TIMER</td><td style="text-align: center;">타이머 개체</td><td><a href="https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms686790(v=vs.85)">TimerCallback</a></td><td>타이머가 만료될 때 콜백 함수를 작업자 스레드가 실행하도록 요청한다. 기존에 사용하였던 TP_TIMER에 새로운 타이머 설정과 함께 요청하여 재활용이 가능하다.

* [CreateThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpooltimer)
* [SetThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-setthreadpooltimer)
* [IsThreadpoolTimerSet](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-isthreadpooltimerset)
* [WaitForThreadpoolTimerCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpooltimercallbacks)
* [CloseThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpooltimer)

</td></tr><tr><td style="text-align: center;">TP_WAIT</td><td style="text-align: center;">대기 개체</td><td><a href="https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms687017(v=vs.85)">WaitCallback</a></td><td>커널 개체가 signaled 되었을 때 콜백 함수를 작업자 스레드가 실행하도록 요청한다. 기존에 사용하였던 TP_WAIT에 새로운 커널 개체의 핸들과 함께 요청하여 재활용이 가능하다.

* [CreateThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolwait)
* [SetThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-setthreadpoolwait)
* [WaitForThreadpoolWaitCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpoolwaitcallbacks)
* [CloseThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolwait)

</td></tr><tr><td style="text-align: center;">TP_IO</td><td style="text-align: center;">입출력 완료 개체</td><td><a href="https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms684124(v=vs.85)">IoCompletionCallback</a></td><td>입출력 완료 포트에 관여된 파일 핸들의 입출력 요청이 완료될 때 입출력 완료 패킷을 작업자 스레드가 실행하도록 요청한다.

* [CreateThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolio)
* [StartThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-startthreadpoolio)
* [CancelThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-cancelthreadpoolio)
* [WaitForThreadpoolIoCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpooliocallbacks)
* [CloseThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolio)

</td></tr></tbody></table>

간단한 작업을 단순히 기본 스레드 풀에서 실행하는 경우라면, Win32는 [SimpleCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms686295(v=vs.85)) 콜백 함수와 [TrySubmitThreadpoolCallback](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-trysubmitthreadpoolcallback) 함수를 통해 간소화된 작업자 스레드로의 요청 절차를 제공한다.

## 사용자 지정 스레드 풀
기본 스레드 풀 환경은 최대 500개의 작업자 스레드까지 제공할 수 있으나, 이는 시스템에서 제공하는 기본 환경 설정으로 변경이 불가하다. 성능 최적화 등의 설계상 이유로 스레드 풀의 환경을 직접 지정해야 할 경우, [InitializeThreadpoolEnvironment](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-initializethreadpoolenvironment) 함수로 초기화된 TP_CALLBACK_ENVIRON 구조체를 [CreateThreadpool](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpool)에 전달하여 스레드 풀을 생성할 때 적용시킨다. 그리고 해당 구조체를 차후 작업 항목의 `pcbe` 매개변수 인자로 전달하여 원하는 스레드 풀에서 처리될 수 있도록 지정할 수 있다.

# 파이버
**[파이버](https://learn.microsoft.com/en-us/windows/win32/procthread/fibers)**(fiber)는 단일 [사용자 모드](Processor.md#사용자-모드) [스레드](#스레드)에 의해 [스케줄링](Processor.md#스케줄링)되는 작업 흐름의 단위이다. 본래 [UNIX](https://en.wikipedia.org/wiki/Unix)에서 제작된 어플리케이션을 [Windows](Windows.md) 버전으로 아예 새로 개발하는 대신, UNIX 어플리케이션 특징을 살려 [포팅](https://en.wikipedia.org/wiki/Porting)을 지원하는 차원에서 소개되었다. 단, Windows의 네이티브 스레드를 활용한 적합한 어플리케이션 설계를 위해 파이버는 가능한 기피되어야 한다.

### 파이버 로컬 스토리지
**파이버 로컬 스토리지**(fiber local storage; FLS)는 파이버 버전의 [스레드 로컬 스토리지](#스레드-로컬-스토리지)이다.

# 스레드 생명주기
본 장은 [C](C.md) 언어로 개발된 [윈도우](Windows.md) 어플리케이션을 위주로 설명한다. 즉, [Win32 API](WinAPI.md) 이외에 [C 런타임 라이브러리](C.md#c-런타임-라이브러리)(CRT)가 함께 언급되어 개념을 설명한다.

## 스레드 생성
본래 [C 표준 라이브러리](https://en.wikipedia.org/wiki/C_standard_library)는 단일 스레드 프로그램을 위해 설계되어 다중 스레딩을 지원하는 매커니즘이 결여된다. [C 런타임 라이브러리](C.md#c-런타임-라이브러리)(일명 CRT)는 이를 보완하기 위해 [스레드 컨텍스트](#스레드-컨텍스트)를 저장할 `_tiddata` [구조체](C.md#구조체)를 [TLS](#스레드-로컬-스토리지)에 초기화하여 제공한다. CRT의 [_beginthread](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex) (혹은 [_beginthreadex](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex)) 함수는  Win32의 [CreateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread)를 내포하는 동시에 `_tiddata` 구조체를 초기화하는 등 다중 스레딩에 필요한 추가 리소스를 확보한다.

CreateThread가 실행되면 먼저 스레드 [커널 개체](Kernel.md#커널-개체)의 (대부분) 맴버들을 초기화하고 [스레드 스택](#스레드-스택)을 위한 [메모리](Memory.md) 영역을 할당한다. 스레드 스택의 최상단 주소에는 매개변수 `lpParameter` 및 `lpStartAddress` 인자를 순서대로 푸쉬하며, 다음은 이들 인자들이 무엇인지 소개한다.

* `lpParameter`: 스레드로 전달할 변수의 포인터
* `lpStartAddress`: 스레드가 실행할 프로그램 정의 함수의 포인터

마지막으로 스레드 커널 개체의 맴버 중 컨텍스트 관련 맴버들을 초기화한다: (1) [SP](Assembly.md#포인터-레지스터)에는 스택상 `lpStartAddress` 인자가 위치한 포인터, 그리고 (2) [IP](Assembly.md#명령어-포인터-레지스터)에는 스레드를 시작할 [RtlUserThreadStart](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/xperf/thread-start-functions) 함수의 포인터로 할당된다. 이대로 [스케줄링](Processor.md#스케줄링)되면 [프로세서](Processor.md#프로세서)는 스레드에 할애된 프로그램 정의 함수를 실행하게 된다.

## 스레드 종료
스레드를 종료하려면 대응하는 CRT의 [_endthread](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/endthread-endthreadex) (혹은 [_endthreadex](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/endthread-endthreadex)) 함수를 호출하거나, 또는 프로그램 정의 함수가 반환하여 종료될 시 자동적으로 호출된다. CRT 리소스 정리를 마친 다음, 내포된 Win32의 [ExitThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread) 함수 실행을 마지막으로 스택 메모리 해제 및 스레드 상태 코드를 반환한다. 이때 스레드 커널 개체의 참조 카운트가 차감되어, 만일 카운트가 0에 달하면 개체는 OS에 의해 자동 소멸된다.

### 잘못된 스레드 종료 방법
스레드를 종료하는 가장 올바른 방법은 프로그램 정의 함수가 [반환문](C.md#return-반환문) 혹은 CRT의 _endthread (혹은 _endthreadex) 함수를 호출하여 종료되는 것이다. 그 외에 스레드를 종료하는 방법이 몇 가지 존재하며 이들의 문제점을 함께 설명한다.

1. *스레드에서 Win32의 [ExitThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread) 함수 호출* : 프로그램을 C/C++ 코드로 작성하고 있다면 리소스 정리를 위해 가급적 CRT 함수를 호출한다.

1. *본 프로세스 (혹은 타 프로세스)에서 Win32의 [TerminateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminatethread) 함수 호출* : 해당 스레드의 프로세스가 종료되지 않는 이상, 시스템은 스레드 스택을 제거하지 않는다. 이는 마이크로소프트의 의도된 설계로, 스레드 스택을 제거하면서 유발될 [메모리 접근 오류](https://en.wikipedia.org/wiki/Segmentation_fault)를 방지하는 차원의 조치이다.
