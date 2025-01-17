# 파일 시스템

# 파일 입출력
[입출력 동기화](https://learn.microsoft.com/en-us/windows/win32/fileio/synchronous-and-asynchronous-i-o)에는 두 가지 유형이 존재한다:

![동기 및 비동기 입출력](https://learn.microsoft.com/en-us/windows/win32/fileio/images/fig2bedit.png)

1. **동기 입출력**(synchronous I/O)

    [입출력](https://en.wikipedia.org/wiki/Input/output) 작업이 완료될 때까지 입출력 작업 요청한 스레드는 대기 상태에 진입하여 기다린다.

1. **[비동기 입출력](https://en.wikipedia.org/wiki/Asynchronous_I/O)**(asynchoronous I/O)

    입출력 작업이 완료되기 전에 다른 작업 처리를 허용한다. [윈도우 OS](Windows.md)의 [Win32 API](WinAPI.md)에서는 이를 [`OVERLAPPED`](#overlapped-구조체) 구조체로부터 구현하여 *overlapped* 입출력이라고도 칭한다. 단, 성능 향상에 도움이 된다면 비동기 입출력은 반드시 FIFO 방식에 따라 처리하지 않는다.

윈도우 OS는 비동기 입출력 작업의 완료를 알리는 네 가지 방법을 제공한다.

* [Device Kernel Object](Driver.md#디바이스-개체)
* [Event Kernel Object](Thread.md#이벤트-개체)
* [Alertable I/O](#알림-가능한-입출력)
* [I/O Completion Port](#입출력-완료-포트)

### OVERLAPPED 구조체
[OVERLAPPED](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-overlapped)는 비동기 입출력에 필요한 정보를 담는 [구조체](C.md#구조체)이다.

## 알림 가능한 입출력
**[알림 가능한 입출력](https://learn.microsoft.com/en-us/windows/win32/fileio/alertable-i-o)**(alertable I/O)은 사용자 모드 스레드가 오로지 "알림 가능한(alertable)" 상태일 때 [APC](Thread.md#비동기-프로시저-호출)로 대기 중인 비동기 입출력을 처리하는 매커니즘이다. 비동기식의 [ReadFileEx](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-readfileex) 및 [WriteFileEx](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-writefileex) 등의 함수는 [콜백 함수](C.md#콜백-함수), 일명 완료 루틴(completion routine) 포인터를 매개변수로 전달 받아 APC 큐에 대기시킨다.

스레드를 알림 가능한 상태로 전환하는 함수는 아래와 같이 한정된다:

* [SleepEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-sleepex)
* [WaitForSingleObjectEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [WaitForMultipleObjectsEx](https://learn.microsoft.com/en-us/windows/desktop/api/synchapi/nf-synchapi-waitformultipleobjectsex)
* [SignalObjectAndWait](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-signalobjectandwait)
* [MsgWaitForMultipleObjectsEx](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-msgwaitformultipleobjectsex)

이로부터 스레드가 알림 가능한 상태로 진입할 시, 커널은 먼저 해당 스레드의 APC 큐를 확인한다. 만일 APC 큐가 비었을 경우, 커널은 다음 두 사건이 일어날 때까지 스레드 실행을 중단한다.

1. [대기 함수](Thread.md#대기-함수)가 기다리는 [커널 개체](Kernel.md#커널-개체)가 *signaled* 상태로 진입
1. 스레드의 APC 큐에 콜백 함수가 대기

APC 큐에 대기 중인 완료 루틴이 있다면 커널은 이를 대기열에서 꺼내 스레드로 전송한다. 그리고 스레드는 큐로부터 수신받은 콜백 함수를 실행한다. 이러한 과정을 반복하여 APC 큐의 나머지 콜백 함수를 처리하되, 비동기 입출력의 특성에 따라 FIFO 순서를 보장하지 않는다. 마침내 모든 완료 루틴을 처리하여 큐가 비었을 때, 스레드는 자신을 알림 가능한 상태로 만든 함수로부터 반환한다.

알림 가능한 입출력은 두 가지 치명적인 단점을 지닌다: (1) 콜백 함수를 활용한다는 점에서 코드 구현에 상당한 제약이 있으며, (2) 입출력을 요청한 스레드가 또한 완료 루틴을 처리해야 한다. 추후 마이크로소프트는 해당 매커니즘이 갖는 문제를 "[입출력 완료 포트](#입출력-완료-포트)"란 대안을 소개하였다.

## 입출력 완료 포트
**[입출력 완료 포트](https://learn.microsoft.com/en-us/windows/win32/fileio/i-o-completion-ports)**(I/O completion ports)
