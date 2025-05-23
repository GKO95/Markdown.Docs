# 비동기 입출력
[입출력 동기화](https://learn.microsoft.com/en-us/windows/win32/fileio/synchronous-and-asynchronous-i-o)에는 두 가지 유형이 존재한다:

![동기 및 비동기 입출력](https://learn.microsoft.com/en-us/windows/win32/fileio/images/fig2bedit.png)

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">입출력 동기화의 두 유형</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">동기 입출력</th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Asynchronous_I/O">비동기 입출력</a></th></tr></thead><tbody><tr style="text-align: center;"><td>Synchronous I/O</td><td>Asynchoronous I/O (aka. <i><a href="#overlapped-구조체">Overlapped I/O</a></i>)</td></tr><tr><td>입력을 요청한 <a href="Processor.md#사용자-모드">사용자 모드</a> 스레드는 <a href="Processor.md#커널-모드">커널 모드</a>에서 요청 처리를 완료할 때까지 대기 상태에 진입하여 기다린다.</td><td>입력을 요청한 <a href="Processor.md#사용자-모드">사용자 모드</a> 스레드는 <a href="Processor.md#커널-모드">커널 모드</a>에서 요청 처리를 진행하는 동안 다른 작업을 처리할 수 있다.</td></tr><tr><td><i>N/A</i>&nbsp;<sub>(동기 입출력은 작업이 완료될 때까지 기다려야 하기 때문에, 완료 여부를 알려야 할 필요가 없다)</sub></td><td>비동기 입출력 작업의 완료를 알리는 방법들을 나열한다:

* [장치 커널 개체](Driver.md#디바이스-개체)
* [이벤트 커널 개체](Synchronization.md#이벤트-개체)
* [알림 가능한 입출력](#알림-가능한-입출력)
* [입출력 완료 포트](#입출력-완료-포트)

</td></tr></tbody></table>

비동기 입출력 요청을 완료한 커널 모드 [스레드](Thread.md)는 *signaled* 되어 알리고, 사용자 모드 스레드는 해당 입출력 완료에 대한 추가적인 조치를 취한다. 단, 성능 향상에 도움이 된다면 비동기 입출력은 반드시 FIFO 방식에 따라 처리하지 않는다.

### OVERLAPPED 구조체
[OVERLAPPED](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-overlapped)는 비동기 입출력에 필요한 정보를 담는 [구조체](C.md#구조체)이다.

## 알림 가능한 입출력
**[알림 가능한 입출력](https://learn.microsoft.com/en-us/windows/win32/fileio/alertable-i-o)**(alertable I/O)은 [사용자 모드](Processor.md#사용자-모드)의 [스레드](Thread.md)가 오로지 "알림 가능한" 상태일 때 [APC](Thread.md#비동기-프로시저-호출)를 활용하여 큐에 대기 중인 [콜백 함수](C.md#콜백-함수)(일명 완료 루틴)를 비동기식으로 처리하는 매커니즘이다. 다음은 알림 가능한 입출력에 활용되는 Win32 API 목록을 소개한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">알림 가능한 입출력 관련 Win32 API</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">APC 함수 대기열 큐잉</th><th style="text-align: center;">알림 가능한 상태 전환</th></tr></thead><tbody><tr><td>APC 함수가 큐잉되면 <a href="Processor.md#인터럽트">소프트웨어 인터럽트</a>를 일으켜 대기 중인 완료 루틴이 존재함을 알린다.</td><td>만일 APC 큐가 비었을 시, (1) 대기 대상의 <a href="Synchronization.md#대기-함수">커널 개체</a>가 <i>signaled</i> 되거나 (2) APC가 큐잉될 때까지 스레드 실행이 중단된다.</td></tr><tr><td>

* [QueueUserAPC](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-queueuserapc)
* [ReadFileEx](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-readfileex)
* [WriteFileEx](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-writefileex)
* [SetWaitableTimer](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setwaitabletimer)
* [SetWaitableTimerEx](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setwaitabletimerex)

</td><td>

* [SleepEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-sleepex)
* [WaitForSingleObjectEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [WaitForMultipleObjectsEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-waitformultipleobjectsex)
* [SignalObjectAndWait](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-signalobjectandwait)
* [MsgWaitForMultipleObjectsEx](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-msgwaitformultipleobjectsex)

</td></tr></tbody></table>

커널은 APC 큐에 대기 중인 완료 루틴을 꺼내어 스레드로 전송하여 콜백 함수를 실행한다. 만일 추가로 대기 중인 APC 함수가 있을 경우, 스레드는 이를 FIFO 순서대로 처리한다.<sup>[[출처](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-queueuserapc#remarks)]</sup> 그러나 알림 가능한 입출력 매커니즘은 다음 두 가지의 치명적인 단점을 가진다:

1. 완료 루틴이 콜백 함수로 실행되는 점에서 코드 구현에 상당한 제약을 지닌다.
1. 비동기 입출력을 요청한 스레드가 또한 완료 루틴을 처리해야 한다.

추후 마이크로소프트는 알림 가능한 입출력의 문제점을 보완한 "[입출력 완료 포트](#입출력-완료-포트)"를 소개하였다.

## 입출력 완료 포트
**[입출력 완료 포트](https://learn.microsoft.com/en-us/windows/win32/fileio/i-o-completion-ports)**(I/O completion ports)는 [스레드 풀](Thread.md#스레드-풀)을 활용하여 다수의 비동기 입출력 요청을 [병행](https://en.wikipedia.org/wiki/Concurrency_(computer_science))으로 처리할 수 있는 매커니즘이다. 본래 장치 입출력을 위해 설계되었지만, 비동기 입출력을 지원하는 [파일](FileSystem.md) 및 [커널 개체](Kernel.md#커널-개체)에도 적용 가능하다. 일반적으로 입출력 완료 포트의 [작업자 스레드](Thread.md#작업자-스레드) 개수를 [CPU](Processor.md#프로세서-코어)의 두 배로 설정하는 걸 권장한다.

[CreateIoCompletionPort](https://learn.microsoft.com/en-us/windows/win32/fileio/createiocompletionport) 함수로부터 생성된 입출력 완료 포트는 총 다섯 개의 제각각 역할을 지닌 자료 구조로 구성된다:

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">입출력 완료 포트의 구조</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">자료 구조</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: left;">Device List</td><td>입출력 완료 포트에 관여한 장치(혹은 파일)들의 <a href="Process.md#핸들">핸들</a> 리스트이다.</td></tr><tr><td style="text-align: left;">I/O Completion Queue</td><td>입출력 요청을 완료한 커널 모드 스레드가 전달한 입출력 완료 패킷을 FIFO 순서대로 담는 대기열이다.<ul><li>시스템은 우선 해당 장치(혹은 파일)가 포트 관여 여부를 검사한 다음 패킷을 큐에 대기시킨다.</li><li>한편, <a href="https://learn.microsoft.com/en-us/windows/win32/fileio/postqueuedcompletionstatus">PostQueuedCompletionStatus</a> 함수는 포트에 직접 입출력 완료 패킷을 큐에 대기시킨다.</li></ul></td></tr><tr><td style="text-align: left;">Waiting Thread Queue</td><td>I/O Completion Queue의 패킷을 처리할 준비가 된, 즉 <a href="https://learn.microsoft.com/en-us/windows/win32/api/ioapiset/nf-ioapiset-getqueuedcompletionstatus">GetQueuedCompletionStatus</a> 함수를 호출하여 대기 중인 스레드의 ID를 담는 LIFO 대기열이다. <ul><li>LIFO 순서는 가장 최근에 실행된 스레드를 활용하여 성능 및 메모리 효율을 높인다.</li></ul></td></tr><tr><td style="text-align: left;">Released Thread List</td><td>대기 상태에서부터 깨어난 스레드들의 ID를 담는 리스트이다. 이를 통해 입출력 완료 포트는 깨어난 스레드를 기억하고 실행 여부를 예의주시한다.</td></tr><tr><td style="text-align: left;">Paused Thread List</td><td>(<a href="Synchronization.md#대기-함수">대기 함수</a> 등에 의해) 대개 상태에 놓인 스레드들의 ID를 담는 리스트이다.</td></tr></tbody></table>

입출력 완료 포트가 생성되면 CreateIoCompletionPort 함수는 해당 포트의 핸들을 반환한다. 즉, 입출력 완료 포트는 이를 생성한 [프로세스](Process.md)에 한정되어 타 프로세스와 공유될 수 없다. 반면, 동일 프로세스의 스레드 간에 공유될 수 있어 스레드 간 통신(inter-thread communication)에도 활용된다.

### 입출력 완료 패킷
**입출력 완료 패킷**(I/O completion packet)은 [비동기 입출력](#비동기-입출력) 작업이 완료되면 이를 지정된 입출력 완료 포트로 전송하여 완료 여부를 알리는 데이터 구조이다. 아래는 입출력 완료 패킷의 구성을 소개하기 위한 [유사코드](https://en.wikipedia.org/wiki/Pseudocode)이다.

```c
typedef struct _IO_COMPLETION_PACKET {
    DWORD        dwByteTransferred;
    ULONG_PTR    dwCompletionKey;
    LPOVERLAPPED lpOverlapped;
    DWORD        dwError;
} IO_COMPLETION_PACKET, *PIO_COMPLETION_PACKET;
```
