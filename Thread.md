# 스레드
**[스레드](https://en.wikipedia.org/wiki/Thread_(computing))**(thread)는 [프로세스](Process.md)에 로드된 프로그램 이미지의 코드를 실행하기 위해 [프로세서](Processor.md)에서 처리하는 작업 흐름의 단위이다. 프로세스는 두 개 이상의 스레드를 활용할 수 있으며, 동일한 [가상 주소 공간](Process.md#가상-주소-공간)에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 단, 스레드 추가 여부는 프로그램 개발 당시 설계에 따라 결정되고 배포된 이후로 임의 추가할 수 있는 게 아니다.

### 기본 스레드
프로세스가 생성되면 코드 실행을 위해 기본적으로 한 개의 스레드가 함께 생성되는데, 이를 **기본 스레드**(primary thread)라고 부른다. 기본 스레드는 프로그램을 본격적으로 시작하는 [진입 함수](C.md#진입점)를 실행하기 때문에, 정상적인 기본 스레드의 종료는 결과적으로 프로세스 종료를 초래한다.

## 스레드 컨텍스트
**스레드 컨텍스트**(thread context)

## 스레드 로컬 스토리지
**[스레드 로컬 스토리지](https://en.wikipedia.org/wiki/Thread-local_storage)**(thread local storage; TLS)

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
**[대기 함수](https://learn.microsoft.com/en-us/windows/win32/sync/wait-functions)**(wait functions)는 스레드 자신의 코드 실행을 막으며, 특정 기준을 충족할 때까지 반환하지 않는다. 그 동안 대기 함수를 호출한 스레드는 대기 상태에 진입하여 아무런 동작 없이 기다리게 된다. 만일 그 요건이 커널 개체일 경우, signaled 상태로 전환될 때 대기 함수가 반환한다. 단, 이미 요건이 충족된 상태에서 대기 함수를 호출하게 되면 스레드는 대기 상태로 전환되지 않는다. 아래는 대기 함수의 일부를 소개한다.

* [WaitForSingleObject](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject)
* [WaitForMultipleObjects](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitformultipleobjects)
* 기타 등등

성공적인 대기 부작용(successful wait side effect)

그 중에서 WaitForMultipleObjects가 유용한 점은 모든 작업이 [원자적](Processor.md#원자적-연산)이기 때문에 잠재적 [교착 상태](https://en.wikipedia.org/wiki/Deadlock_(computer_science))를 방지할 수 있다:

1. 스레드 A의 WaitForMultipleObjects 함수가 모든 개체가 signaled 되었는지 확인하고 반환하면서 개체들을 nonsignaled 상태로 리셋하려고 한다.
1. 하지만 스레드 B 또한 동일한 개체들을 확인하고 있다면, 스레드 B의 확인이 마칠 때까지 스레드 A는 상태를 nonsignaled로 리셋할 수 없다.

### 이벤트 개체
**[이벤트 개체](https://learn.microsoft.com/en-us/windows/win32/sync/event-objects)**(event object)는 [SetEvent](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setevent) 함수를 통해 명시적으로 signaled 상태로 설정할 수 있는 동기화 개체이다. 이벤트 개체는 다음과 같이 두 유형이 존재한다.

* Manul-reset event: [ResetEvent](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-resetevent) 함수를 명시적으로 호출하지 않는 이상 signaled 상태를 계속 유지한다.
* Auto-reset event: signaled 상태를 유지하다가 대기 중인 스레드가 재개될 때 시스템이 자동적으로 nonsignaled 상태로 복원시킨다. 대기 중인 스레드가 없을 때, 이벤트 개체는 signaled 상태를 유지한다. 한편, 대기 중인 스레드가 두 개 이상일 경우, 재개할 대기 스레드는 선택되는데 FIFO 순서가 아니다.

### 뮤텍스 개체
**[뮤텍스 개체](https://learn.microsoft.com/en-us/windows/win32/sync/mutex-objects)**(mutex object)는 아무런 스레드에 의해 소유되지 않을 시 signaled 상태가 되고, 소유되었을 때 nonsignaled 상태가 된다. 다시 말해, 뮤텍스는 스레드의 개체 소유 여부에 따라 상태가 결정된다. 오로지 한 개의 스레드씩만 뮤텍스 개체를 소유할 수 있다.

> 뮤텍스는 공유 리소스를 "mutually exclusive"하게 접근한다는 점에서 비롯된 용어이다.

단, 뮤텍스 개체를 소유할 스레드는 FIFO 순서로 소유하지 않는다. 만일 스레드가 뮤텍스 개체를 놓아주지 않은 채 종료하였다면, 뮤텍스는 유기되었다고 간주된다. 그러면 뮤텍스를 대기하던 스레드는 유기된 뮤텍스 개체 소유권을 획득하지만, 대기 함수는 WAIT_ABANDONED을 반환한다. 유기된 뮤텍스 개체는 오류가 발생하였으며 뮤텍스로 보호된 리소스는 알려지지 않은 상태에 있음을 의미한다.

### 세마포어 개체
**[세마포어 개체](https://learn.microsoft.com/en-us/windows/win32/sync/semaphore-objects)**(semaphore object)는 0부터 지정된 최대치까지 카운트를 유지하는 동기화 개체이다. 여기서 언급한 카운트는 세마포어 개체 획득이 가능한 큐 슬롯을 의미한다. 즉, 세마포어의 목적은 제한된 개수의 스레드만이 병행하여 실행될 수 있도록 하고, 나머지는 대기 함수에 의해 기다리도록 만든다. 그러므로 세마포어 개체가 0에 달하면 세마포어는 nonsignaled이 되어 대기 함수로부터 기다리게 만들고, 그 외에는 signaled 상태로 스레드의 대기를 마치고 실행하도록 한다.

* 스레드가 세마포어 개체에 의한 대기를 마쳤을 시, 카운트는 감소한다.
* 스레드가 [ReleaseSemaphore](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasesemaphore)를 호출하면 카운트를 증가한다.

세마포어의 카운트는 절대로 0보다 작을 수 없고 최대치보다 클 수 없다.

### 대기 가능한 타이머 개체
**[대기 가능한 타이머 개체](https://learn.microsoft.com/en-us/windows/win32/sync/waitable-timer-objects)**(waitable timer object)는 지정된 시간에 도달할 때 signaled 상태로 설정되는 동기화 개체이다. 대기 가능한 타이머 개체는 다음과 같이 두 유형이 존재하며, 둘 다 주기적으로 실행되는 타이머로 설정할 수 있다.

* Manual-reset timer: [SetWaitableTimer](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-setwaitabletimer) 함수를 명시적으로 호출하지 않는 이상 signaled 상태를 계속 유지한다.
* Synchronization timer: 대기 가능한 타이거 개체로부터 기다리고 있는 스레드가 대기를 완료(즉, 재개)할 때까지 signaled 상태를 계속 유지한다.
* Periodic timer: 특정 시간이 만료될 때마다 다시 시작되는 타이머이며, 이는 타이머가 리셋되거나 취소될 때까지 반복된다.

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
