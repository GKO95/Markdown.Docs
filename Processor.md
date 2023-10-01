# 프로세서
[프로세서](https://ko.wikipedia.org/wiki/중앙_처리_장치)(processor), 또는 CPU로 알려진 하드웨어는 프로그램의 [기계어](https://ko.wikipedia.org/wiki/기계어)를 처리하는 [전자회로](https://ko.wikipedia.org/wiki/전자_회로)이다. 프로그램을 실행하기 위해 필요한 시스템의 다양한 작업들은 [명령어 집합](https://ko.wikipedia.org/wiki/명령어_집합)에 의해 정의되어 있으며, 대표적으로 [x86](https://ko.wikipedia.org/wiki/X86)과 [ARM](https://ko.wikipedia.org/wiki/ARM_아키텍처) 계열 명령어 집합 아키텍처가 존재한다. 프로세서에서 처리하는 명령어 집합이 무엇인지에 따라 [운영체제](https://ko.wikipedia.org/wiki/운영체제)의 아키텍처가 함께 결정된다.

### 명령어
프로세서가 처리하게 될 [명령어](https://en.wikipedia.org/wiki/Instruction_set_architecture#Instructions)는 일반적으로 두 가지 필드로 구성되어 있다.

![MIPS32 Add Immediate 명령의 구조](https://upload.wikimedia.org/wikipedia/commons/2/2a/Mips32_addi.svg)

1. [명령 코드](https://ko.wikipedia.org/wiki/명령_코드)(일명 opcode)는 덧셈, 복사, 이동 등의 프로세서가 수행할 연산 작업을 명시한다.
1. [피연산자](https://ko.wikipedia.org/wiki/피연산자#컴퓨터_과학)는 opcode에 따라 아예 없거나 한 개 이상이 [레지스터](Assembly.md#레지스터), [리터럴](C.md#구문) 및 [상수](C.md#변수), 또는 [메모리 주소](C.md#포인터)로서 활용된다.

## 프로세서 구조
다음은 단일 코어 프로세서의 구조를 다이어그램으로 보여주며, 황색 바탕의 "Processor"는 실질적인 연산을 담당하는 물리적인 [프로세서 코어](#프로세서-코어)를 의미한다. 그리고 흑색과 적색 화살표는 각각 데이터와 제어 흐름 방향을 가리킨다.

![단일 프로세서 중앙 처리 장치](https://upload.wikimedia.org/wikipedia/commons/3/3a/ABasicComputer.svg)

* [제어 장치](#제어-장치)
* [산술 논리 장치](#산술-논리-장치)
* [레지스터](#레지스터)

### 제어 장치
[제어 장치](https://en.wikipedia.org/wiki/Control_unit)(Control Unit; CU)는 프로세서의 연산 작업을 지휘하는 CPU의 구성요소이다. [명령어](#명령어)로부터 식별된 opcode는 [이진 디코더](https://ko.wikipedia.org/wiki/복호화#복호기_(디코더,_decoder))에 의해 해당 연산 작업을 수행하는데 필요한 타이밍 및 제어 신호로 변환된다. 결과적으로 CPU와 타 장치들 간 데이터 흐름을 지휘하기 위해 대부분의 컴퓨터 리소스를 관리한다.

### 산술 논리 장치
[산술 논리 장치](https://ko.wikipedia.org/wiki/산술_논리_장치)(Arithmetic Logic Unit; ALU)는 CPU의 구성요소 중 이진 정수의 [산술](https://ko.wikipedia.org/wiki/산술) 및 [비트 연산](https://ko.wikipedia.org/wiki/비트_연산)을 담당하면서도 실질적인 컴퓨터 연산처리에서 가장 핵심되는 장치이다.

> [부동소수점](https://ko.wikipedia.org/wiki/부동소수점) 연산을 담당하는 [FPU](https://ko.wikipedia.org/wiki/부동소수점_장치)가 있으나, 주로 수학 계산을 위해 사용되며 초창기 CPU에는 내장되지 않은 장치이다.

![프로세서 ALU의 입출력 데이터를 표현한 다이어그램](https://upload.wikimedia.org/wikipedia/commons/0/0f/ALU_block.gif)

ALU는 기본적으로 opcode와 피연산자를 입력받고, 해당 opcode 작업의 결과물을 출력한다. 만일 [상태 레지스터](https://ko.wikipedia.org/wiki/상태_레지스터)로부터 입력을 받으면 [캐리](https://en.wikipedia.org/wiki/Carry_flag), [오버플로우](https://en.wikipedia.org/wiki/Overflow_flag) 여부 등의 상태가 이전 작업에서 발생하였는지 확인할 수 있으며, 현재 작업에서 발생한 상태 정보는 다시 상태 레지스터로 출력된다.

### 레지스터
[레지스터](https://ko.wikipedia.org/wiki/프로세서_레지스터)(register)는 CPU에 내장되어 프로세서가 가장 빨리 접근할 수 있는 (8비트, 16비트, 32비트 등) [비트](https://ko.wikipedia.org/wiki/비트_(단위)) 단위의 소규모 임시 [저장공간](Disk.md)이다. 아래 그림은 [x86-64](https://ko.wikipedia.org/wiki/X86-64) 아키텍처의 프로세서에 탑재된 레지스터들이다.

![x86-64 아키텍처의 프로세서 레지스터](https://upload.wikimedia.org/wikipedia/commons/1/15/Table_of_x86_Registers_svg.svg)

하드웨어 기능을 제어하는 [제어 레지스터](https://en.wikipedia.org/wiki/Control_register)(CR)나 작업 상태를 나타내는 [FLAGS 레지스터](https://en.wikipedia.org/wiki/FLAGS_register)(RFLASG)와 같이 일부는 특수한 목적을 지닌다. 한편, 다양한 용도로 활용될 수 있는 [범용 레지스터](Assembly.md#범용-레지스터)는 [어셈블리](Assembly.md) 언어를 참고하도록 한다.

## 프로세서 규격
아래 [작업 관리자](https://ko.wikipedia.org/wiki/작업_관리자_(윈도우))에 나타난 CPU 성능 정보를 예시로 시스템에 영향을 미치는 프로세서의 규격에 대해 설명한다.

![작업 관리자에서 표시된 CPU 성능 정보 예시](./images/processor_taskmgr.png)

* [프로세서 클럭](#프로세서-클럭) (GHz)
* [프로세서 코어](#프로세서-코어)
* [프로세서 점유](#프로세서-점유) (%)

### 프로세서 클럭
[클럭 속도](https://ko.wikipedia.org/wiki/클럭_속도)(clock rate 또는 clock speed), 일명 클럭 주파수(clock frequency)는 [메인보드](https://ko.wikipedia.org/wiki/메인보드)에 위치한 [클럭 발생기](https://en.wikipedia.org/wiki/Clock_generator)로부터 프로세서가 작업을 수행하는데 기준이 되는 [클럭 신호](https://ko.wikipedia.org/wiki/클럭_신호)가 얼마나 빈번히 생성될 수 있는지 나타내는 [헤르츠](https://ko.wikipedia.org/wiki/헤르츠)(Hz) 단위의 수치이다. 프로세서의 연산은 클럭 신호에 동기화되어, 주파수가 높을수록 동일한 시간동안 더 많은 작업을 수행할 수 있다. 그러나 클럭 속도는 프로세서가 초당 작업한 개수를 의미하는 게 절대 아니다.

> 다시 말해, 위의 작업 관리자에서 클럭 속도가 4.24 GHz로 측정되었지만 프로세서가 초당 42.4억 개의 작업을 수행한 걸 의미하는 게 아니다. 이는 한 [명령어](#명령어)을 수행하는 데 opcode에 따라 필요한 클럭 주기가 다양하기 때문이다.

* **[동적 주파수 스케일링](https://ko.wikipedia.org/wiki/동적_주파수_스케일링)**(dynamic frequency scaling)

    일명 CPU 스로틀링(CPU throttling)은 전력 관리 기법으로, 전원 절약 및 CPU 발열 등의 필요에 따라 실시간으로 클럭 주파수를 자동 조절한다. 발열이 심할 경우에는 MHz 단위까지 내려가 마우스 및 키보드 응답 지연이 가시적으로 나타나기도 한다.

### 프로세서 코어
프로세서 코어(processor core)는 CPU에서 연산을 담당하는 물리적인 [전자회로](https://ko.wikipedia.org/wiki/집적_회로)를 가리키며, 하나의 칩에 두 개 이상의 프로세서 코어를 가진 CPU를 [멀티 코어](https://ko.wikipedia.org/wiki/멀티_코어)라고 지칭한다. 프로세서 성능을 향상시키는데 [클럭 속도](#프로세서-클럭)의 기술적 제약을 극복하기 위해 하드웨어 제조사는 2005년에 처음으로 시장에 멀티 코어 CPU를 소개하였다. 멀티 코어 CPU는 여러 [스레드](Process.md#스레드)를 동시에 처리할 수 있는 장점을 지닌다.

> 위의 작업 관리자는 시스템의 프로세서가 헥사코어, 즉 여섯 개의 코어를 가진 [AMD](https://www.amd.com) [Ryzen 5 5600X](https://www.amd.com/en/products/cpu/amd-ryzen-5-5600x)라고 표시하였다. 여기서 12개의 그래프 항목은 SMT에 의한 논리 프로세서 정보를 보여주는 것으로 아래 내용을 참고한다.

* **[동시 멀티스레딩](https://ko.wikipedia.org/wiki/동시_멀티스레딩)**(simultaneous multithreading; SMT)

    하드웨어적 [멀티스레딩](https://ko.wikipedia.org/wiki/멀티스레딩)을 지원하는 경우, 프로세서 코어는 매 클럭 주기마다 다수의 스레드의 명령어를 처리할 수 있다. 하나의 물리 코어가 마치 두 개 이상의 프로세서 역할을 처리하는 것처럼 보여주어, 이를 논리 프로세서(logical processor)라고 부른다. 대표적으로 [인텔](https://www.intel.com)의 [하이퍼스레딩](https://ko.wikipedia.org/wiki/하이퍼스레딩), AMD의 [Zen 마이크로아키텍처](https://en.wikipedia.org/wiki/Zen_(microarchitecture)) 등이 SMT에 해당한다.

### 프로세서 점유
프로세서가 ([시스템](Process.md#시스템-프로세스)) [프로세스](Process.md)의 스레드로부터 [이미지](https://ko.wikipedia.org/wiki/실행_파일) 코드를 연산하고 처리하는 데 할애한 시간을 [프로세서 시간](https://ko.wikipedia.org/wiki/CPU_타임)(processor time)이라고 부른다. 프로세스의 프로세서 시간은 [스케줄링](#스케줄링)이나 입출력 요청 등에 의해 대기 혹은 준비 상태에 진입하여 처리되지 않는 동안 반영되지 않는다. 일정한 간격으로 샘플링된 시간 동안 스레드가 얼마나 오래 처리되었는지를 토대로 프로세서 점유율(%)이 계산된다.

## 프로세서 모드
[보호 링](https://ko.wikipedia.org/wiki/보호_링)(protection ring)은 데이터와 기능을 결함과 위협적인 행위로부터 보호하는 메커니즘이다.

![x86 프로세서의 보호 링 다이어그램](https://upload.wikimedia.org/wikipedia/commons/2/2f/Priv_rings.svg)

보호 링은 시스템 운영체제의 [권한](https://en.wikipedia.org/wiki/Privilege_(computing))(privilege) 구조를 이루는 계층으로써, CPU 구조가 하드웨어적으로 어떤 [모드](https://en.wikipedia.org/wiki/CPU_modes)에 있는지에 따라 권한에 의해 제한된 일부 명령어들 활용 가능여부가 결정된다. 해당 명령어들은 CPU 및 메모리와 같은 하드웨어를 직접적으로 상호작용하므로 자칫 잘못하면 시스템에 치명적인 문제를 야기한다.

[윈도우 NT](Windows.md) 운영체제는 만일을 대비해 x86 프로세서가 제공하는 네 개의 링 계층 중에서 오로지 Ring 0 그리고 Ring 3만 사용한다:

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">윈도우 NT에서 사용하는 x86 프로세서 모드</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/보호_링#수퍼바이저_모드">커널 모드</a> (<a href="Kernel.md#커널">슈퍼바이저</a> 모드)</th><th style="text-align: center;">사용자 모드</th></tr></thead>
<tbody><tr style="text-align: center;"><td>Ring 0</td><td>Ring 3</td></tr><tr><td>시스템에 민감한 영향을 줄 수 있는 입출력 동작이나 메모리 접근에 아무런 제약을 받지 않고 아키텍처의 모든 작업을 수행할 수 있다.</td><td>하드웨어 상호작용 및 커널 구성요소 접근에 대한 제한이 존재한다. 커널 함수가 필요하면 <a href="WinAPI.md#시스템-서비스">시스템 호출</a>을 통해 CPU를 커널 모드로 전환해야 한다.</td></tr><tr style="text-align: center;"><td><a href="Process.md#가상-주소-공간">가상 주소 공간</a>: 커널 공간</td><td><a href="Process.md#가상-주소-공간">가상 주소 공간</a>: 사용자 공간</td></tr><tr style="text-align: center;"><td><a href="Kernel.md#커널">커널</a> 및 <a href="Driver.md">드라이버</a></td><td>응용 프로그램의 <a href="Process.md">프로세스</a></td></tr></tbody>
</table>

이렇게 보호 링이 분류된 이유는 "더 많은 제어에는 더 큰 책임이 뒤따른다"는 관점에서 비롯된다. 커널 모드의 프로그램 오동작은 시스템 전체에 충돌을 일으켜 [블루스크린](BSOD.md)을 야기하는 원인이기 때문에 매우 신중하게 개발되어야 한다.

# 스케줄링
[스케줄링](https://ko.wikipedia.org/wiki/스케줄링_(컴퓨팅))(scheduling)은 처리되어야 할 리소스(즉, [프로세스](Process.md))의 작업(즉, [스레드](Process.md#스레드))을 사용 가능한 [프로세서 코어](#프로세서-코어)에 분배 및 할당하는 행위이며, 이를 담당하는 프로그램을 스케줄러(scheduler)라고 부른다. 스케줄링에 의해 운영체제는 [멀티태스킹](https://ko.wikipedia.org/wiki/다중작업)이 가능해져 여러 프로그램 및 기능을 동시에 실행할 수 있는 것처럼 보여진다.

스케줄링된 프로세스는 [퀀텀](https://en.wikipedia.org/wiki/Preemption_(computing)#Time_slice)(quantum)이란 일회성 시간제 티켓을 부여받는데, 이는 CPU가 프로세스를 처리하는 데 주어진 고정된 시간이다. 처리 도중에 프로세스의 퀀텀이 소진될 시, CPU는 해당 작업을 완료여부와 상관없이 즉각 중단하고 [실행 큐](#프로세서-실행-큐)에서 스케줄링 대기 중인 다음 프로세스를 처리한다. 한편, 작업이 마무리되지 못한 프로세스는 다시 실행 큐로 되돌아가 스케줄링이 되기를 기다린다.

> [윈도우](Windows.md) 운영체제는 [선점형](https://ko.wikipedia.org/wiki/스케줄링_(컴퓨팅)#비선점형과_선점형) [라운드 로빈](https://ko.wikipedia.org/wiki/라운드_로빈_스케줄링)(pre-emptive round-robin) 스캐줄링 알고리즘을 사용한다.

### 프로세서 실행 큐
각 프로세서 코어마다 처리될 스레드가 대기하는 [실행 큐](https://en.wikipedia.org/wiki/Run_queue)(run queue)가 존재하며, [우선순위](#스케줄링-우선순위)가 높은 순서대로 나열된다. 실행 큐에 대기할 수 있는 프로세스 개수에는 제한이 없으나, 이로 인해 [문맥 교환](#문맥-교환)이 빈번하게 일어나면 성능 저하의 요인으로 작용할 수 있다. 멀티프로세서 시스템의 경우, 실행 큐에 대기 중인 프로세스 개수는 프로세서 코어의 [논리 프로세서](#논리-프로세서) 개수만큼 나누어 계산하도록 한다.

## 문맥 교환
[문맥](https://en.wikipedia.org/wiki/Context_(computing))(context)은 프로세스 및 스레드가 중단된 시점으로부터 작업을 재개하기 위해 필요한 정보들이다: [스택](https://ko.wikipedia.org/wiki/콜_스택), [레지스터](https://ko.wikipedia.org/wiki/프로세서_레지스터) 등이 해당한다. 그리고 [문맥 교환](https://ko.wikipedia.org/wiki/문맥_교환)(context switch)은 실행 중인 프로세스 및 스레드를 나중에 재개할 수 있도록 문맥을 저장하여 대기시키고, 처리되어야 할 문맥을 불러와 실행하는 절차를 가리킨다. 스케줄링과 함께 멀티태스킹을 가능하게 만드는 핵심 기능이다.

> 프로세스의 문맥보다 스레드의 문맥 데이터가 더 작은 관계로, 멀티프로세스보다 멀티스레드의 문맥 교환 작업이 더 빨리 이루어진다.

일반적으로 문맥 교환이 이루어지는 요인으로 다음과 같다:

1. 스케줄링된 퀀텀 소진
2. 우선순위에 의한 선점
3. 프로세스의 대기 상태 전환

## 프로세스 상태
[프로세스 상태](https://en.wikipedia.org/wiki/Process_state)(process state)는 현재 프로세스가 어떠한 상태에 있는지를 가리키며, 크게 세 가지로 분류된다: 준비(ready), 실행(running), 그리고 대기(waiting) 상태가 있다. 윈도우 운영체제는 프로세스 상태에 따라 어떻게 처리할 지 결정한다. 다음은 프로세스 상태의 설명 및 전환되는 경우를 소개한다.

![프로세스의 수명 주기를 상태와 함께 표시한 다이어그램](https://www.researchgate.net/profile/Xiangyu-Lin-2/publication/346492253/figure/fig3/AS:963825763368962@1606805377750/Process-state-transition.png)

<table style="width: 100%; margin: auto;">
<caption style="caption-side: top;">프로세스 상태 및 전환</caption>
<colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup>
<thead><tr><th style="text-align: center;">프로세스 상태</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;">생성<br/>(Created)</td><td>프로세스가 처음으로 생성되었을 때의 상태이며, 곧바로 준비 상태로 전환된다.</td></tr>
<tr><td style="text-align: center;">준비<br/>(Ready)</td><td>프로세서가 즉시 실행할 수 있는 준비된 상태이다.<ul style="list-style-type: '→ ';"><li style="margin: 5px 0;"><b>실행</b>: 스케줄링에 의해 프로세스를 처리할 프로세서가 지정 및 배치되면서 전환된다.</li></ul></td></tr>
<tr><td style="text-align: center;">실행<br/>(Running)</td><td>프로세서에 의해 현재 실행되고 있는 상태이다.<ul style="list-style-type: '→ ';"><li style="margin: 5px 0;"><b>준비</b>: 퀀텀 소진, <a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-sleep"><code>Sleep</code></a> 함수, 혹은 우선순위에 밀리면 준비 상태로 전환된다.</li><li style="margin: 5px 0;"><b>대기</b>: <a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject"><code>WaitingForSingleObject</code></a> 또는 <code>Sleep</code> 함수로 대기 중인 프로세스는 타 프로세스의 도움을 받거나 커널 동작을 기다려야 한다.</li></ul></td></tr>
<tr><td style="text-align: center;">대기<br/>(Waiting)</td><td>특정 이벤트가 발생하기 전까지는 프로세서로부터 실행될 수 없는 유예 상태이다.<ul style="list-style-type: '→ ';"><li style="margin: 5px 0;"><b>준비</b>: 대기 상태에서 탈출한 프로세스를 처리할 잔여 프로세서가 없을 시 전환된다.</li><li style="margin: 5px 0;"><b>실행</b>: 대기 상태에서 탈출한 프로세스를 처리할 잔여 프러세서가 있거나, 혹은 높은 우선순위에 의해 곧바로 처리될 시 전환된다.</li></ul></td></tr>
<tr><td style="text-align: center;">종료<br/>(Terminated)</td><td>외부에 의해 강제로, 혹은 코드에 의해 종료되었을 시 전환되는 상태이다.</td></tr></tbody>
</table>

## 스케줄링 우선순위
[스케줄링 우선순위](https://learn.microsoft.com/en-us/windows/win32/procthread/scheduling-priorities)(scheduling priorities)는 프로세스와 스레드가 생성될 클래스로부터 정해진 우선순위에 따라 결정된 스케줄링 기준이다. 우선순위 범위는 1(최하) ~ 31(최상)까지 있으며, 특수한 경우의 우선순위 0은 오로지 시스템 운영 및 보안을 위한 것으로 [영값 페이지 커널 스레드](Memory.md#여유-메모리) 등에 사용된다. 동일한 우선순위 간에는 라운드 로빈 알고리즘에 의해 공평하게 스케줄링한 다음 차우선순위로 넘어간다.

윈도우 NT에서는 프로세스와 스레드의 우선순위 클래스 및 레벨은 다음과 같다:

* 프로세스 우선순위 클래스

    * `IDLE_PRIORITY_CLASS`
    * `BELOW_NORMAL_PRIORITY_CLASS`
    * `NORMAL_PRIORITY_CLASS`
    * `ABOVE_NORMAL_PRIORITY_CLASS`
    * `HIGH_PRIORITY_CLASS`
    * `REALTIME_PRIORITY_CLASS`

* 스레드 우선순위 레벨
    
    * `THREAD_PRIORITY_IDLE`
    * `THREAD_PRIORITY_LOWEST`
    * `THREAD_PRIORITY_BELOW_NORMAL`
    * `THREAD_PRIORITY_NORMAL`
    * `THREAD_PRIORITY_ABOVE_NORMAL`
    * `THREAD_PRIORITY_HIGHEST`
    * `THREAD_PRIORITY_TIME_CRITICAL`

프로세스의 우선순위 클래스와 스레드의 우선순위 레벨의 조합으로부터 각 스레드의 기초 우선순위(base priority)가 형성된다.

# 인터럽트
[인터럽트](https://ko.wikipedia.org/wiki/인터럽트)(interrupt; 간혹 "트랩"이라고도 언급)는 마우스 움직임이나 키보드 입력과 같은 시스템에서 발생한 일종의 비동기 사건, 즉 이벤트(event)가 최우선으로 처리될 수 있도록 [프로세서](#프로세서)에 요청되는 신호이다. 아래 그림은 인터럽트가 프로세서로부터 처리되는, 즉 인터럽트 서비스(interrupt service) 과정을 간략히 보여준다.

![인터럽트의 종류 및 처리 과정](https://upload.wikimedia.org/wikipedia/commons/c/cf/Interrupt_Process.PNG)

인터럽트를 전달받은 프로세서는 이벤트를 처리하기 위해 현재 실행 중이던 [스레드](Process.md#스레드)를 잠시 중단시키고 재개되어야 할 시점의 스레드 [상태](https://en.wikipedia.org/wiki/State_(computer_science))(state)를 저장한다. 각 인터럽트마다 대응되는 함수를 [인터럽트 핸들러](https://ko.wikipedia.org/wiki/인터럽트_핸들러)(interrupt handler) 또는 인터럽트 서비스 루틴(interrupt service routine; ISR)이라고 부르는데, 프로세서에 의해 실행되는 ISR이 바로 이벤트를 처리하는 역할을 한다.

> ISR은 실행 중이던 스레드를 중단시켜 프로세서를 점유한 것이므로 최대한 빠른 시간 내에 처리되어야 한다. 그러나 ISR의 작업들이 많아질수록 인터럽트 처리 시간이 길어지는데, 이는 스레드가 재개되는 시점을 미루거나 새로운 인터럽트가 제때 처리되지 못하게 한다. 윈도우 운영체제는 [DPC](#지연-프로시저-호출)를 제공하므로써 이러한 문제를 해소한다.

마지막으로 프로세서가 중단된 스레드를 다시 실행하므로써 일련의 인터럽트 서비스가 마무리된다.

### 하드웨어 인터럽트
[하드웨어 인터럽트](https://en.wikipedia.org/wiki/Interrupt#Hardware_interrupts)(hardware interrupt)는 마우스, 키보드, 프린터 등의 컴퓨터 [하드웨어](https://ko.wikipedia.org/wiki/컴퓨터_하드웨어) 또는 [외부 장치](https://ko.wikipedia.org/wiki/주변기기)가 운영체제와 통신 및 상호작용을 하기 위해 발신되는 인터럽트이다.

> 운영체제는 하드웨어마다 발생되는 인터럽트에 각자 다른 [IRQ](https://ko.wikipedia.org/wiki/인터럽트_요청) 값을 지정하여 장치를 분별한다. 만일 두 개 이상의 장치가 동일한 IRQ로 전송하면 [혼선](https://en.wikipedia.org/wiki/Interrupt_request_(PC_architecture)#Conflicts)이 발생하여 시스템 [프리징](https://ko.wikipedia.org/wiki/프리징_(컴퓨팅))을 야기한다.

비록 하드웨어 인터럽트는 시스템에 아무 때나 도달할 수 있으나, 결국 프로세서의 [클럭 발진기](https://en.wikipedia.org/wiki/Clock_generator)에 의해 동기화된다.

### 소프트웨어 인터럽트
[소프트웨어 인터럽트](https://en.wikipedia.org/wiki/Interrupt#Software_interrupts)(software interrupt)는 운영체제가 컴퓨터 하드웨어 또는 외부 장치와 통신 및 동작을 지시하기 위해, [특정 명령어](https://ko.wikipedia.org/wiki/INT_(x86_명령어))를 실행하거나 조건에 부합하면 프로세서로부터 발신되는 인터럽트이다. 일반적으로 소프트웨어 인터럽트는 운영체제의 커널단에서 수신받아 처리된다.

> 흔히 커널 프로세스에 나타나서는 안될 특정 소프트웨어 인터럽트가 존재하나, 모종의 이유로 해당 신호가 발신되었다면 [시스템 충돌](BSOD.md)을 초래할 수 있다.

프로그램이 실행되는 도중에 발생한 [예외](https://ko.wikipedia.org/wiki/예외_처리)(exception)도 소프트웨어 인터럽트에 해당한다.

## 지연 프로시저 호출
[지연 프로시저 호출](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-dpcs)(deferred procedure calls; DPC)은 [큐](https://ko.wikipedia.org/wiki/큐_(자료_구조))에 대기된 작업들을 나중에 한꺼번에 처리하는 매커니즘이다.

인터럽트 핸들러는 기존 스레드를 중단시켜 프로세서를 점유한 것이기 때문에, 원활한 스레드 재개를 위해 인터럽트 서비스는 되도록 빠른 시간 내에 완료되어야 한다. 하지만 인터럽트 핸들러가 처리하는 작업이 많아질수록 인터럽트 서비스 완료에 필요한 시간은 더 길어지게 된다. 이는 스레드가 다시 실행되는 시점을 늦추고 또 다른 인터럽트 처리를 방해하여 매끄럽지 못한 시스템 성능을 야기할 수 있다.

DPC는 필연적이지만 나중에 처리되어도 무관한 낮은 우선순위의 작업들을 실행할 수 있도록 다음 두 가지 특성을 지닌다:

1. 인터럽트 서비스가 완료되면 큐에 대기된 작업들이 순서대로 처리된다.
2. 인터럽트보다 낮은 IRQL을 가져 새로운 인터럽트 신호가 수신되면 DPC 처리 작업은 서비스 완료 때까지 잠시 중단된다.

> 과거 운영체제는 인터럽트 핸들러를 1차(First-Level Interrupt Handler; FLIH)와 2차(SLIH)로 나누었으며, 여기서 후자가 윈도우 운영체제의 DPC에 해당한다.

그러나 단일 및 누적 DPC 작업 처리 시간이 각각 20초와 120초를 초과하면 시스템은 이를 비정상 동작으로 인지하여 [중지코드](BSOD.md#버그-검사-코드) [0x133 DPC_WATCHDOG_VIOLATION](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/bug-check-0x133-dpc-watchdog-violation)이 발생한다.