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
