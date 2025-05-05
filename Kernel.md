# 커널
**[커널](https://en.wikipedia.org/wiki/Kernel_(operating_system))**(kernel), 일명 **슈퍼바이저**(supervisor)는 [운영체제](https://en.wikipedia.org/wiki/Operating_system)의 핵심 프로그램으로 일반적으로 시스템 전체에 대한 제어권을 가진다. [메모리](Memory.md)에 항상 상주하여 하드웨어와 소프트웨어 간 상호작용을 가능케 하여, 운영체제의 아래 역할을 담당한다.

* [장치 드라이버](Driver.md)를 통한 하드웨어 리소스 제어: 메모리, 입출력, 암호화 등
* [프로세스](Process.md) 간 위의 하드웨어 리소스 할당에 대한 갈등 중재
* 공통 리소스 활용의 최적화: [CPU](Processor.md) 및 캐시 사용률, 파일 시스템, 네트워크 소켓 등

## 아키텍처
운영체제 설계에 따라 커널은 크게 세 가지의 아키텍처로 설계될 수 있다.

![커널 설계에 따른 아키텍처](https://upload.wikimedia.org/wikipedia/commons/d/d0/OS-structure2.svg)

본 부문에서 자주 인용될 "서비스"란, 호출 가능한 커널 루틴 혹은 그 집합을 의미한다.

### 모놀리식 커널
**[모놀리식 커널](https://en.wikipedia.org/wiki/Monolithic_kernel)**(monolithic kernel)은 모든 서비스가 단일 프로그램으로 빌드되어 [커널 공간](Processor.md#권한-수준)에서 처리하는 구조이다. 단조로운 구조에 관리가 매우 편하고, 단일 프로그램에서 모든 커널 작업 및 서비스가 수행되니 성능 속도가 매우 빠르다. 단, 커널 공간의 특성상 사소한 오류가 시스템 전체에 영향을 줄 수 있는 위험이 항상 존재한다. 운영체제 런타임 도중에 장치 드라이버를 언제든지 불러올 수 있는 모듈성(modularity)을 지원한다.

마이크로소프트의 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 및 [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x) 시리즈가 모놀리식 커널를 사용하였다.

### 마이크로커널
**[마이크로커널](https://en.wikipedia.org/wiki/Microkernel)**(microkernel)은 운영체제 구동에서 기초적이지만 필연적인 저급 커널 서비스만을 제외한 나머지를 [사용자 공간](Processor.md#권한-수준)으로 분리시킨 구조이다: [스케줄링](Processor.md#스케줄링), 메모리 관리, 기초적인 [IPC](Process.md#프로세스-간-통신)가 최소한으로 마련되어야 할 서비스이다. 한편, 사용자 모드에서 고급 커널 서비스를 제공하는 프로그램들을 서버(server)라고 부른다.

> 마이크로커널의 기초적인 IPC 서비스는 사용자 모드에 위치한 서버 혹은 장치 드라이버 간 통신을 위해 반드시 필요한 기능이다.

모놀리식 커널의 단점인 방대한 커널 이미지로 인한 코드 관리의 어려움 및 장치 드라이버 충돌에 의한 커널 전체에 미치는 악영향을 해소하고자 고안되었다. 커널 서비스의 서버화는 커널 개발 및 업데이트 시간을 단축시키지만, 사용자 모드에 위치하여 하드웨어 접근성 효율이 떨어진다. IPC 통신으로 인한 성능 저하 및 커널 동작 절차의 복잡성도 단점으로 함께 지적되었다.

### 하이브리드 커널
**[하이브리드 커널](https://en.wikipedia.org/wiki/Hybrid_kernel)**(hybrid kernel)은 마이크로커널처럼 서비스에 따라 서버가 나뉘어져 있으나, 모놀리식 커널처럼 전부 (혹은 대부분) 커널 공간에 존재한다. 결국 시스템 안전성 취약점은 여전히 존재하지만, 마이크로커널에 비해 사용자 및 커널 모드 전환이 불필요하여 커널 서비스의 성능 [오버헤드](https://ko.wikipedia.org/wiki/오버헤드)가 없다.

마이크로소프트의 [윈도우 NT](Windows.md)가 하이브리드 커널의 영향을 받은 대표적인 운영체제이다.

# NT 커널
[윈도우 NT](Windows.md) 운영체제의 커널 이미지 ntoskrnl.exe는 아래와 같이 구성된다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">윈도우 NT 커널 이미지의 구성</caption><colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col/><col/><col/><col/><col/><col/><col/><col/><col/><col/></colgroup><thead><tr><th style="text-align: center;">이미지</th><th style="text-align: center;">계층</th><th colspan="10" style="text-align: center;">구성</th></tr></thead><tbody><tr><td rowspan="3" style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/Ntoskrnl.exe">ntoskrnl.exe</a><br/>(모듈명: <code>nt</code>)</td><td rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</td><td colspan="10" style="text-align: center;">시스템 서비스 담당자</td></tr><tr><td style="text-align: center;"><a href="#입출력-관리자">입출력 관리자</a></td><td style="text-align: center;">캐시 관리자</td><td style="text-align: center;">보안 참조 모니터</td><td style="text-align: center;"><a href="#전원-관리자">전원 관리자</a></td><td style="text-align: center;"><a href="#pnp-관리자">PnP 관리자</a></td><td style="text-align: center;"><a href="#메모리-관리자">메모리 관리자</a></td><td style="text-align: center;"><a href="#프로세스-관리자">프로세스 관리자</a></td><td style="text-align: center;"><a href="#개체-관리자">개체 관리자</a></td><td style="text-align: center;">구성 관리자</td><td style="text-align: center;">ALPC</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">Kernel</a></td><td colspan="10" style="text-align: center;"><a href="Processor.md#스케줄링">스케줄링</a>, 동기화, <a href="Processor.md#인터럽트">인터럽트</a> 등 기초적인 핵심 함수 제공</td></tr></tbody></table>

Executive는 특정 작업을 수행하는 여러 구성원들로 이루어진 ntoskrnl.exe의 상위 계층이다. 한편, Kernel 계층은 모듈에서 필요로 하는 기초적인 커널 함수들을 제공하는 하위 계층이며 [마이크로커널](#마이크로커널)의 역할을 담당한다. 이러한 구조의 정립으로 Kernel은 단순히 OS 매커니즘을 구현하고, Executive는 이를 활용하여 실질적인 정책 결정에 기여한다.

아래는 `nt` 모듈의 함수 접두사가 각각 어떤 목적으로 사용되는 지 식별하는 일부를 도표로 소개한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">NT 함수 접두사</caption><colgroup><col style="width: 10%;"/><col style="width: 40%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">접두사</th><th style="text-align: center;">영문</th><th style="text-align: center;">의미</th></tr></thead><tbody><tr><td style="text-align: center;"><code>Alpc</code></td><td>Asynchronous Local Procedure Call</td><td><a href="https://en.wikipedia.org/wiki/Local_Inter-Process_Communication">비동기 로컬 프로시저 호출</a></td></tr><tr><td style="text-align: center;"><code>Cc</code></td><td>Common cache</td><td>공통 캐시</td></tr><tr><td style="text-align: center;"><code>Cm</code></td><td>Configuration manager</td><td><a href="#구성-관리자">구성 관리자</a></td></tr><tr><td style="text-align: center;"><code>Csr</code></td><td>Client/server runtime</td><td><a href="Windows.md#윈도우-서브시스템">CSRSS</a> 서브시스템 프로세스</td></tr><tr><td style="text-align: center;"><code>Dbg</code></td><td>Kernel debug support</td><td>커널 디버그 지원</td></tr><td style="text-align: center;"><code>Etw</code></td><td>Event Tracing for Windows</td><td><a href="ETW.md">윈도우 이벤트 추적</a></td></tr><tr><td style="text-align: center;"><code>Ex</code></td><td>Executive support routines</td><td><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</a> 지원 루틴</td></tr><tr><td style="text-align: center;"><code>Hv</code></td><td>Hive Library</td><td><a href="Registry.md#레지스트리-하이브">하이브</a> 라이브러리</td></tr><tr><td style="text-align: center;"><code>Hvl</code></td><td>Hypervisor Library</td><td><a href="Hypervisor.md">하이퍼바이저</a> 라이브러리</td></tr><tr><td style="text-align: center;"><code>Hal</code></td><td>Hardware Abstraction Layer</td><td><a href="#하드웨어-추상-계층">하드웨어 추상 계층</a></td></tr><tr><td style="text-align: center;"><code>Io</code></td><td>I/O manager</td><td><a href="#입출력-관리자">입출력 관리자</a></td></tr><tr><td style="text-align: center;"><code>Kd</code></td><td>Kernel Debugger</td><td>커널 디버거</td></tr><tr><td style="text-align: center;"><code>Ke</code></td><td>Kernel</td><td><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">커널</a></td></tr><tr><td style="text-align: center;"><code>Ldr</code></td><td>Loader</td><td><a href="https://en.wikipedia.org/wiki/Portable_Executable">PE 포맷</a> 로더</td></tr><tr><td style="text-align: center;"><code>Lpc</code></td><td>Local Procedure Call</td><td><a href="https://en.wikipedia.org/wiki/Local_Inter-Process_Communication">로컬 프로시저 호출</a></td></tr><tr><td style="text-align: center;"><code>Lsa</code></td><td>Local Security Authority</td><td><a href="https://en.wikipedia.org/wiki/Local_Security_Authority_Subsystem_Service">로컬 보안 인증</a></td></tr><tr><td style="text-align: center;"><code>Mm</code></td><td>Memory manager</td><td><a href="#메모리-관리자">메모리 관리자</a></td></tr><tr><td style="text-align: center;"><code>Nt</code></td><td>Native system service (from user mode)</td><td>NT <a href="WinAPI.md#시스템-서비스">시스템 서비스</a> (사용자 모드에서 호출)</td></tr><tr><td style="text-align: center;"><code>Ob</code></td><td>Object manager</td><td><a href="#개체-관리자">개체 관리자</a></td></tr><tr><td style="text-align: center;"><code>Po</code></td><td>Power manager</td><td><a href="#전원-관리자">전원 관리자</a></td></tr><tr><td style="text-align: center;"><code>Pp</code></td><td>PnP manager</td><td><a href="#pnp-관리자">PnP 관리자</a></td></tr><tr><td style="text-align: center;"><code>Rtl</code></td><td>Run-time library</td><td><a href="https://en.wikipedia.org/wiki/Runtime_library">런타임 라이브러리</a>; 커널에 직접적으로 가담하지 않지만 네이티브 어플리케이션에서 사용할 수 있는 다양한 유틸리티 함수를 제공한다.</td></tr><tr><td style="text-align: center;"><code>Tp</code></td><td>Thread pool manager</td><td><a href="https://en.wikipedia.org/wiki/Thread_pool">스레드 풀</a> 관리자</a></td></tr><tr><td style="text-align: center;"><code>Vf</code></td><td>Driver Verifier</td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/driver-verifier">드라이버 검증 도구</a></td></tr><tr><td style="text-align: center;"><code>Vsl</code></td><td>Virtual Secure Mode library</td><td><a href="Hypervisor.md#가상-보안-모드">가상 보안 모드</a> 라이브러리</td></tr><tr><td style="text-align: center;"><code>Whea</code></td><td>Windows Hardware Error Architecture</td><td>윈도우 하드웨어 오류 아키텍처</td></tr><tr><td style="text-align: center;"><code>Wmi</code></td><td>Windows Management Instrumentation</td><td><a href="WMI.md">윈도우 관리 도구</a></td></tr><tr><td style="text-align: center;"><code>Zw</code></td><td>(n/a)</td><td>커널 모드 접근으로 미러된 <code>Nt</code> <a href="WinAPI.md#시스템-서비스">시스템 서비스</a> 진입점; 사용자 모드에서 접근할 때 진행하는 파라미터 유효성 검증을 처리하지 않는다.</td></tr></tbody></table>

<sup>_† NT 함수 접두사 중에서 `p`로 끝나거나 `i`로 대체된 건 각각 private 및 internal을 의미하는, 즉 내부 함수를 가리킨다._</sup>

### 개체 기반
[윈도우 NT](Windows.md) [운영체제](https://en.wikipedia.org/wiki/Operating_system)는 **[객체 기반](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/object-based)**(object-based)이다.

> 단, 이는 윈도우 OS가 전통적인 C++에서 언급하는 [객체 지향](https://en.wikipedia.org/wiki/Object-oriented_programming)(object-oriented)임을 의미하는 게 절대 아니다.

Executive의 다양한 구성 요소들은 한 개 이상의 개체 [자료형](C.md#사용자-정의-자료형)을 정의하고 이를 처리할 수 있는 [커널 모드](Processor.md#커널-모드) 루틴을 제공한다. 어느 한 Executive 구성도 다른 구성의 개체를 직접 접근할 수 없으므로, 원할 시 이를 지원하는 루틴을 호출해야 한다.

## 입출력 관리자
**[입출력 관리자](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/windows-kernel-mode-i-o-manager)**(I/O manager)

### 입출력 요청 패킷
**[입출력 요청 패킷](https://en.wikipedia.org/wiki/I/O_request_packet)**(I/O request packet; [IRP](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_irp))은 [WDM](Driver.md#윈도우-드라이버-모델) 및 [윈도우 NT](Windows.md) [장치 드라이버](Driver.md)가 다른 드라이버 또는 운영체제와 통신하기 위해 사용하는 [커널 모드](Processor.md#권한-수준) [구조체](C.md#구조체)이다. I/O 요청에 대한 정보들을 구조체 포인터 하나만으로 참조할 수 있으며, 즉시 처리가 불가하면 큐에 대기될 수 있다. I/O 완료는 [`IoCompleteRequest`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-iocompleterequest) 루틴에 해당 IRP 구조체의 포인터를 전달하여 입출력 관리자로 보고된다.

일반적으로 IRP는 입출력 관리자가 사용자 모드의 입출력 요청에 대응하여 생성되지만 [PnP 관리자](#pnp-관리자), [전원 관리자](#전원-관리자) 등의 다른 시스템 구성요소에 의해 생성되기도 한다. 심지어 드라이버가 생성하여 타 드라이버에게 전달될 수 있다.

## 전원 관리자

## PnP 관리자
**[PnP 관리자](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/windows-kernel-mode-plug-and-play-manager)**(PnP manager)

## 하드웨어 추상 계층
**[하드웨어 추상 계층](https://ko.wikipedia.org/wiki/하드웨어_추상화)**(Hardware Abstraction Layer; HAL)은 윈도우 NT kernel이 시스템을 구성하는 CPU, 메모리, 디스크 등의 하드웨어와 직접 통신 및 제어하기 위해 필요한 저급 인터페이스를 제공한다. 하드웨어 이식성을 구현하는 핵심 구성요소이며, hal.dll 라이브러리에 정의되어 NT 커널 이미지 ntoskrnl.exe에 로드된다.

> [윈도우 10, 버전 2004](https://en.wikipedia.org/wiki/Windows_10,_version_2004) (코드네임: 20H1) 빌드부터 HAL은 ntoskrnl.exe 안에 포함(혹은 정적 링크)되었으며, DLL은 하위호환을 위해 남겨둔 상태이다.

x86 시스템의 경우, 시스템 부팅 단계에서 [APIC](https://ko.wikipedia.org/wiki/APIC) 지원 여부에 따라 두 종류의 HAL 중 하나를 시스템에 로드한다: halacpi.dll ([ACPI](https://ko.wikipedia.org/wiki/ACPI)만 지원) 그리고 halmacpi.dll (ACPI + [SMP](https://ko.wikipedia.org/wiki/대칭형_다중_처리), 즉 APIC 지원). 반면, x64 및 ARM64 시스템은 마더보드에 ACPI와 APIC가 모두 필요하므로 hal.dll 하나만 존재한다.

마이크로소프트에서 제공하는 기본적인 HAL만으로 부족할 경우를 대비하여, 이전에는 3rd 파티 제조사가 자체적으로 HAL을 제공하는 방안을 고려하였지만 현실적이지 않다고 판단하였다. 시스템이 필요에 따라 언제든지 로드가 자유로운 DLL 파일로 제작된 HAL extension을 도입하였으며(예를 들어 HalExtPL080.dll, HalExtIntcLpioDMA.dll 등), 이를 개발하기 위해서는 반드시 마이크로소프트의 협업이 필요하다.

## 개체 관리자
**[개체 관리자](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/windows-kernel-mode-object-manager)**(object manager)는 [커널 개체](#커널-개체)를 관리하는 [커널](#nt-커널)의 구성 요소이다. 커널 개체의 (생성 및 파괴를 포함한) 생명주기 및 연관 정보를 관리 및 추적하는 게 핵심 역할이다. 

### 커널 개체
**[커널 개체](https://learn.microsoft.com/en-us/windows/win32/sysinfo/kernel-objects)**(kernel object)는 NT 커널 Executive의 다양한 서브시스템이 다루는 리소스 유형들을 접근할 수 있도록 [개체 관리자](#개체-관리자)에서 관리하는 정적 [구조체](C.md#구조체)의 런타임 [인스턴스](C.md#사용자-정의-자료형)이다. [프로세스](Process.md) 및 [스레드](Thread.md) 등을 포함한 모든 리소스가 생성되면 대응하는 커널 개체를 함께 생성된다. [사용자 모드](Processor.md#사용자-모드)에서 커널 개체를 사용하려면 [핸들](Process.md#핸들)을 통해 간접적으로 참조해야 한다.

* 커널 개체는 두 가지 신호 상태가 있으며, *signaled* 그리고 *nonsignaled*로 나뉘어진다. 각 객체 유형마다 신호 상태의 토글 조건이 이미 정해져 있다. 예를 들어, [프로세스](Process.md) 커널 개체는 nonsignaled 상태로 생성되고 종료될 때 signaled 상태로 변경된다.

### 참조 카운트
개체 관리자는 커널 개체가 생성될 때 **[참조 카운트](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/life-cycle-of-an-object#object-reference-count)**(혹은 **[포인터](C.md#포인터) 카운트**)를 1로 설정하며, 해당 카운트가 0으로 감소하면 더 이상 참조되지 않는 걸로 간주하여 제거한다. 개체 관리자에서 관리하는 참조 카운트가 실제로 참조한 개수와 일치하도록 신경써야 하며, 그렇지 않을 시 다음 문제가 발생한다.

* 커널 개체를 실제로 참조한 개수 > 참조 카운트: [Bugcheck](BSOD.md#버그-검사-코드) [0x18 REFERENCE_BY_POINTER](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/bug-check-0x18--reference-by-pointer) 등의 사유로 시스템이 [충돌](BSOD.md)할 수 있다.
* 커널 개체를 실제로 참조한 개수 < 참조 카운트: 커널 관리자는 개체가 아직 참조되었다고 판단하여 해제하지 않아 [메모리 누수](https://en.wikipedia.org/wiki/Memory_leak)를 야기한다.

참조 카운트 외에 [핸들 카운트](Process.md#핸들)도 함께 집계되며, 이 둘의 차이는 다음과 같다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">참조 및 핸들 카운트 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">참조 카운트</th><th style="text-align: center;">핸들 카운트</th></tr></thead><tbody><tr><td><a href="C.md#포인터">포인터</a>를 통해 커널 개체를 참조한 개수를 집계한다.<ul><li>참조 루틴: <a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-obreferenceobjectbypointer">ObReferenceObjectByPointer</a> 등</li><ul></td><td><a href="Process.md#핸들">핸들</a>을 통해 커널 개체를 참조한 개수를 집계한다. 단, 핸들의 특성상 참조 카운트도 함께 증감시킨다.<ul><li>참조 루틴: <a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-obreferenceobjectbyhandle">ObReferenceObjectByHandle</a> 등</li><ul></td></tr></tbody></table>
