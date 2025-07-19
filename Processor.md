# 프로세서
**[프로세서](https://ko.wikipedia.org/wiki/중앙_처리_장치)**(processor), 또는 CPU로 알려진 하드웨어는 프로그램의 [기계어](https://ko.wikipedia.org/wiki/기계어)를 처리하는 [전자회로](https://en.wikipedia.org/wiki/Electronic_circuit)이다. 프로그램을 실행하기 위해 필요한 시스템의 다양한 작업들은 [명령어 집합](https://en.wikipedia.org/wiki/Instruction_set_architecture)에 의해 정의되어 있으며, 대표적으로 [x86](https://en.wikipedia.org/wiki/X86)과 [ARM](https://en.wikipedia.org/wiki/ARM_architecture_family) 계열 명령어 집합 아키텍처가 존재한다. 프로세서에서 처리하는 명령어 집합이 무엇인지에 따라 [운영체제](https://en.wikipedia.org/wiki/Operating_system)의 아키텍처가 함께 결정된다.

### 명령어
> *출처: [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)*

프로세서가 처리하게 될 [**명령어**](https://en.wikipedia.org/wiki/Instruction_set_architecture#Instructions)는 일반적으로 두 가지 필드로 구성되어 있다.

![MIPS32 Add Immediate 명령의 구조](https://upload.wikimedia.org/wikipedia/commons/2/2a/Mips32_addi.svg)

1. [명령 코드](https://en.wikipedia.org/wiki/Opcode)(일명 opcode)는 덧셈, 복사, 이동 등의 프로세서가 수행할 연산 작업을 명시한다.
1. [피연산자](https://en.wikipedia.org/wiki/Operand#Computer_science)는 opcode에 따라 아예 없거나 한 개 이상이 [레지스터](#레지스터), [리터럴](C.md#구문) 및 [상수](C.md#변수), 또는 [메모리 주소](C.md#포인터)로서 활용된다.

본 문서에서는 명령어가 무엇인지 소개하며, 자세한 내용은 [어셈블리](Assembly.md#명령어) 문서에서 다룬다. 만일 어떠한 명령어가 있는지 확인하고 싶다면 위의 참고 링크로부터 다운로드한 소프트웨어 개발자 매뉴얼 문서에서 *Volume 2: Instruction Set Reference, A-Z*를 참고한다.

### 원자성 연산
**[원자성 연산](https://en.wikipedia.org/wiki/Linearizability)**(atomic operation)이란, 타 스레드의 간섭이 불가하고 더 이상 쪼갤 수 없는 최소 단위의 연산이다. 다시 말해, "원자성"이라고 하면 스레드가 작업이 완료될 때까지 다른 스레드는 기다릴 수 밖에 없는 구조를 의미한다. 간단한 CPU [명령어](#명령어)라도 다수의 기초적인 연산 작업이 동원되며, 그 예시로 [ADD](https://www.felixcloutier.com/x86/add) 명령어를 수행하기 위해 필요한 원자성 연산들을 단계대로 소개한다.<sup>[[참고](https://en.wikipedia.org/wiki/Instruction_cycle#Summary_of_stages[3])]</sup>

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">ADD 명령어의 연산 수행 단계</caption><colgroup><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">단계</th><th style="text-align: center;">프로세서 작업</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td><a href="https://en.wikipedia.org/wiki/Instruction_cycle#Fetch_stage">Fetch</a></td><td>메모리로부터 명령어를 확보한다.</td></tr><tr><td style="text-align: center;">2</td><td><a href="https://en.wikipedia.org/wiki/Instruction_cycle#Decode_stage[6]">Decode</a></td><td>확보한 명령어를 해독하여 메모리 주소에 값을 더하는 명령인 걸 파악한다.</td></tr><tr><td style="text-align: center;">3</td><td>Read</td><td>메모리 주소의 현재 값을 읽는다.</td></tr><tr><td style="text-align: center;">4</td><td><a href="https://en.wikipedia.org/wiki/Instruction_cycle#Execute_stage">Execute</a></td><td>덧셈 연산을 수행한다.</td></tr><tr><td style="text-align: center;">5</td><td>Write</td><td>덧셈 연산의 결과값을 메모리 주소에 쓴다.</td></tr></tbody></table>

일부 명령어들은 (x86의 경우) [LOCK](https://www.felixcloutier.com/x86/lock) 접두사와 함께 사용되어 원자성 연산될 수 있으며, [윈도우 OS](Windows.md)에서는 이를 [인터락 함수](Synchronization.md#인터락-함수)로서 제공한다.

## 프로세서 구조
다음은 단일 코어 프로세서의 구조를 다이어그램으로 보여주며, 황색 바탕의 "Processor"는 실질적인 연산을 담당하는 물리적인 [프로세서 코어](#프로세서-코어)를 의미한다. 그리고 흑색과 적색 화살표는 각각 데이터와 제어 흐름 방향을 가리킨다.

![단일 프로세서 중앙 처리 장치](https://upload.wikimedia.org/wikipedia/commons/3/3a/ABasicComputer.svg)

* [제어 장치](#제어-장치)
* [산술 논리 장치](#산술-논리-장치)
* [레지스터](#레지스터)

### 제어 장치
**[제어 장치](https://en.wikipedia.org/wiki/Control_unit)**(Control Unit; CU)는 프로세서의 연산 작업을 지휘하는 CPU의 구성요소이다. [명령어](#명령어)로부터 식별된 opcode는 [이진 디코더](https://en.wikipedia.org/wiki/Binary_decoder)에 의해 해당 연산 작업을 수행하는데 필요한 타이밍 및 제어 신호로 변환된다. 결과적으로 CPU와 타 장치들 간 데이터 흐름을 지휘하기 위해 대부분의 컴퓨터 리소스를 관리한다.

### 산술 논리 장치
**[산술 논리 장치](https://en.wikipedia.org/wiki/Arithmetic_logic_unit)**(Arithmetic Logic Unit; ALU)는 CPU의 구성요소 중 이진 정수의 [산술](https://en.wikipedia.org/wiki/Arithmetic) 및 [비트 연산](https://en.wikipedia.org/wiki/Bitwise_operation)을 담당하면서도 실질적인 컴퓨터 연산처리에서 가장 핵심되는 장치이다.

> [부동소수점](https://en.wikipedia.org/wiki/Floating-point_arithmetic) 연산을 담당하는 [FPU](https://en.wikipedia.org/wiki/Floating-point_unit)가 있으나, 주로 수학 계산을 위해 사용되며 초창기 CPU에는 내장되지 않은 장치이다.

![프로세서 ALU의 입출력 데이터를 표현한 다이어그램](https://upload.wikimedia.org/wikipedia/commons/0/0f/ALU_block.gif)

ALU는 기본적으로 opcode와 피연산자를 입력받고, 해당 opcode 작업의 결과물을 출력한다. 만일 [상태 레지스터](https://en.wikipedia.org/wiki/Status_register)로부터 입력을 받으면 [캐리](https://en.wikipedia.org/wiki/Carry_flag), [오버플로우](https://en.wikipedia.org/wiki/Overflow_flag) 여부 등의 상태가 이전 작업에서 발생하였는지 확인할 수 있으며, 현재 작업에서 발생한 상태 정보는 다시 상태 레지스터로 출력된다.

### 레지스터
**[레지스터](https://ko.wikipedia.org/wiki/프로세서_레지스터)**(register)는 CPU에 내장되어 프로세서가 가장 빨리 접근할 수 있는 (8비트, 16비트, 32비트 등) [비트](https://en.wikipedia.org/wiki/Bit) 단위의 소규모 임시 [저장공간](Storage.md)이다. 아래 그림은 [x86-64](https://en.wikipedia.org/wiki/X86-64) 아키텍처의 프로세서에 탑재된 레지스터들이다.

![x86-64 아키텍처의 프로세서 레지스터](https://upload.wikimedia.org/wikipedia/commons/1/15/Table_of_x86_Registers_svg.svg)

하드웨어 기능을 제어하는 [제어 레지스터](https://en.wikipedia.org/wiki/Control_register)(CR)나 작업 상태를 나타내는 [FLAGS 레지스터](https://en.wikipedia.org/wiki/FLAGS_register)(RFLASG)와 같이 일부는 특수한 목적을 지닌다. 한편, 다양한 용도로 활용될 수 있는 [범용 레지스터](Assembly.md#범용-레지스터)는 [어셈블리](Assembly.md) 언어를 참고하도록 한다.

### CPU 캐시
**[CPU 캐시](https://en.wikipedia.org/wiki/CPU_cache)**(CPU cache)는 [메모리](Memory.md)의 데이터를 접근하는데 소모되는 시간 및 에너지를 절약하기 위한 도입된 CPU상 [하드웨어 캐시](https://en.wikipedia.org/wiki/Cache_(computing)#HARDWARE)이다. 용량은 작지만 매우 빠른 메모리로 프로세서 코어와 근접하여 자주 사용되는 메모리 주소의 데이터 복사본을 저장한다. 단, 메모리에서 CPU로 캐싱되는 데이터는 (필요한 만큼의 크기가 아닌) "[캐시 라인](https://en.wikipedia.org/wiki/CPU_cache#Cache_entries)"이란 고정된 크기만큼 복사된다.

* 대부분 CPU는 L1 및 L2, 그리고 L3 이상의 [캐시 계층](https://en.wikipedia.org/wiki/Cache_hierarchy)을 가진다.

## 프로세서 규격
아래 [작업 관리자](TaskMgr.md)에 나타난 CPU 성능 정보를 예시로 시스템에 영향을 미치는 프로세서의 규격에 대해 설명한다.

![작업 관리자에서 표시된 CPU 성능 정보 예시](./images/processor_taskmgr.png)

* [프로세서 시간](#프로세서-시간) (%)
* [프로세서 클럭](#프로세서-클럭) (GHz)
* [프로세서 코어](#프로세서-코어)

### 프로세서 시간
**[프로세서 시간](https://en.wikipedia.org/wiki/CPU_time)**(processor time), 일명 **프로세서 점유**(processor utilization)는 [프로세스](Process.md) (또는 [스레드](Thread.md))로부터 [이미지](https://en.wikipedia.org/wiki/Executable) 코드를 연산하고 처리하는 데 할애한 시간이다. 프로세스의 프로세서 시간은 [스케줄링](#스케줄링)이나 입출력 요청 등에 의해 대기 혹은 준비 상태에 진입하여 처리되지 않는 동안 반영되지 않는다. 일정한 간격으로 샘플링된 시간 동안 스레드가 얼마나 오래 처리되었는지를 토대로 프로세서 점유율(%)이 계산된다.

### 프로세서 클럭
**[클럭 속도](https://en.wikipedia.org/wiki/Clock_rate)**(clock rate 또는 clock speed), 일명 **클럭 주파수**(clock frequency)는 [메인보드](https://en.wikipedia.org/wiki/Motherboard)에 위치한 [클럭 발생기](https://en.wikipedia.org/wiki/Clock_generator)로부터 프로세서가 작업을 수행하는데 기준이 되는 [클럭 신호](https://en.wikipedia.org/wiki/Clock_signal)가 얼마나 빈번히 생성될 수 있는지 나타내는 [헤르츠](https://en.wikipedia.org/wiki/Hertz)(Hz) 단위의 수치이다. 프로세서의 연산은 클럭 신호에 동기화되어, 주파수가 높을수록 동일한 시간동안 더 많은 작업을 수행할 수 있다. 그러나 클럭 속도는 프로세서가 초당 작업한 개수를 의미하는 게 절대 아니다.

> 다시 말해, 위의 작업 관리자에서 클럭 속도가 4.24 GHz로 측정되었지만 프로세서가 초당 42.4억 개의 작업을 수행한 걸 의미하는 게 아니다. 이는 한 [명령어](#명령어)을 수행하는 데 opcode에 따라 필요한 클럭 주기가 다양하기 때문이다.

* **[동적 주파수 스케일링](https://en.wikipedia.org/wiki/Dynamic_frequency_scaling)**(dynamic frequency scaling)

    필요에 따라 CPU 클럭 주파수가 동적으로 변하는 기술이며, 발열로 인해 전력을 절약하거나 (일명 CPU 스로틀링; CPU throttling) 작업 처리 성능을 높이기 위해 사용된다. 인텔은 전자와 후자의 경우에 대해 각각 [SpeedStep](https://www.intel.com/content/www/us/en/support/articles/000007073/processors.html) 및 [Turbo Boost](https://www.intel.com/content/www/us/en/gaming/resources/turbo-boost.html)을 소개한다.

    * `Processor Information\% Processor Performance`: 프로세서의 성능을 가리키는 속도가 기본 속도에 비해 비율을 계산한 것으로, 100% 보다 낮거나 높은지 여부에 따라 동적 주파수 스케일링 여부를 파악할 수 있다. 위의 예시에서 % Processor Performance는 대략 114.59%이다.

    * `Processor Information\% Processor Utility`: 프로세서 시간이 아닌 클럭 주파수를 함께 고려하여 성능에 따른 작업 처리 능률을 나타내는 수치이다. 윈도우 8부터 [작업 관리자](TaskMgr.md)의 프로세스 및 성능 탭의 CPU 사용량은 해당 카운터가 반영된다.<sup>[<a href="https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/cpu-usage-exceeds-100">출처</a>]</sup>

### 프로세서 코어
**프로세서 코어**(processor core)는 CPU에서 연산을 담당하는 물리적인 [전자회로](https://en.wikipedia.org/wiki/Integrated_circuit)를 가리키며, 하나의 칩에 두 개 이상의 프로세서 코어를 가진 CPU를 [멀티 코어](https://en.wikipedia.org/wiki/Multi-core_processor)라고 지칭한다. 프로세서 성능을 향상시키는데 [클럭 속도](#프로세서-클럭)의 기술적 제약을 극복하기 위해 하드웨어 제조사는 2005년에 처음으로 시장에 멀티 코어 CPU를 소개하였다. 멀티 코어 CPU는 여러 [스레드](Thread.md)를 동시에 처리할 수 있는 장점을 지닌다.

> 위의 작업 관리자는 시스템의 프로세서가 헥사코어, 즉 여섯 개의 코어를 가진 [AMD](https://www.amd.com) [Ryzen 5 5600X](https://www.amd.com/en/products/cpu/amd-ryzen-5-5600x)라고 표시하였다. 여기서 12개의 그래프 항목은 SMT에 의한 논리 프로세서 정보를 보여주는 것으로 아래 내용을 참고한다.

* **[동시 멀티스레딩](https://en.wikipedia.org/wiki/Simultaneous_multithreading)**(simultaneous multithreading; SMT)

    하드웨어적 [멀티스레딩](https://ko.wikipedia.org/wiki/멀티스레딩)을 지원하는 경우, 프로세서 코어는 매 클럭 주기마다 다수의 스레드의 명령어를 처리할 수 있다. 하나의 물리 코어가 마치 두 개 이상의 프로세서 역할을 처리하는 것처럼 보여주어, 이를 논리 프로세서(logical processor)라고 부른다. 대표적으로 [인텔](https://www.intel.com)의 [하이퍼스레딩](https://ko.wikipedia.org/wiki/하이퍼스레딩), AMD의 [Zen 마이크로아키텍처](https://en.wikipedia.org/wiki/Zen_(microarchitecture)) 등이 SMT에 해당한다.

* **[이기종 컴퓨팅](https://en.wikipedia.org/wiki/Heterogeneous_computing)**(heterogeneous computing)

    CPU 안에 두 개의 다른 주파수를 가진 프로세서 코어가 존재하여, 높은 주파수의 성능 위주 P-코어와 낮은 주파수의 효율 위주 E-코어로 나뉘어진다. 인텔은 이러한 하이브리드 CPU 설계를 12세대 코어 프로세서 [*Alder Lake*](https://en.wikipedia.org/wiki/Alder_Lake)부터 도입하고 있으며, 그 외에도 [ARM](https://en.wikipedia.org/wiki/Arm_Holdings)의 [DynamIQ](https://www.arm.com/technologies/dynamiq) (옛 [big.LITTLE](https://en.wikipedia.org/wiki/ARM_big.LITTLE))가 대표적이다.

## 리얼 모드
**[리얼 모드](https://en.wikipedia.org/wiki/Protected_mode)**(real mode)

## 보호 모드
**[보호 모드](https://en.wikipedia.org/wiki/Protected_mode)**(protected mode)

### 권한 수준
> *참고: [User mode and kernel mode - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)*

보호 모드의 x86 계열 프로세서는 시스템의 데이터와 기능을 위협적인 행위나 결함으로부터 보호하는 [보호 링](https://en.wikipedia.org/wiki/Protection_ring)(protection ring) 매커니즘을 사용한다.

![x86 프로세서의 보호 링 다이어그램](https://upload.wikimedia.org/wikipedia/commons/2/2f/Priv_rings.svg)

보호 링은 시스템 운영체제의 [권한](https://en.wikipedia.org/wiki/Privilege_(computing)) 구조를 이루는 계층으로써, CPU 구조가 하드웨어적으로 어떤 [모드](https://en.wikipedia.org/wiki/CPU_modes)에 있는지에 따라 권한에 의해 제한된 일부 명령어들 활용 가능여부가 결정된다. 해당 명령어들은 CPU 및 메모리와 같은 하드웨어를 직접적으로 상호작용하므로 자칫 잘못하면 시스템에 치명적인 문제를 야기한다.

[윈도우 NT](Windows.md)는 만일을 대비해 x86 계열 프로세서가 제공하는 네 개의 계층 중에서 오로지 Ring 0 그리고 Ring 3만 사용한다:

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">윈도우 NT에서 사용하는 x86 프로세서 모드</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">

#### [커널 모드](https://en.wikipedia.org/wiki/Protection_ring#Supervisor_mode)

</th><th style="text-align: center;">

#### 사용자 모드

</th></tr></thead><tbody><tr style="text-align: center;"><td>Ring 0</td><td>Ring 3</td></tr><tr><td>시스템에 민감한 영향을 줄 수 있는 입출력 동작이나 메모리 접근에 아무런 제약을 받지 않고 아키텍처의 모든 작업을 수행할 수 있다.</td><td>하드웨어 상호작용 및 커널 구성요소 접근에 대한 제한이 존재한다. 커널 함수가 필요하면 <a href="WinAPI.md#시스템-서비스">시스템 호출</a>을 통해 CPU를 커널 모드로 전환해야 한다.</td></tr><tr style="text-align: center;"><td><a href="Process.md#가상-주소-공간">가상 주소 공간</a>: 커널 공간</td><td><a href="Process.md#가상-주소-공간">가상 주소 공간</a>: 사용자 공간</td></tr><tr style="text-align: center;"><td><a href="Kernel.md#커널">커널</a> 및 <a href="Driver.md">드라이버</a></td><td>응용 프로그램의 <a href="Process.md">프로세스</a></td></tr></tbody></table>

커널 모드의 프로그램 오동작은 시스템 전체에 충돌을 일으켜 [블루스크린](BSOD.md)을 야기하는 원인이기 때문에 매우 신중하게 개발되어야 한다.

# 스케줄링
**[스케줄링](https://en.wikipedia.org/wiki/Scheduling_(computing))**(scheduling)이란, 작업([스레드](Thread.md))을 실행하기 위해 필요한 리소스([프로세서](#프로세서))를 할당시키는 행위를 가리키며, 이를 담당하는 코드를 [**스케줄러**](https://en.wikipedia.org/wiki/Scheduling_(computing)#SCHEDULER)라고 부른다. 스케줄링 기술은 단일 코어 프로세서 시스템에서도 다수의 프로그램 및 기능을 동시에 실행하는 [멀티태스킹](https://en.wikipedia.org/wiki/Computer_multitasking)을 가능케 한다. 스케줄되지 않은 프로세서는 [유휴 프로세스](https://en.wikipedia.org/wiki/System_Idle_Process)의 스레드를 실행하여 (외부 인터럽트가 발생할 때까지) CPU를 중단시키는 [HLT](https://en.wikipedia.org/wiki/HLT_(x86_instruction)) 명령을 반복적으로 수행한다.

* 다시 말해, 유휴 프로세스는 아무런 작업을 하지 않음을 의미한다. 해당 프로세스의 [CPU 사용량](#프로세서-시간)이 90%라면 시스템 전반이 사용하는 CPU는 10%이다.

### 선점형 라운드 로빈
[윈도우 OS](Windows.md)에서 채택한 [스케줄링 규율](https://en.wikipedia.org/wiki/Scheduling_(computing)#Scheduling_disciplines)은 [우선순위](#스케줄링-우선순위) 개념이 접목된 "[**선점형**](https://en.wikipedia.org/wiki/Preemption_(computing))" [**라운드 로빈**](https://en.wikipedia.org/wiki/Round-robin_scheduling)(preemptive round-robin)이다.

라운드 로빈이란, 모든 프로세스가 우선순위 상관없이 동일한 시간동안 균등하게 작업을 처리될 수 있는 스케줄링 알고리즘이다. 프로세스는 작업을 처리하기 위해 주어진 고정 시간, 즉 [퀀텀](https://en.wikipedia.org/wiki/Preemption_(computing)#Time_slice)(quantum)이란 일회성 시간제 티켓을 스케줄러로부터 할당 받는다. 퀀텀이 소진되면 프로세서는 완료 여부와 상관 없이 해당 작업을 즉시 중단하고 [큐](https://ko.wikipedia.org/wiki/큐_(자료_구조))의 맨 뒤로 되돌아가 스케줄링이 될 다음 순서까지 기다린다. 한편, 프로세서는 큐에 대기하다 퀀텀을 할당 받은 다음 프로세스를 처리한다.

### 스케줄링 우선순위
**[스케줄링 우선순위](https://learn.microsoft.com/en-us/windows/win32/procthread/scheduling-priorities)**(scheduling priorities)는 [프로세스](Process.md#프로세스) 또는 [스레드](Thread.md)가 타 객체에 비해 CPU로부터 얼마나 먼저 처리되어야 하는지 알리는 [필드](Cpp.md#클래스) 값이다. 우선순위 범위는 1 - 31이며 높을수록 스케줄링 최상위 우선권을 가진다. 특수한 경우의 우선순위 0은 오로지 시스템 운영 및 보안을 위한 것으로 [영값 페이지 커널 스레드](Memory.md#여유-메모리) 등에 사용된다. 동일한 우선순위 간에는 라운드 로빈 알고리즘에 의해 공평하게 스케줄링한 다음 차우선순위로 넘어간다.

다음은 [윈도우 NT](Windows.md) 운영체제의 우선순위 체계이며, 우선순위 클래스와 레벨의 조합으로부터 각 스레드의 기초 우선순위(base priority)가 형성된다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">프로세스 및 스레드 우선순위</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">프로세스 우선순위 클래스</th><th style="text-align: center;">스레드 우선순위 레벨</th></tr></thead><tbody><tr style="text-align: center;"><td><code>IDLE_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_IDLE</code></td></tr><tr style="text-align: center;"><td><code>BELOW_NORMAL_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_LOWEST</code></td></tr><tr style="text-align: center;"><td><code>NORMAL_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_BELOW_NORMAL</code></td></tr><tr style="text-align: center;"><td><code>ABOVE_NORMAL_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_NORMAL</code></td></tr><tr style="text-align: center;"><td><code>HIGH_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_ABOVE_NORMAL</code></td></tr><tr style="text-align: center;"><td><code>REALTIME_PRIORITY_CLASS</code></td><td><code>THREAD_PRIORITY_HIGHEST</code></td></tr><tr style="text-align: center;"><td>-</td><td><code>THREAD_PRIORITY_TIME_CRITICAL</code></td></tr></tbody></table>

## 컨텍스트 교환
> *참고: [Context Switching - OSDev Wiki](https://wiki.osdev.org/Context_Switching)*

**[컨텍스트](https://en.wikipedia.org/wiki/Context_(computing))**(context; 문맥)은 프로세스 및 스레드가 중단된 시점으로부터 작업을 재개하기 위해 필요한 정보들이다: [스택](https://ko.wikipedia.org/wiki/콜_스택), [레지스터](Assembly.md#레지스터) 등이 해당한다.

* [프로세스 컨텍스트](Process.md#프로세스-컨텍스트)
* [스레드 컨텍스트](Thread.md#스레드-컨텍스트)

**[컨텍스트 교환](https://en.wikipedia.org/wiki/Context_switch)**(context switch)은 실행 중인 프로세스 및 스레드를 나중에 재개할 수 있도록 컨텍스트를 저장하여 대기시키고, 처리되어야 할 컨텍스트를 불러와 실행하는 절차를 가리킨다. 스케줄링과 함께 멀티태스킹을 가능하게 만드는 핵심 기능이다. 그러나 컨텍스트 교환이 너무 빈번하게 일어나면 성능 저하의 요인으로 작용할 수 있다. 프로세스보다 스레드의 컨텍스트 데이터가 더 작은 관계로, 멀티프로세스보다 멀티스레드의 컨텍스트 교환 작업이 더 빨리 이루어진다.

일반적으로 컨텍스트 교환이 이루어지는 요인으로 다음과 같다:

1. 스케줄링된 [퀀텀](#선점형-라운드-로빈) 소진
2. [우선순위](#스케줄링-우선순위)에 의한 선점
3. 프로세스의 [대기 상태](#프로세스-상태) 전환

## 프로세스 상태
**[프로세스 상태](https://en.wikipedia.org/wiki/Process_state)**(process state)는 생성된 프로세스가 현 시점에 어떠한 상태에 있는지 나타내며, 이에 따라 스케줄링 여부가 결정한다. 비록 "프로세스"라고 언급을 하였으나 정확히 말하면 이미지 코드를 실행하는 프로세스의 "[스레드](Thread.md)"를 가리킨다. 다음은 프로세스 상태의 각 [종류](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.threadstate)마다 소개한다.

![프로세스 상태를 함께 나타낸 생명 주기 다이어그램](https://www.researchgate.net/profile/Xiangyu-Lin-2/publication/346492253/figure/fig3/AS:963825763368962@1606805377750/Process-state-transition.png)

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">프로세스 상태의 종류 및 설명</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">프로세스 상태</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">생성<br/>(Created; New)</td><td>프로세스 (또는 스레드)가 생성되었을 때의 상태이다.<br/><ul style="list-style-type: '→';"><li style="padding-left: 4px;">준비: <i>생성 및 초기화가 완료된 프로세스 (또는 스레드)는 자동으로 준비 상태로 전환된다.</i></li></ul></td></tr><tr><td style="text-align: center;">준비<br/>(Ready)</td><td>스레드가 큐에서 퀀텀을 할당 받으면 즉시 프로세서를 활용할 수 있는 준비된 (<a href="https://en.wikipedia.org/wiki/Idle_(CPU)">유휴</a>) 상태이다.<ul style="list-style-type: '→';"><li style="padding-left: 4px;">실행: <i>스케줄링에 의해 스레드가 퀀텀을 할당 받으면 전환된다.</i></li></ul></td></tr><tr><td style="text-align: center;">실행<br/>(Running)</td><td>프로세서에 의해 퀀텀을 할당 받은 작업이 처리되는, 다시 말해 프로세스 (또는 스레드)가 실행 중인 상태이다.<ul style="list-style-type: '→';"><li style="padding-left: 4px;">준비: <i>퀀텀이 소진되거나 우선순위에 밀려, 또는 <a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-sleep"><code>Sleep</code></a> 함수에 의해 큐로 되돌아가면 전환된다.</i></li><li style="padding-left: 4px;">대기: <i><a href="https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject"><code>WaitingForSingleObject</code></a> 또는 <code>Sleep</code> 등의 대기 함수에 의해 전환된다.</i></li></ul></td></tr><tr><td style="text-align: center;">대기<br/>(Waiting)</td><td>외부로부터 특정 이벤트가 발생하기 전까지 스레드는 퀀텀을 할당 받을 수 없는 유예 상태이다.<ul style="list-style-type: '→';"><li style="padding-left: 4px;">준비: <i>대기 상태 탈출 이후, 가용 프로세서가 없을 시 전환된다.</i></li><li style="padding-left: 4px;">실행: <i>대기 상태 탈출 이후, 가용 프로세서가 있거나 높은 우선순위를 가져 곧바로 실행될 시 전환된다.</i></li></ul></td></tr><tr><td style="text-align: center;">종료<br/>(Terminated)</td><td>프로세스 (또는 스레드)가 코드 실행 완료, 혹은 내부 코드나 외부의 강제적 요인에 의해 종료된 상태이다.</td></tr></tbody></table>

# 인터럽트
**[인터럽트](https://ko.wikipedia.org/wiki/인터럽트)**(interrupt), 간혹 **트랩**(trap)이라고도 언급되며 시스템에서 발생한 일종의 [비동기](https://en.wikipedia.org/wiki/Asynchronous_communication) 사건, 즉 이벤트가 최우선으로 처리될 수 있도록 [프로세서](#프로세서)에 요청되는 신호이다. 대표적인 예시로 마우스 커서 움직임이나 키보드 타자 입력 등이 인터럽트에 의해 처리된다. 인터럽트는 하드웨어 혹은 소프트웨어에 의해 발생될 수 있다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">인터럽트 유형별 개요</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Interrupt#Hardware_interrupts">하드웨어 인터럽트</a></th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Interrupt#Software_interrupts">소프트웨어 인터럽트</a></th></tr></thead><tbody><tr style="text-align: center;"><td>전달 발향: <a href="https://ko.wikipedia.org/wiki/컴퓨터_하드웨어">하드웨어</a> & <a href="https://ko.wikipedia.org/wiki/주변기기">외부 장치</a> → <a href="https://ko.wikipedia.org/wiki/운영체제">운영체제</a></td><td>전달 방향: <a href="https://ko.wikipedia.org/wiki/운영체제">운영체제</a> → <a href="https://ko.wikipedia.org/wiki/컴퓨터_하드웨어">하드웨어</a> & <a href="https://ko.wikipedia.org/wiki/주변기기">외부 장치</a></td></tr><tr><td>마우스, 키보드, 프린터 등의 장치가 운영체제와 통신하거나 정보를 전달하기 위해 발신하는 인터럽트이다. 운영체제는 하드웨어 인터럽트마다 각자 다른 <a href="https://ko.wikipedia.org/wiki/인터럽트_요청">IRQ</a> 값을 지정하여 장치를 분별한다.</td><td>프로세서는 <a href="https://ko.wikipedia.org/wiki/INT_(x86_명령어)">INT 명령어</a> 연산 혹은 특정 조건에 부합하면 발신되는 인터럽트이며, 일반적으로 <a href="Kernel.md#하드웨어-추상-계층">HAL</a> 상주하는 <a href="Kernel.md">커널</a>에서 수신한다. 프로그램이 실행되는 도중에 발생하는 <a href="C.md#예외-처리">예외</a>도 이에 해당한다.</td></tr></tbody></table>

### 인터럽트 요청 수준
**[인터럽트 요청 수준](https://en.wikipedia.org/wiki/IRQL_(Windows))**(interrupt request level; IRQL)은 인터럽트의 우선순의를 정의하며, 낮은 IRQL에서 실행 중인 스레드는 높은 IRQL의 코드에 의해 간섭받을 수 있다. IRQL 범위와 각 수준들의 역할은 프로세서마다 다르며, 아래는 x86과 amd64에 대해 간단히 소개한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">인터럽트 요청 수준의 명칭 및 설명&nbsp;<sup>[<a href="https://download.microsoft.com/download/e/b/a/eba1050f-a31d-436b-9281-92cdfeae4b45/IRQL_thread.doc">다운로드</a>]</sup></caption><colgroup><col style="width: 15%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 65%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">IRQL 명칭</th><th colspan="2" style="text-align: center; border-bottom-style: none;">수준</th><th rowspan="2" style="text-align: center;">설명</th></tr><tr><th style="text-align: center;">x86</th><th style="text-align: center;">AMD64</th></tr></thead><tbody><tr><td>PASSIVE_LEVEL</td><td style="text-align: center;">0</td><td style="text-align: center;">0</td><td><a href="#사용자-모드">사용자 모드</a> <a href="Thread.md">스레드</a>, 그리고 대부분의 <a href="#커널-모드">커널 모드</a> 작업</td></tr><tr><td>APC_LEVEL</td><td style="text-align: center;">1</td><td style="text-align: center;">1</td><td><a href="Thread.md#비동기-프로시저-호출">비동기 프로시저 호출</a>(APC) 및 <a href="Memory.md#페이지-부재">페이지 부재</a></td></tr><tr><td>DISPATCH_LEVEL</td><td style="text-align: center;">2</td><td style="text-align: center;">2</td><td><a href="#스케줄링">스레드 스케줄러</a> 및 <a href="#지연-프로시저-호출">지연 프로시저 호출</a>(DPC)</td></tr><tr><td>DIRQL</td><td style="text-align: center;">3-26</td><td style="text-align: center;">3-11</td><td>장치 인터럽트 (Device IRQL)</td></tr><tr><td>PROFILE_LEVEL</td><td style="text-align: center;">27</td><td style="text-align: center;">15</td><td><a href="https://en.wikipedia.org/wiki/Profiling_(computer_programming)">프로파일링</a> 타이머 (윈도우 2000 이전 빌드)</td></tr><tr><td>SYNCH_LEVEL</td><td style="text-align: center;">27</td><td style="text-align: center;">13</td><td><a href="#프로세서">프로세서</a> 전반의 코드 및 명령 스트림 동기화</td></tr><tr><td>CLOCK_LEVEL</td><td style="text-align: center;">N/A</td><td style="text-align: center;">13</td><td>클럭 타이머</td></tr><tr><td>CLOCK2_LEVEL</td><td style="text-align: center;">28</td><td style="text-align: center;">N/A</td><td>클럭 타이머 (x86 하드웨어 전용)</td></tr><tr><td>IPI_LEVEL</td><td style="text-align: center;">29</td><td style="text-align: center;">14</td><td>캐시 일관성을 위한 프로세스 간 인터럽트</td></tr><tr><td>POWER_LEVEL</td><td style="text-align: center;">30</td><td style="text-align: center;">14</td><td>전원 실패</td></tr><tr><td>HIGH_LEVEL</td><td style="text-align: center;">31</td><td style="text-align: center;">15</td><td><a href="https://en.wikipedia.org/wiki/Machine_Check_Architecture">기계 검사</a> 및 치명적 오류; 프로파일링 타이머 (윈도우 XP 이상 빌드)</td></tr></tbody></table>

<sup>_† 출처: [Managing Hardware Priorities - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/managing-hardware-priorities)<br/>‡ 참고문헌: [What is IRQL and why is it important? | Microsoft Community Hub](https://techcommunity.microsoft.com/blog/askperf/what-is-irql-and-why-is-it-important/372666)_</sup>

## 인터럽트 핸들러
**[인터럽트 핸들러](https://ko.wikipedia.org/wiki/인터럽트_핸들러)**(interrupt handler) 또는 **인터럽트 서비스 루틴**(interrupt service routine; ISR)은 발생한 인터럽트에 대응하는 [함수](C.md#함수)를 가리킨다. 인터럽트도 제각각 용도가 다르기 때문에, 인터럽트를 처리할 수 있는 ISR 또한 다양하게 존재한다. 아래는 프로세서가 인터럽트를 처리하는 "인터럽트 서비스" 과정을 소개한다.

![인터럽트의 유형에 따른 서비스 절차](https://upload.wikimedia.org/wikipedia/commons/c/cf/Interrupt_Process.PNG)

위의 그림을 정리하면 다음과 같다:

1. 이벤트로부터 발생한 인터럽트를 전달받은 프로세서는 이를 처리하기 위해 현재 실행 중이던 [스레드](Thread.md)를 잠시 중단한다.
1. 프로세서는 차후 작업을 재개할 수 있도록 중단된 스레드의 [상태](https://en.wikipedia.org/wiki/State_(computer_science))를 저장한다.
1. 프로세서는 해당 인터럽트에 대응하는 ISR을 실행하여 이벤트를 처리한다.
1. ISR이 마무리되면 프로세서는 일시적으로 중단한 스레드 실행을 재개한다.

ISR은 실행 중이던 스레드를 중단시켜 프로세서를 점유한 것이므로 최대한 빠른 시간 내에 처리되어야 한다. 그러나 ISR의 작업들이 많아질수록 인터럽트 처리 시간이 길어지는데, 이는 스레드가 재개되는 시점을 미루거나 새로운 인터럽트가 제때 처리되지 못하게 한다. 윈도우 운영체제는 [DPC](#지연-프로시저-호출)를 제공하므로써 이러한 문제를 해소한다.

## 지연 프로시저 호출
**[지연 프로시저 호출](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-dpcs)**(deferred procedure calls; DPC)은 [큐](https://ko.wikipedia.org/wiki/큐_(자료_구조))에 대기된 작업들을 나중에 한꺼번에 처리하는 매커니즘이다.

ISR은 기존 스레드를 중단시켜 프로세서를 점유한 것이기 때문에, 원활한 스레드 재개를 위해 인터럽트 서비스는 되도록 빠른 시간 내에 완료되어야 한다. 하지만 인터럽트 핸들러가 처리하는 작업이 많아질수록 인터럽트 서비스 완료에 필요한 시간은 더 길어지게 된다. 이는 스레드가 다시 실행되는 시점을 늦추고 또 다른 인터럽트 처리를 방해하여 매끄럽지 못한 시스템 성능을 야기할 수 있다.

DPC는 필연적이지만 나중에 처리되어도 무관한 낮은 우선순위의 작업들을 실행할 수 있도록 다음 두 가지 특성을 지닌다:

1. 인터럽트 서비스가 완료되면 큐에 대기된 작업들이 순서대로 처리된다.
2. 인터럽트보다 낮은 IRQL을 가져 새로운 인터럽트 신호가 수신되면 DPC 처리 작업은 서비스 완료 때까지 잠시 중단된다.

> 과거 운영체제는 인터럽트 핸들러를 1차(First-Level Interrupt Handler; FLIH)와 2차(SLIH)로 나누었으며, 여기서 후자가 윈도우 운영체제의 DPC에 해당한다.

그러나 단일 및 누적 DPC 작업 처리 시간이 각각 20초와 120초를 초과하면 시스템은 이를 비정상 동작으로 인지하여 [중지코드](BSOD.md#버그-검사-코드) [0x133 DPC_WATCHDOG_VIOLATION](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/bug-check-0x133-dpc-watchdog-violation)이 발생한다.
