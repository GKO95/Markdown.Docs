# 파일 시스템
**[파일 시스템](https://en.wikipedia.org/wiki/File_system)**(file system; FS)은 파일의 조직성과 접근성을 관리하는 체제이다; [파일명](https://en.wikipedia.org/wiki/Filename), [디렉토리](https://en.wikipedia.org/wiki/Directory_(computing)), [메타정보](https://en.wikipedia.org/wiki/Metadata), 접근 제어, 디스크 공간 및 파편화 등이 전부 파일 시스템에 의해 통제된다. 만일 규정된 파일 시스템이 없을 경우, [스토리지](Storage.md)마다 파일을 접근하는 방식에 호환성 문제가 발생하여 파일 손상이나 손실까지 초래될 수 있다.

아래는 다양한 파일 시스템 중 일부를 나열하며, 설계 목적에 따라 특성 및 장단점을 지닌다.

* [NTFS](#ntfs)
* [ReFS](#refs)
* [FAT](https://en.wikipedia.org/wiki/File_Allocation_Table) <sub>(FAT12, FAT16, FAT32)</sub>
* [exFAT](https://en.wikipedia.org/wiki/ExFAT)

## NTFS
**[NTFS](https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview)**(NT File System)는 [Windows OS](Windows.md)에서 채택한 주 파일 시스템이다.

### ReFS
**[ReFS](https://learn.microsoft.com/en-us/windows-server/storage/refs/refs-overview)**(Resilient File System)

# 파일 입출력
[입출력 동기화](https://learn.microsoft.com/en-us/windows/win32/fileio/synchronous-and-asynchronous-i-o)에는 두 가지 유형이 존재한다:

![동기 및 비동기 입출력](https://learn.microsoft.com/en-us/windows/win32/fileio/images/fig2bedit.png)

1. **동기 입출력**(synchronous I/O)

    입력을 요청한 [사용자 모드](Processor.md#사용자-모드) 스레드는 [커널 모드](Processor.md#커널-모드)에서 요청 처리를 완료할 때까지 대기 상태에 진입하여 기다린다.

1. **[비동기 입출력](https://en.wikipedia.org/wiki/Asynchronous_I/O)**(asynchoronous I/O)

    > [윈도우 OS](Windows.md)의 [Win32 API](WinAPI.md)에서는 이를 [`OVERLAPPED`](#overlapped-구조체) 구조체로부터 구현하여 *overlapped* 입출력이라고도 칭한다.

    입력을 요청한 [사용자 모드](Processor.md#사용자-모드) 스레드는 [커널 모드](Processor.md#커널-모드)에서 요청 처리를 진행하는 동안 다른 작업을 처리할 수 있다. 요청을 완료한 커널 모드 스레드는 *singaled* 되어 알리고, 사용자 모드 스레드는 해당 입출력 완료에 대한 추가적인 조치를 취한다. 단, 성능 향상에 도움이 된다면 비동기 입출력은 반드시 FIFO 방식에 따라 처리하지 않는다.

윈도우 OS는 비동기 입출력 작업의 완료를 알리는 네 가지 방법을 제공한다.

* [Device Kernel Object](Driver.md#디바이스-개체)
* [Event Kernel Object](Synchronization.md#이벤트-개체)
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

1. [대기 함수](Synchronization.md#대기-함수)가 기다리는 [커널 개체](Kernel.md#커널-개체)가 *signaled* 상태로 진입
1. 스레드의 APC 큐에 콜백 함수가 대기

APC 큐에 대기 중인 완료 루틴이 있다면 커널은 이를 대기열에서 꺼내 스레드로 전송한다. 그리고 스레드는 큐로부터 수신받은 콜백 함수를 실행한다. 이러한 과정을 반복하여 APC 큐의 나머지 콜백 함수를 처리하되, 비동기 입출력의 특성에 따라 FIFO 순서를 보장하지 않는다. 마침내 모든 완료 루틴을 처리하여 큐가 비었을 때, 스레드는 자신을 알림 가능한 상태로 만든 함수로부터 반환한다.

알림 가능한 입출력은 두 가지 치명적인 단점을 지닌다: (1) 콜백 함수를 활용한다는 점에서 코드 구현에 상당한 제약이 있으며, (2) 입출력을 요청한 스레드가 또한 완료 루틴을 처리해야 한다. 추후 마이크로소프트는 해당 매커니즘이 갖는 문제를 "[입출력 완료 포트](#입출력-완료-포트)"란 대안을 소개하였다.

## 입출력 완료 포트
**[입출력 완료 포트](https://learn.microsoft.com/en-us/windows/win32/fileio/i-o-completion-ports)**(I/O completion ports)는 [스레드 풀](Thread.md#스레드-풀)을 활용하여 다수의 비동기 입출력 요청을  처리할 수 있는 매커니즘이다. 미리 할당된 스레드 풀을 사용함으로써, 매번 입출력 요청을 처리할 때마다 스레드를 생성하는 방식보다 효율적인 [병행성](https://en.wikipedia.org/wiki/Concurrency_(computer_science))을 보장한다. 본래 장치 입출력을 위해 설계되었지만, 파일을 포함한 비동기 입출력을 지원하는 [커널 개체](Kernel.md#커널-개체)도 사용 가능하다.

> 일반적으로 스레드 풀의 스레드 개수를 [CPU](Processor.md#프로세서-코어)의 두 배로 설정하는 걸 권장한다.

커널 모드 스레드가 비동기 입출력 요청을 완료하면 입출력 완료 패킷(I/O completion packet)을 지정된 입출력 완료 포트로 전송한다. 여기서 입출력 완료 패킷에는 다음 정보들을 포함한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="text-align: center;">입출력 완료 패킷의 구성</capation><colgroup><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/></colgroup><thead><tr><th style="text-align: center;"><code>dwBytesTransferred</code></th><th style="text-align: center;"><code>dwCompletionKey</code></th><th style="text-align: center;"><code>pOverlapped</code></th><th style="text-align: center;"><code>dwError</code></th></tr></thead><tbody><tr style="text-align: center;"><td>전송된 <a href="https://en.wikipedia.org/wiki/Byte">바이트</a> 개수</td><td>Completion Key</td><td><a href="#overlapped-구조체">Overlapped</a> 포인터</td><td>오류 코드</td></tr></tbody></table>

[CreateIoCompletionPort](https://learn.microsoft.com/en-us/windows/win32/fileio/createiocompletionport) 함수로부터 생성된 입출력 완료 포트는 총 다섯 개의 제각각 역할을 지닌 자료 구조로 구성된다:

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">입출력 완료 포트의 구조</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">자료 구조</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: left;">Device List</td><td>입출력 완료 포트에 관여한 장치(혹은 파일)들의 <a href="Process.md#핸들">핸들</a> 리스트이다.</td></tr><tr><td style="text-align: left;">I/O Completion Queue</td><td>입출력 요청을 완료한 커널 모드 스레드가 전달한 입출력 완료 패킷을 FIFO 순서대로 담는 대기열이다.<ul><li>시스템은 우선 해당 장치(혹은 파일)가 포트 관여 여부를 검사한 다음 패킷을 큐에 대기시킨다.</li><li>한편, <a href="https://learn.microsoft.com/en-us/windows/win32/fileio/postqueuedcompletionstatus">PostQueuedCompletionStatus</a> 함수는 포트에 직접 입출력 완료 패킷을 큐에 대기시킨다.</li></ul></td></tr><tr><td style="text-align: left;">Waiting Thread Queue</td><td>I/O Completion Queue의 패킷을 처리할 준비가 된, 즉 <a href="https://learn.microsoft.com/en-us/windows/win32/api/ioapiset/nf-ioapiset-getqueuedcompletionstatus">GetQueuedCompletionStatus</a> 함수를 호출하여 대기 중인 스레드의 ID를 담는 LIFO 대기열이다. <ul><li>LIFO 순서는 가장 최근에 실행된 스레드를 활용하여 성능 및 메모리 효율을 높인다.</li></ul></td></tr><tr><td style="text-align: left;">Released Thread List</td><td>대기 상태에서부터 깨어난 스레드들의 ID를 담는 리스트이다. 이를 통해 입출력 완료 포트는 깨어난 스레드를 기억하고 실행 여부를 예의주시한다.</td></tr><tr><td style="text-align: left;">Paused Thread List</td><td>(<a href="Synchronization.md#대기-함수">대기 함수</a> 등에 의해) 대개 상태에 놓인 스레드들의 ID를 담는 리스트이다.</td></tr></tbody></table>

입출력 완료 포트가 생성되면 CreateIoCompletionPort 함수는 해당 포트의 핸들을 반환한다. 즉, 입출력 완료 포트는 이를 생성한 [프로세스](Process.md)에 한정되어 타 프로세스와 공유될 수 없다. 반면, 동일 프로세스의 스레드 간에 공유될 수 있어 스레드 간 통신(inter-thread communication)에도 활용된다.
