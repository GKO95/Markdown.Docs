# 스레드
**[스레드](https://en.wikipedia.org/wiki/Thread_(computing))**(thread)는 [프로세스](Process.md)에 로드된 프로그램 이미지의 코드를 실행하기 위해 [프로세서](Processor.md)에서 처리하는 작업 흐름의 단위이다. 프로세스는 두 개 이상의 스레드를 활용할 수 있으며, 동일한 [가상 주소 공간](Process.md#가상-주소-공간)에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 단, 스레드 추가 여부는 프로그램 개발 당시 설계에 따라 결정되고 배포된 이후로 임의 추가할 수 있는 게 아니다.

### 기본 스레드
프로세스가 생성되면 코드 실행을 위해 기본적으로 한 개의 스레드가 함께 생성되는데, 이를 **기본 스레드**(primary thread)라고 부른다. 기본 스레드는 프로그램을 본격적으로 시작하는 [진입 함수](C.md#진입점)를 실행하기 때문에, 정상적인 기본 스레드의 종료는 결과적으로 프로세스 종료를 초래한다.

## 스레드 컨텍스트
**스레드 컨텍스트**(thread context)

## 비동기 프로시저 호출
**[비동기 프로시저저 호출](https://learn.microsoft.com/en-us/windows/win32/sync/asynchronous-procedure-calls)**(asynchronous procedure call; APC)은 특정 [스레드 컨텍스트](#스레드-컨텍스트)에서 [비동기식](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming))으로 실행되는 [함수](C.md#함수)이다. 각 [스레드](#스레드)마다 APC가 대기하면 시스템에서 [소프트웨어 인터럽트](Processor.md#인터럽트)를 일으키는 [큐](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))를 갖는다. 그리고 해당 스레드가 [스케줄링](Processor.md#스케줄링)될 때 대기 중인 APC 함수는 순서대로 실행된다.

* [시스템](Kernel.md) 및 [어플리케이션](Process.md)에 의해 생성된 APC를 각각 [*커널 모드 APC*](Processor.md#커널-모드) 그리고 [*사용자 모드 APC*](Processor.md#사용자-모드)라고 부른다.

## 스레드 로컬 스토리지
**[스레드 로컬 스토리지](https://en.wikipedia.org/wiki/Thread-local_storage)**(thread local storage; TLS)

# 스레드 풀
> 본 장은 [Windows Vista](Windows.md)부터 소개된 최신 [스레드 풀 API](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pool-api) 위주로 소개하며, 이전 윈도우 OS의 [옛 스레드 풀 API](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pooling) 내용과 혼돈하지 않도록 유의한다.

**[스레드 풀](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pools)**(thread pool)은 어플리케이션을 대신하여 [비동기](FileSystem.md#파일-입출력) [콜백](C.md#콜백-함수)을 효율적으로 실행할 스레드들의 집합체이다. 각 [프로세스](Process.md)마다 기본 스레드 풀이 자동적으로 생성되며, 풀의 스레드는 흔히 [CreateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread) 함수로 생성되는 [스레드](#스레드)와 전혀 다른 존재로써 "작업자 스레드"라고 부른다.

아래는 스레드 풀에 대한 설명을 이어가기 전에 숙지해야 할 용어 및 설명을 간략히 소개한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">스레드 풀 관련 용어</caption><colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">용어</th><th style="text-align: center;">원문</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td>작업 항목</td><td>work item</td><td>처리되어야 할 작업 대상이다: <a href="#ThreadpoolWork"><code>TP_WORK</code></a>, <a href="#ThreadpoolTimer"><code>TP_TIMER</code></a>, <a href="#ThreadpoolWait"><code>TP_WAIT</code></a>, <a href="#ThreadpoolIo"><code>TP_IO</code></a></td></tr><tr><td>작업 대기열</td><td>work queue</td><td>처리되어야 할 작업 항목들이 대기하는 공간이다.</td></tr><tr><td>작업자 스레드</td><td>worker thread</td><td>작업 대기열에서 기다리는 작업 항목을 처리하는 스레드 풀의 스레드이다.</td></tr><tr><td>작업자 팩토리</td><td>worker factory</td><td>내부 알고리즘에 따라 작업자 스레드의 생성 및 파괴 등을 결정하고 관리한다.</td></tr></tbody></table>

<sup>_† 작업 항목에는 <sup>(1)</sup> 요청을 완료하기 필요한 데이터 ([파일 시간](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime), [커널 개체](Kernel.md#커널-개체), [입출력 완료 포트](FileSystem.md#입출력-완료-포트)의 [핸들](Process.md#핸들)) 및 <sup>(2)</sup> 작업자 스레드가 실행할 콜백 함수가 포함된다._</sup>

요청을 완료한 작업자 스레드는 곧바로 파괴되지 않고 다음에 처리할 대기열의 작업 항목을 기다리며 재활용된다. 허나, 작업자 팩토리는 성능 효율을 위해 스레드 재활용이 아닌 파괴하고 새로 생성하는 결정을 내릴 수 있다. 작업자 스레드의 생성과 파괴는 임의로 이루어질 수 없음을 의미한다. 기본적으로 스레드 풀은 작업자 스레드의 최소 및 최대 개수를 1-500으로 제한하며, [사용자 지정 스레드 풀](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpool)을 생성하면 환경 설정될 수 있다.

스레드 풀은 아래와 같이 장단점을 가지고 있어, 사용 여부는 신중히 선택해야 한다.

1. 성능적으로 부담되는 스레드의 생성 및 파괴 행위를 줄이고, 운영체제의 자체적인 관리 및 운영으로 활용하기 매우 편리하다.
1. 스레드 풀을 사용하게 될 시, 프로세스가 종료될 때까지 커널 리소스가 메모리를 점유한 채 잔여할 수 있다.

## ThreadpoolWork
**TP_WORK** 구조체의 작업 개체(work object)를 다루는 스레드 풀 API들을 소개한다. TP_WORK은 실행하려는 [WorkCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms687396(v=vs.85)) 프로토타입의 콜백 함수를 작업 대기열에 전달하여 작업자 스레드에게 처리하도록 요청한다.

* WinAPI 목록: *[CreateThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolwork)* <sub>(생성)</sub>, *[SubmitThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-submitthreadpoolwork)* <sub>(요청)</sub>, *[WaitForThreadpoolWorkCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpoolworkcallbacks)* <sub>(대기)</sub>, *[CloseThreadpoolWork](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolwork)* <sub>(해제)</sub>

스레드 풀 작업 중 ThreadpoolWork는 가장 활용도가 높기 때문에 이를 간단히 구현할 수 있는 [Win32 API](WinAPI.md)를 제공한다:

1. 실행하려는 콜백 함수를 [SimpleCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms686295(v=vs.85)) 프로토타입을 기반으로 작성한다.
1. [TrySubmitThreadpoolCallback](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-trysubmitthreadpoolcallback) 함수로 작성된 콜백 함수를 스레드 풀의 작업 대기열로 전송한다.

## ThreadpoolTimer
**TP_TIMER** 구조체의 타이머 개체(timer object)를 다루는 스레드 풀 API들을 소개한다. TP_TIMER는 실행하려는 [TimerCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms686790(v=vs.85)) 프로토타입의 콜백 함수를 타이머가 만료될 때 작업자 스레드에게 처리하도록 요청한다. 기존에 사용하였던 TP_TIMER에 새로운 타이머 설정으로 요청하여 재활용이 가능하다.

* WinAPI 목록: *[CreateThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpooltimer)* <sub>(생성)</sub>, *[SetThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-setthreadpooltimer)* <sub>(요청)</sub>, *[IsThreadpoolTimerSet](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-isthreadpooltimerset)* <sub>(점검)</sub>, *[WaitForThreadpoolTimerCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpooltimercallbacks)* <sub>(대기)</sub>, *[CloseThreadpoolTimer](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpooltimer)* <sub>(해제)</sub>

## ThreadpoolWait
**TP_WAIT** 구조체의 대기 개체(wait object)를 다루는 스레드 풀 API들을 소개한다. TP_WAIT은 실행하려는 [WaitCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms687017(v=vs.85)) 프로토타입의 콜백 함수를 [커널 개체](Kernel.md#커널-개체)가 *signaled* 되었을 때 작업자 스레드에게 처리하도록 요청한다. 기존에 사용하였던 TP_WAIT에 새로운 커널 개체의 핸들과 함께 요청하여 재활용이 가능하다.

* WinAPI 목록: *[CreateThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolwait)* <sub>(생성)</sub>, *[SetThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-setthreadpoolwait)* <sub>(요청)</sub>, *[WaitForThreadpoolWaitCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpoolwaitcallbacks)* <sub>(대기)</sub>, *[CloseThreadpoolWait](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolwait)* <sub>(해제)</sub>

## ThreadpoolIo
**TP_IO** 구조체의 입출력 완료 개체(I/O completion object)를 다루는 스레드 풀 API들을 소개한다. TP_IO는 실행하려는 [IoCompletionCallback](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/ms684124(v=vs.85)) 프로토타입의 콜백 함수를 [입출력 완료 포트](FileSystem.md#입출력-완료-포트)에 관여된 파일 핸들의 입출력 요청이 완료될 때 입출력 완료 패킷을 작업자 스레드에게 처리하도록 요청한다.

* WinAPI 목록: *[CreateThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-createthreadpoolio)* <sub>(생성)</sub>, *[StartThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-startthreadpoolio)* <sub>(요청)</sub>, *[CancelThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-cancelthreadpoolio)* <sub>(취소)</sub>, *[WaitForThreadpoolIoCallbacks](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-waitforthreadpooliocallbacks)* <sub>(대기)</sub>, *[CloseThreadpoolIo](https://learn.microsoft.com/en-us/windows/win32/api/threadpoolapiset/nf-threadpoolapiset-closethreadpoolio)* <sub>(해제)</sub>

# 스레드 동기화
**[스레드 동기화](https://en.wikipedia.org/wiki/Synchronization_(computer_science))**(thread synchronization)는 두 개 이상의 [스레드](#스레드)가 동시에 [공유 리소스](https://en.wikipedia.org/wiki/Shared_resource)를 접근하지 못하도록 보장하는 매커니즘을 일컫는다. 대체로 한 스레드가 리소스를 접근하는 동안 나머지는 대기하는 방식으로 구현된다. 만일 스레드 간 동기화가 적절히 조치되지 않았다면 개별 스레드가 정상적으로 코드를 실행하여도 의도치 않은 결과를 초래할 수 있다.

아래는 스레드 동기화의 필요성을 설명하기 위해, A와 B란 두 개의 스레드는 다음 코드를 수행한다고 가정한다.

```nasm
ADD [counter], 1
```

두 개의 스레드가 동시에 실행된다면 최종적으로 `counter`에 저장된 값은 2만큼 증가하였을 것이라 예상할 수 있다. 하지만 두 스레드가 아래 순서대로 병행 실행될 시, 오히려 1만큼 증가하는 결과값을 가지게 된다:

1. 스레드 A가 `counter`의 값을 읽는다.
1. 스레드 B가 `counter`의 값을 읽는다.
1. 스레드 A가 읽은 값에 1을 더하여 `counter`에 반환한다.
1. 스레드 B가 읽은 값에 1을 더하여 `counter`에 반환한다.

다음은 [윈도우 OS](Windows.md)가 스레드 동기화를 구현할 수 있도록 제공하는 함수, 개체 등의 매커니즘을 나열한다:

* **[사용자 모드](Processor.md#사용자-모드)**

    *다른 [가상 주소 공간](Process.md#가상-주소-공간)을 가진 타 [프로세스](Process.md)와 함께 활용될 수 없는 등의 제약이 있으나, 성능적으로 매우 빠른 장점이 있다. 아래 "atomic" 태그는 [원자적](Processor.md#원자적-연산)으로 동기화하는 걸 의미한다.*

    * [InterLocked Functions](#인터락-함수) (*atomic*)
    * [Spinlocks](#스핀락)
    * [Critical Section Objects](#임계-구역) (*atomic*)
    * [Slim Reader/Writer (SRW) Locks](#슬림-읽기쓰기-락)
    * [Condition Variables](#조건-변수) (*atomic*)

* **[커널 모드](Processor.md#커널-모드)**

    *윈도우 OS가 제공하는 [동기화 커널 개체](Kernel.md#커널-개체)의 신호 상태에 따라 [대기 함수](#대기-함수)가 스레드 대기 여부를 결정한다. [시스템 서비스](WinAPI.md#시스템-서비스)에 의한 성능 [오버헤드](https://en.wikipedia.org/wiki/Overhead_(computing))가 불가피하지만 모든 상황에서 활용이 가능하다.*
    
    * [Event Objects](#이벤트-개체)
    * [Waitable Timer Objects](#대기-가능한-타이머-개체)
    * [Semaphore Objects](#세마포어-개체)
    * [Mutex Objects](#뮤텍스-개체)

윈도우 OS는 코드 성능을 향상시키기 위해 절대 [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)) 순서로 동기화하지 않는다; 먼저 기다렸다고 그 다음 순서로 접근할 수 있다는 걸 보장하지 않는다. 하지만 [마이크로소프트](https://aka.ms/microsoft)는 동기화 알고리즘을 공개하지 않는다. 차후 알고리즘이 언제든지 변경될 수 있으며, 알고리즘에 너무 의존하는 코드 개발을 방지한다.

## 인터락 함수
**[인터락 함수](https://learn.microsoft.com/en-us/windows/win32/sync/interlocked-variable-access)**(interlocked functions)는 [Win32 API](WinAPI.md) 중에서 간단한 연산을 [원자적](Processor.md#원자적-연산)으로 실행하는 함수들을 일컫는다. 만일 [x86](https://en.wikipedia.org/wiki/X86) 아키텍처일 경우, 이들은 대부분 [LOCK](https://www.felixcloutier.com/x86/lock) 접두사와 함께 실행될 수 있는 명령어로 구성되며, 아래는 일부 인터락 함수들을 소개한다.

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">인터락 함수의 x86 명령어 조합 및 설명</caption><colgroup><col style="width: 20%;"/><col style="width: 15%;"/><col style="width: 65%;"/></colgroup><thead><tr><th style="text-align: center;">인터락 함수</th><th style="text-align: center;">명령어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedexchange">InterLockedExchange</a></td><td><a href="https://www.felixcloutier.com/x86/xchg"><code>XCHG</code></a></td><td>32비트 변수를 지정한 값으로 변경한다.</td></tr><tr><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedexchangeadd">InterLockedCompareExchange</a></td><td><code>LOCK</code>+<a href="https://www.felixcloutier.com/x86/cmpxchg"><code>CMPXCHG</code></a></td><td>두 32비트 값을 비교한 결과에 따라 다른 32비트 값으로 변경 여부를 결정한다.</td></tr><tr><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedexchangeadd">InterLockedExchangeAdd</a></td><td><code>LOCK</code>+<a href="https://www.felixcloutier.com/x86/add"><code>ADD</code></a></td><td>두 32비트 값의 덧셈을 연산한다.</td></tr><tr><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedexchange">InterLockedIncrement</a></td><td><code>LOCK</code>+<a href="https://www.felixcloutier.com/x86/inc"><code>INC</code></a></td><td>32비트 변수의 값을 1만큼 증가시킨다.</td></tr><tr><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedxor">InterLockedXor</a></td><td><code>LOCK</code>+<a href="https://www.felixcloutier.com/x86/xor"><code>XOR</code></a></td><td>두 32비트 값의 <a href="https://en.wikipedia.org/wiki/Exclusive_or">베타적 논리합</a>을 연산한다.</td></tr></tbody></table>

<sup>_† 참고 문헌: [How does InterlockedIncrement work internally? - The Old New Thing](https://devblogs.microsoft.com/oldnewthing/20130913-00/?p=3243)_</sup>

## 스핀락
**[스핀락](https://en.wikipedia.org/wiki/Spinlock)**(spinlocks)은 스레드가 [락](https://en.wikipedia.org/wiki/Lock_(computer_science))을 소유할 때까지 반복적으로 획득 가능 여부를 확인하는 [루프](C.md#while-반복문), 즉 "스핀"을 활용하는 동기화 매커니즘이다. 락을 소유한 스레드는 루프로부터 탈출하여 공유 리소스에 접근하게 된다. [원자적](Processor.md#원자적-연산)이지 않고 [프로세서 시간](Processor.md#프로세서-시간)을 허비하기 때문에 [단일 코어](Processor.md#프로세서-코어) 시스템에서는 가급적 기피되어야 한다. 반면 다중 코어 시스템의 경우, 한 CPU 코어가 스핀을 돌더라도 나머지 여유 CPU 코어가 다른 작업을 수행하여 오히려 유용할 수 있다.

아래는 스핀락을 원리를 간단히 설명하기 위한 이론적 예시 코드이다.

```c
// Constantly checking for spinlock availability.
while (InterLockedExchange(&g_fResourceInUse, TRUE) == TRUE) {
    Sleep(0);
}

// Access to resource.
...

// End of access to resource; release spinlock.
InterLockedExchange(&g_fResourceInUse, FALSE);
```

비록 [InterLockedExchage](https://learn.microsoft.com/en-us/windows/win32/api/winnt/nf-winnt-interlockedexchange)란 [인터락 함수](#인터락-함수)가 동원되었지만, 이는 스핀락 자체가 원자적이라는 걸 의미하지 않는다.

## 임계 구역
**[임계 구역](https://learn.microsoft.com/en-us/windows/win32/sync/critical-section-objects)**(critical sections)은 CRITICAL_SECTION [구조체](C.md#구조체) 인스턴스의 획득 여부를 기준으로 일부 코드 영역을 오로지 한 스레드씩만 [원자적](Processor.md#원자적-연산)으로 진입할 수 있도록 허용한다. 만일 단순한 연산에 동기화가 필요할 경우, 임계 구역을 지정하기 보다 [인터락 함수](#인터락-함수)를 활용하는 걸 권장한다.

다음은 임계 구역 개체와 관련된 [Win32 API](WinAPI.md)를 일부 소개한다.

* [InitializeCriticalSection](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-initializecriticalsection): 임계 구역 개체를 초기화한다.
* [EnterCriticalSection](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-entercriticalsection): 임계 구역 개체의 소유권을 대기 및 확보한다.
* [LeaveCriticalSection](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-leavecriticalsection): 임계 구역 개체의 소유권을 포기한다.
* 기타 등등

## 슬림 읽기/쓰기 락
**[슬림 읽기/쓰기 락](https://learn.microsoft.com/en-us/windows/win32/sync/slim-reader-writer--srw--locks)**(slim reader/writer lock), 간단히 **SRW 락**은 공유 리소스의 접근 사유를 *읽기* 및 *쓰기*로 구분한다. SRWLOCK [구조체](C.md#구조체)를 활용한 [원자적](Processor.md#원자적-연산)이지 않은 동기화 매커니즘이다. [임계 구역](#임계-구역)보다 가볍고 빠르다는 장점이 있지만, 구조가 매우 간단(즉, "슬림")하여 제약 사항이 존재한다.<sup>[[참고](https://devblogs.microsoft.com/oldnewthing/20220304-00/?p=106309)]</sup>

스레드가 리소스를 접근하려 할 때, 요청에 따라 SRW 락은 두 가지 모드를 제공한다:

* **공유 모드**(shared mode)

    *다수의 스레드에 읽기 전용 접근을 부여하여, 공유 리소스의 읽기 작업을 [병행](https://en.wikipedia.org/wiki/Concurrency_(computer_science))으로 수행할 수 있도록 한다. 만일 읽기 작업이 쓰기 작업을 초과한다면, 이러한 병행성은 임계 구역에 비해 성능과 처리량이 향상한다.*
    
    * ([Try](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-tryacquiresrwlockshared))[AcquireSRWLockShared](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-acquiresrwlockshared)
    * [ReleaseSRWLockShared](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasesrwlockshared)

* **전용 모드**(exclusive mode)

    *단 한 개의 스레드씩만 읽기 및 쓰기 접근을 부여하여, 해당 모드로 공유 리소스를 처리하는 동안 아무런 스레드도 이를 접근할 수 없다.*
    
    * ([Try](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-tryacquiresrwlockexclusive))[AcquireSRWLockExclusive](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-acquiresrwlockexclusive)
    * [ReleaseSRWLockExclusive](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasesrwlockexclusive)

매우 간단한 구조를 가지고 있어 SRW 락의 상태 정보가 빈약하다. 즉, 이미 소유하고 있는 SRW 락의 모드를 공유에서 전용 (또는 그 반대)로 전환이 불가하다. 그리고 반복적인 SRW 락 획득은 [교착 상태](https://en.wikipedia.org/wiki/Deadlock_(computer_science))를 유발할 수 있기 때문에 주의해야 한다.

## 조건 변수
**[조건 변수](https://learn.microsoft.com/en-us/windows/win32/sync/condition-variables)**(conditional variables)는 CONDITION_VARIABLE [구조체](C.md#구조체)를 활용한 [원자적](Processor.md#원자적-연산)인 동기화 매커니즘이며, 특정 조건을 충족할 때까지 스레드를 대기시킨다. 아래 함수를 호출하여 스레드가 소유한 [임계 구역](#임계-구역) (혹은 [SRW 락](#슬림-읽기쓰기-락))을 잠시 내려놓게 하고 지정된 타이머가 만료하거나 재개 함수가 호출될 때까지 대기시킨다.

* [SleepConditionVariableCS](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-sleepconditionvariablecs)
* [SleepConditionVariableSRW](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-sleepconditionvariablesrw)

재개한 스레드는 임계 구역 (혹은 SRW 락)의 소유권을 다시 획득하고 공유 리소스를 접근 및 작업을 수행한다.

## 대기 함수
**[대기 함수](https://learn.microsoft.com/en-us/windows/win32/sync/wait-functions)**(wait functions)는 [커널 개체](Kernel.md#커널-개체)가 signaled 될 때까지 함수를 호출한 스레드를 대기 상태로 전환시켜 실행을 막는다. 커널 개체가 signaled 되면 대기 함수는 반환하는데, 이로부터 기다리다 재개한 스레드는 "대기를 완료하였다"고 언급한다. 단, 대기 함수를 호출하였을 당시 이미 signaled 되었다면 스레드는 대기 상태로 전환되지 않는다. 아래는 대표적인 두 가지 대기 함수를 소개한다.

* [WaitForSingleObject](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject)
* [WaitForMultipleObjects](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitformultipleobjects)

특히 WaitForMultipleObjects 함수는 [원자성](Processor.md#원자적-연산)이 접목되어 "성공적인 대기 부작용(successful wait side-effect)"에 의해 발생할 수 있는 [교착 상태](https://en.wikipedia.org/wiki/Deadlock_(computer_science))를 방지할 수 있다.

> 예를 들어, 스레드 A의 WaitForMultipleObjects 함수가 모든 개체의 신호 상태가 signaled인 걸 확인하였다. 대기 함수는 종료하면서 개체들을 nonsignaled 상태로 초기화를 시도하지만, 스레드 B 또한 동일한 개체들의 신호 여부를 확인하는 중이다. 스레드 A는 결국 스레드 B의 검사를 마칠 때까지 기다린다.

대기 함수에는 아무런 커널 개체를 활용할 수 있지만, 다음은 윈도우 OS가 제공하는 스레드 동기화에 특화된 커널 개체들을 소개한다.

### 이벤트 개체
**[이벤트 개체](https://learn.microsoft.com/en-us/windows/win32/sync/event-objects)**(event objects)는 [SetEvent](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setevent) 함수를 호출하여 signaled 상태로 설정하는 동기화 커널 개체이다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">이벤트 개체 유형</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">이벤트 개체</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td>Manul-reset event</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-resetevent">ResetEvent</a> 함수를 명시적으로 호출하지 않는 이상 signaled 상태를 계속 유지한다.</td></tr><tr><td>Auto-reset event</td><td>Signaled 상태를 유지하다 스레드가 대기를 완료할 시 자동적으로 nonsignaled 상태로 복원한다.<ul><li>대기 중인 스레드가 없을 경우, 이벤트 개체는 signaled 상태를 유지한다.</li><li>대기 중인 스레드가 두 개 이상일 경우, 대기를 완료할 스레드는 선택되지만 FIFO 순서는 아니다.</li></ul></td></tr></tbody></table>

### 대기 가능한 타이머 개체
**[대기 가능한 타이머 개체](https://learn.microsoft.com/en-us/windows/win32/sync/waitable-timer-objects)**(waitable timer objects)는 지정된 시간에 도달할 때 signaled 상태로 설정되는 동기화 커널 개체이다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">대기 가능한 타이머 개체 유형</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">타이머 개체</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td>Manul-reset timer</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setwaitabletimer">SetWaitableTimer</a> 함수를 명시적으로 호출하지 않는 이상 signaled 상태를 계속 유지한다.</td></tr><tr><td>Synchronization timer</td><td>타이머가 지정된 시간에 도달하여도 스레드가 대기를 완료할 때까지 signaled 상태를 유지한다.</td></tr><tr><td>Periodic timer</td><td>특정 시간이 만료될 때마다 타이머가 다시 시작되는데, 이는 초기화 또는 취소할 때까지 반복된다.<ul><li>Periodic timer는 위의 두 타이머 유형과 함께 접목될 수 있다.</li></ul></td></tr></tbody></table>

### 세마포어 개체
**[세마포어 개체](https://learn.microsoft.com/en-us/windows/win32/sync/semaphore-objects)**(semaphore objects)는 0부터 지정한 최대치 이내에 (빈 큐 슬롯 개수를 의미하는) 카운트를 유지하는 동기화 커널 개체이다. 다시 말해, 세마포어 목적은 제한된 개수의 스레드만 병행으로 실행될 수 있게 허용하고, 나머지는 대기 함수에 의해 기다리도록 한다. 카운트는 절대로 0보다 작을 수 없고 최대치를 초과할 수 없다.

1. 카운트 감소 조건: 스레드가 대기 함수로부터 대기를 완료하였을, 즉 코드 실행을 재개할 때이다.
1. 카운트 증가 조건: 스레드가 [ReleaseSemaphore](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasesemaphore)를 호출할 때이다.

카운트가 0에 도달하면 세마포어는 nonsignaled 되어 스레드를 대기 함수로부터 기다리게 한다. 반면, 카운트가 0이 아니면 세마포어는 signaled 되어 스레드는 대기 함수를 기다리지 않거나 대기를 완료하여 코드 실행을 재개한다.

### 뮤텍스 개체
**[뮤텍스 개체](https://learn.microsoft.com/en-us/windows/win32/sync/mutex-objects)**(mutex objects)는 스레드로부터 소유되었는지 여부에 따라 신호 상태가 결정되는 동기화 커널 개체이다; 뮤텍스 커널의 소유권을 획득한 스레드가 있을 시 nonsignaled, 그렇지 않을 시에는 signaled 상태가 된다. 단, 뮤텍스 개체의 소유권은 FIFO 순서로 획득되지 않는다.

스레드가 소유권을 놓아주지 않은 채 종료하였다면, 뮤텍스 개체는 유기(abandoned)되었다고 부른다. 유기된 뮤텍스는 대기 중인 다른 스레드가 소유권을 획득하지만 WAIT_ABANDONED이 대기 함수로부터 반환된다. 특히 유기된 뮤텍스는 이전 스레드에서 오류가 발생하였을 가능성을 시사하며, 뮤텍스로부터 보호된 리소스 상태는 불명확하기 때문에 유의해서 처리되어야 한다.

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
