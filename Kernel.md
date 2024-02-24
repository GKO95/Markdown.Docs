# 커널
[커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅))(kernel), 일명 슈퍼바이저(supervisor)는 [운영체제](https://ko.wikipedia.org/wiki/운영체제)의 핵심 프로그램으로 일반적으로 시스템 전체에 대한 제어권을 가진다. [메모리](Memory.md)에 항상 상주하여 하드웨어와 소프트웨어 간 상호작용을 가능케 하여, 운영체제의 아래 역할을 담당한다.

* [장치 드라이버](Driver.md)를 통한 하드웨어 리소스 제어: 메모리, 입출력, 암호화 등
* [프로세스](Process.md) 간 위의 하드웨어 리소스 할당에 대한 갈등 중재
* 공통 리소스 활용의 최적화: [CPU](Processor.md) 및 캐시 사용률, 파일 시스템, 네트워크 소켓 등

## 아키텍처
운영체제 설계에 따라 커널은 크게 세 가지의 아키텍처로 설계될 수 있다.

![커널 설계에 따른 아키텍처](https://upload.wikimedia.org/wikipedia/commons/d/d0/OS-structure2.svg)

본 부문에서 자주 인용될 "서비스"란, 호출 가능한 커널 루틴 혹은 그 집합을 의미한다.

### 모놀리식 커널
[모놀리식 커널](https://ko.wikipedia.org/wiki/모놀리식_커널)(monolithic kernel)은 모든 서비스가 단일 프로그램으로 빌드되어 [커널 공간](Processor.md#권한-수준)에서 처리하는 구조이다. 단조로운 구조에 관리가 매우 편하고, 단일 프로그램에서 모든 커널 작업 및 서비스가 수행되니 성능 속도가 매우 빠르다. 단, 커널 공간의 특성상 사소한 오류가 시스템 전체에 영향을 줄 수 있는 위험이 항상 존재한다. 운영체제 런타임 도중에 장치 드라이버를 언제든지 불러올 수 있는 모듈성(modularity)을 지원한다.

마이크로소프트의 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 및 [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x) 시리즈가 모놀리식 커널를 사용하였다.

### 마이크로커널
[마이크로커널](https://ko.wikipedia.org/wiki/마이크로커널)(microkernel)은 운영체제 구동에서 기초적이지만 필연적인 저급 커널 서비스만을 제외한 나머지를 [사용자 공간](Processor.md#권한-수준)으로 분리시킨 구조이다: [스케줄링](Processor.md#스케줄링), 메모리 관리, 기초적인 [IPC](Process.md#프로세스-간-통신)가 최소한으로 마련되어야 할 서비스이다. 한편, 사용자 모드에서 고급 커널 서비스를 제공하는 프로그램들을 서버(server)라고 부른다.

> 마이크로커널의 기초적인 IPC 서비스는 사용자 모드에 위치한 서버 혹은 장치 드라이버 간 통신을 위해 반드시 필요한 기능이다.

모놀리식 커널의 단점인 방대한 커널 이미지로 인한 코드 관리의 어려움 및 장치 드라이버 충돌에 의한 커널 전체에 미치는 악영향을 해소하고자 고안되었다. 커널 서비스의 서버화는 커널 개발 및 업데이트 시간을 단축시키지만, 사용자 모드에 위치하여 하드웨어 접근성 효율이 떨어진다. IPC 통신으로 인한 성능 저하 및 커널 동작 절차의 복잡성도 단점으로 함께 지적되었다.

### 하이브리드 커널
[하이브리드 커널](https://ko.wikipedia.org/wiki/하이브리드_커널)(hybrid kernel)은 마이크로커널처럼 서비스에 따라 서버가 나뉘어져 있으나, 모놀리식 커널처럼 전부 (혹은 대부분) 커널 공간에 존재한다. 결국 시스템 안전성 취약점은 여전히 존재하지만, 마이크로커널에 비해 사용자 및 커널 모드 전환이 불필요하여 커널 서비스의 성능 [오버헤드](https://ko.wikipedia.org/wiki/오버헤드)가 없다.

마이크로소프트의 [윈도우 NT](Windows.md)가 하이브리드 커널의 영향을 받은 대표적인 운영체제이다.

# NT 커널
[윈도우 NT](Windows.md) 운영체제의 커널 이미지 ntoskrnl.exe는 아래와 같이 구성된다.

<table style="width: 95%; margin-left: auto; margin-right: auto;">
<caption style="caption-side: top;">윈도우 NT 커널 이미지의 구성</caption>
<colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col/><col/><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead><tr><th style="text-align: center;">이미지</th><th style="text-align: center;">계층</th><th colspan="10" style="text-align: center;">구성</th></tr></thead>
<tbody><tr><td rowspan="3" style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/Ntoskrnl.exe">ntoskrnl.exe</a><br/>(모듈명: <code>nt</code>)</td><td rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</td><td colspan="10" style="text-align: center;">시스템 서비스 담당자</td></tr><tr><td style="text-align: center;"><a href="#입출력-관리자">입출력 관리자</a></td><td style="text-align: center;">캐시 관리자</td><td style="text-align: center;">보안 참조 모니터</td><td style="text-align: center;">전력 관리자</td><td style="text-align: center;"><a href="#pnp-관리자">PnP 관리자</a></td><td style="text-align: center;">메모리 관리자</td><td style="text-align: center;">프로세스 관리자</td><td style="text-align: center;">객체 관리자</td><td style="text-align: center;">구성 관리자</td><td style="text-align: center;">ALPC</td></tr>
<tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">Kernel</a></td><td colspan="10" style="text-align: center;">스케줄링, 동기화, 인터럽트 등 기초적인 핵심 함수 제공</td></tr></tbody>
</table>

Executive는 특정 작업을 수행하는 여러 구성원들로 이루어진 ntoskrnl.exe의 상위 계층이다. 한편, Kernel 계층은 모듈에서 필요로 하는 기초적인 커널 함수들을 제공하는 하위 계층이며 [마이크로커널](#마이크로커널)의 역할을 담당한다. 이러한 구조의 정립으로 Kernel은 단순히 OS 매커니즘을 구현하고, Executive는 이를 활용하여 실질적인 정책 결정에 기여한다.

아래는 `nt` 모듈의 함수 접두사가 각각 어떤 목적으로 사용되는 지 식별하는 도표이다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">NT 함수 접두사</caption><colgroup><col style="width: 10%;"/><col style="width: 40%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">접두사</th><th style="text-align: center;">영문</th><th style="text-align: center;">의미</th></tr></thead><tbody><tr><td style="text-align: center;"><code>Alpc</code></td><td>Asynchronous Local Procedure Call</td><td><a href="https://ko.wikipedia.org/wiki/로컬_프로시저_호출">비동기 로컬 프로시저 호출</a></td></tr><tr><td style="text-align: center;"><code>Cc</code></td><td>Common cache</td><td>공통 캐시</td></tr><tr><td style="text-align: center;"><code>Cm</code></td><td>Configuration manager</td><td><a href="#구성-관리자">구성 관리자</a></td></tr><tr><td style="text-align: center;"><code>Csr</code></td><td>Client/server runtime</td><td><a href="Subsystem.md#윈도우-서브시스템">Csrss.exe</a> 서브시스템 프로세스</td></tr><tr><td style="text-align: center;"><code>Dbg</code></td><td>Kernel debug support</td><td>커널 디버그 지원</td></tr><tr><td style="text-align: center;"><code>Dbgk</code></td><td>Debugging Framework for user mode</td><td>사용자 모드 디버깅 프레임워크</td></tr><tr><td style="text-align: center;"><code>Em</code></td><td>Errata manager</td><td>에라타 관리자</td></tr><tr><td style="text-align: center;"><code>Etw</code></td><td>Event Tracing for Windows</td><td>윈도우 이벤트 추적</td></tr><tr><td style="text-align: center;"><code>Ex</code></td><td>Executive support routines</td><td><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</a> 지원 루틴</td></tr><tr><td style="text-align: center;"><code>FsRtl</code></td><td>File System Runtime Library</td><td>파일 시스템 런타임 라이브러리</td></tr><tr><td style="text-align: center;"><code>Hv</code></td><td>Hive Library</td><td>하이브 라이브러리</td></tr><tr><td style="text-align: center;"><code>Hvl</code></td><td>Hypervisor Library</td><td><a href="Hypervisor.md">하이퍼바이저</a> 라이브러리</td></tr><tr><td style="text-align: center;"><code>Hal</code></td><td>Hardware Abstraction Layer</td><td><a href="#하드웨어-추상-계층">하드웨어 추상 계층</a></td></tr><tr><td style="text-align: center;"><code>Io</code></td><td>I/O manager</td><td><a href="#입출력-관리자">입출력 관리자</a></td></tr><tr><td style="text-align: center;"><code>Kd</code></td><td>Kernel Debugger</td><td>커널 디버거</td></tr><tr><td style="text-align: center;"><code>Ke</code></td><td>Kernel</td><td><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">커널</a></td></tr><tr><td style="text-align: center;"><code>Ks</code></td><td>Kernel streaming</td><td>커널 스트리밍</td></tr><tr><td style="text-align: center;"><code>Kse</code></td><td>Kernel Shim Engine</td><td>커널 <a href="https://en.wikipedia.org/wiki/Shim_(computing)">심</a> 엔진</td></tr><tr><td style="text-align: center;"><code>Kx</code></td><td>(n/a)</td><td>인터럽트 처리, 세마포어, 스핀락, 멀티스레딩 및 컨텍스트 교환 함수</td></tr><tr><td style="text-align: center;"><code>Ky</code></td><td>(n/a)</td><td>트랩 프레임을 생성하고 <code>Kx</code> 접두함수를 호출하는 내부 및 부분 함수</td></tr><tr><td style="text-align: center;"><code>Ldr</code></td><td>Loader</td><td><a href="https://ko.wikipedia.org/wiki/PE_포맷">PE 포맷</a> 로더</td></tr><tr><td style="text-align: center;"><code>Lpc</code></td><td>Local Procedure Call</td><td><a href="https://ko.wikipedia.org/wiki/로컬_프로시저_호출">로컬 프로시저 호출</a></td></tr><tr><td style="text-align: center;"><code>Lsa</code></td><td>Local Security Authority</td><td><a href="https://ko.wikipedia.org/wiki/로컬_보안_인증_하위_시스템_서비스">로컬 보안 인증</a></td></tr><tr><td style="text-align: center;"><code>Mm</code></td><td>Memory manager</td><td><a href="#메모리-관리자">메모리 관리자</a></td></tr><tr><td style="text-align: center;"><code>Nt</code></td><td>Native system service (from user mode)</td><td>NT <a href="WinAPI.md#시스템-서비스">시스템 서비스</a> (사용자 모드에서 호출)</td></tr><tr><td style="text-align: center;"><code>Ob</code></td><td>Object manager</td><td><a href="#객체-관리자">객체 관리자</a></td></tr><tr><td style="text-align: center;"><code>Pf</code></td><td>Prefetcher</td><td><a href="https://en.wikipedia.org/wiki/Prefetcher">프리페처</a></td></tr><tr><td style="text-align: center;"><code>Po</code></td><td>Power manager</td><td><a href="#전원-관리자">전원 관리자</a></td></tr><tr><td style="text-align: center;"><code>PoFx</code></td><td>Power framework</td><td>전원 프레임워크</td></tr><tr><td style="text-align: center;"><code>Pp</code></td><td>PnP manager</td><td><a href="#pnp-관리자">PnP 관리자</a></td></tr><tr><td style="text-align: center;"><code>Ppm</code></td><td>Processor power manager</td><td><a href="Processor.md">프로세서</a> 전원 관리자</td></tr><tr><td style="text-align: center;"><code>Ps</code></td><td>Process support</td><td><a href="Process.md">프로세스</a> 지원</td></tr><tr><td style="text-align: center;"><code>Rtl</code></td><td>Run-time library</td><td><a href="https://ko.wikipedia.org/wiki/런타임_라이브러리">런타임 라이브러리</a>; 커널에 직접적으로 가담하지 않지만 네이티브 어플리케이션에서 사용할 수 있는 다양한 유틸리티 함수를 제공한다.</td></tr><tr><td style="text-align: center;"><code>Se</code></td><td>Security Reference Monitor</td><td>보안 참조 모니터</td></tr><tr><td style="text-align: center;"><code>Sm</code></td><td>Store Manager</td><td>스토어 관리자</td></tr><tr><td style="text-align: center;"><code>Tm</code></td><td>Transaction Manager</td><td><a href="https://ko.wikipedia.org/wiki/커널_트랜잭션_관리자">트랜잭션 관리자</a></td></tr><tr><td style="text-align: center;"><code>Tp</code></td><td>Thread pool manager</td><td><a href="https://en.wikipedia.org/wiki/Thread_pool">스레드 풀</a> 관리자</a></td></tr><tr><td style="text-align: center;"><code>Ttm</code></td><td>Terminal timeout manager</td><td>터미널 타임아웃 관리자</td></tr><tr><td style="text-align: center;"><code>Vf</code></td><td>Driver Verifier</td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/driver-verifier">드라이버 검증 도구</a></td></tr><tr><td style="text-align: center;"><code>Vsl</code></td><td>Virtual Secure Mode library</td><td><a href="Hypervisor.md#가상-보안-모드">가상 보안 모드</a> 라이브러리</td></tr><tr><td style="text-align: center;"><code>Wdi</code></td><td>Windows Diagnostic Infrastructure</td><td>윈도우 진단 인프라구조</td></tr><tr><td style="text-align: center;"><code>Wfp</code></td><td>Windows FingerPrint</td><td>윈도우 지문</td></tr><tr><td style="text-align: center;"><code>Whea</code></td><td>Windows Hardware Error Architecture</td><td>윈도우 하드웨어 오류 아키텍처</td></tr><tr><td style="text-align: center;"><code>Wmi</code></td><td>Windows Management Instrumentation</td><td><a href="WMI.md">윈도우 관리 도구</a></td></tr><tr><td style="text-align: center;"><code>Zw</code></td><td>(n/a)</td><td>커널 모드 접근으로 미러된 <code>Nt</code> <a href="WinAPI.md#시스템-서비스">시스템 서비스</a> 진입점; 사용자 모드에서 접근할 때 진행하는 파라미터 유효성 검증을 처리하지 않는다.</td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><i><sub>† NT 함수 접두사 중에서 <code>p</code>로 끝나거나 <code>i</code>로 대체된 건 각각 private 및 internal을 의미하는, 즉 내부 함수를 가리킨다.</sub></i></caption></table>

## 입출력 관리자

### 입출력 요청 패킷
[입출력 요청 패킷](https://en.wikipedia.org/wiki/I/O_request_packet)(I/O request packet; [IRP](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_irp))은 [WDM](Driver.md#윈도우-드라이버-모델) 및 [윈도우 NT](Windows.md) [장치 드라이버](Driver.md)가 다른 드라이버 또는 운영체제와 통신하기 위해 사용하는 [커널 모드](Processor.md#권한-수준) [구조체](C.md#구조체)이다. I/O 요청에 대한 정보들을 구조체 포인터 하나만으로 참조할 수 있으며, 즉시 처리가 불가하면 큐에 대기될 수 있다. I/O 완료는 [`IoCompleteRequest`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-iocompleterequest) 루틴에 해당 IRP 구조체의 포인터를 전달하여 입출력 관리자로 보고된다.

일반적으로 IRP는 입출력 관리자가 사용자 모드의 입출력 요청에 대응하여 생성되지만 [PnP 관리자](#pnp-관리자), [전원 관리자](#전원-관리자) 등의 다른 시스템 구성요소에 의해 생성되기도 한다. 심지어 드라이버가 생성하여 타 드라이버에게 전달될 수 있다.

## PnP 관리자

## 하드웨어 추상 계층
[하드웨어 추상 계층](https://ko.wikipedia.org/wiki/하드웨어_추상화)(Hardware Abstraction Layer; HAL)은 윈도우 NT kernel이 시스템을 구성하는 CPU, 메모리, 디스크 등의 하드웨어와 직접 통신 및 제어하기 위해 필요한 저급 인터페이스를 제공한다. 하드웨어 이식성을 구현하는 핵심 구성요소이며, hal.dll 라이브러리에 정의되어 NT 커널 이미지 ntoskrnl.exe에 로드된다.

> [윈도우 10, 버전 2004](https://en.wikipedia.org/wiki/Windows_10,_version_2004) (코드네임: 20H1) 빌드부터 HAL은 ntoskrnl.exe 안에 포함(혹은 정적 링크)되었으며, DLL은 하위호환을 위해 남겨둔 상태이다.

x86 시스템의 경우, 시스템 부팅 단계에서 [APIC](https://ko.wikipedia.org/wiki/APIC) 지원 여부에 따라 두 종류의 HAL 중 하나를 시스템에 로드한다: halacpi.dll ([ACPI](https://ko.wikipedia.org/wiki/ACPI)만 지원) 그리고 halmacpi.dll (ACPI + [SMP](https://ko.wikipedia.org/wiki/대칭형_다중_처리), 즉 APIC 지원). 반면, x64 및 ARM64 시스템은 마더보드에 ACPI와 APIC가 모두 필요하므로 hal.dll 하나만 존재한다.

마이크로소프트에서 제공하는 기본적인 HAL만으로 부족할 경우를 대비하여, 이전에는 3rd 파티 제조사가 자체적으로 HAL을 제공하는 방안을 고려하였지만 현실적이지 않다고 판단하였다. 시스템이 필요에 따라 언제든지 로드가 자유로운 DLL 파일로 제작된 HAL extension을 도입하였으며(예를 들어 HalExtPL080.dll, HalExtIntcLpioDMA.dll 등), 이를 개발하기 위해서는 반드시 마이크로소프트의 협업이 필요하다.
