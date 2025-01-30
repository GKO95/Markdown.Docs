# 메모리
**[메모리](https://en.wikipedia.org/wiki/Computer_memory)**(memory)는 시스템에서 즉각적으로 사용할 데이터를 저장하는 공간이다. 흔히 [주기억장치](https://en.wikipedia.org/wiki/Computer_data_storage#Primary_storage)의 대표격인 [RAM](https://en.wikipedia.org/wiki/Random-access_memory)을 언급하는 경우가 대다수이며, 작업 속도가 매우 빠르고 [휘발성](https://en.wikipedia.org/wiki/Volatile_memory)인 특징을 가지고 있다. 또한 [프로세서](Processor.md)와 물리적으로 근접하여 연산시 데이터 접근 속도가 순식간이기 때문에 단기기억 역할을 담당한다. 그러므로 메모리는 시스템의 성능을 결정하는 중요한 요소로 작용한다.

본문에서 사용되는 메모리 용어들을 다음과 같이 정리한다.

* **[가상 메모리](#가상-메모리)**: *(하드웨어와 독립적으로) [운영체제](https://en.wikipedia.org/wiki/Operating_system)로부터 표현된 메모리이며, 프로세서에 의해 처리될 수 있으려면 "물리 메모리"가 반드시 필요하다.*
    * **물리 메모리**: *가상 메모리를 담을 수 있는 하드웨어 실체가 있는 메모리이며, [RAM](https://en.wikipedia.org/wiki/Random-access_memory) 및 [페이징 파일](#페이징-파일)이 해당한다.*
* **[보조 메모리](Storage.md)**: *데이터를 오래 저장할 수 있는 [비휘발성](https://en.wikipedia.org/wiki/Non-volatile_random-access_memory) 메모리이며, [HDD](Storage.md#디스크) 및 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브) 등의 저장 장치가 해당한다.*

각 메모리가 의미하는 바가 무엇인지 이해를 돕기 위해 다음 8 GB RAM이 설치된 시스템의 [작업 관리자](TaskMgr.md)를 예시로 설명한다.

![작업 관리자에서 확인한 메모리 성능 (<a href="Windows.md">윈도우 11</a>, 버전 22H2)](./images/memory_taskmgr.png)

설치된 RAM 중에서 4.2 GB 메모리를 사용하며, 아래와 같이 수치를 살펴볼 수 있다. 자세한 내용을 이해하려면 하기 부문들을 읽기를 적극 권장한다.

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">작업 관리자의 메모리 구성원 소개</caption><colgroup><col style="width: 15%;"/><col style="width: 12%;"/><col style="width: 8%;"/><col/></colgroup><thead><tr><th style="text-align: center;">메모리 구성</th><th style="text-align: center;">그룹</th><th style="text-align: center;">유형</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><b><a href="#작업-집합">사용 중</a></b><br/>(1.2 GB)</td><td style="text-align: center;">-</td><td style="text-align: center;">-</td><td><a href="Kernel.md">커널</a> 및 <a href="Driver.md">드라이버</a>, <a href="Process.md#프로세스">프로세스</a>을 포함한 시스템 전반에서 사용 중인 RAM 크기이다.<ul><li>사용 중인 메모리 내에서도 공간 절약이 가능하여 <a href="https://en.wikipedia.org/wiki/Virtual_memory_compression">압축된 메모리</a><sup>†</sup>는 151 MB이다.</li></ul></td></tr><tr><td rowspan="3" style="text-align: center;"><b>사용 가능</b><br/>(2.4 GB)</td><td rowspan="2" style="text-align: center;"><i>여유</i><br/>(448 MB)</td><td style="text-align: center;">영값</td><td>메모리가 전부 영값으로 비워져 (혹은 채워져) 아무런 데이터가 없어 곧장 사용될 수 있다.<ul><li>오로지 영값으로 정리된 페이지만 타 프로그램에서 확보하여 사용할 수 있다.</li></ul></td></tr><tr><td style="text-align: center;">해제</td><td>프로그램이 종료되면서 함께 해제된 메모리이다. 당시 데이터가 잔여하기 때문에 차후 시스템 스레드에 의해 영값으로 정리된다.<ul><li>영값 페이지가 고갈될 시 대신 제공하지만, 데이터를 정리하는 데 시간이 다소 소모된다.</li></ul></td></tr><tr><td rowspan="2" style="text-align: center;"><i><a href="https://en.wikipedia.org/wiki/Cache_(computing)">캐시</a></i><br/>(2.5 GB)</td><td style="text-align: center;">대기</td><td>프로세스의 가상 메모리와 매핑되었을 당시 데이터가 잔여하여, 만일 프로세스가 해당 페이지를 다시 필요할 경우 단순 매핑만으로 곧바로 사용할 수 있다.</td></tr><tr><td style="text-align: center;"><b>기타</b><br/>(544 MB)</td><td style="text-align: center;">수정</td><td>프로세스의 가상 메모리와 매핑되었을 당시 데이터를 먼저 디스크에 저장을 완료한 다음에 대기 페이지로 전환되기 때문에 <i>사용 중</i> 또는 <i>사용 가능</i> 메모리에 속하지 않는다.</td></tr></tbody></table>

<sup>_† [윈도우 10](https://en.wikipedia.org/wiki/Windows_10)의 [참가자 빌드 10525](https://blogs.windows.com/windows-insider/2015/08/18/announcing-windows-10-insider-preview-build-10525/)부터 소개되어, [작업 집합](#작업-집합)의 가상 메모리를 [디스크](Storage.md)로 [페이징](#페이징-파일)하기 전에 앞서 [페이지](#페이지)를 압축하여 가능한 RAM에 상주하는 기법을 꾀하기 위해 도입된 "메모리 압축(Memory Compression)" [프로세스](Process.md)이다. 해당 프로세스의 작업 집합은 작업 관리자의 메모리 성능에 표시된 '(압축)' 크기와 일치한다._</sup>

## 메모리 관리 장치
> *참고: [Converting Virtual Addresses to Physical Addresses - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/converting-virtual-addresses-to-physical-addresses)*

**[메모리 관리 장치](https://en.wikipedia.org/wiki/Memory_management_unit)**(memory management unit; MMU)는 [프로세서](Processor.md)에 내장되어 있는 반도체로 가상 메모리에서 RAM의 물리 메모리로 주소를 변환하는 역할을 전담한다. 메모리 주소를 변환하는 절차는 결국 프로세서 아키텍처에 의해 정의되며, 변환 과정에 대한 상세한 내용은 *[페이지 테이블](#페이지-테이블)* 장을 참고한다.

# 가상 메모리
> *참고: [Pushing the Limits of Windows: Virtual Memory | Microsoft Learn](https://learn.microsoft.com/en-us/archive/blogs/markrussinovich/pushing-the-limits-of-windows-virtual-memory)*

**[가상 메모리](https://en.wikipedia.org/wiki/Virtual_memory)**(virtual memory)는 일종의 [메모리 관리](https://en.wikipedia.org/wiki/Memory_management_(operating_systems)) 기술이며, 하드웨어 매체의 실체와 무관하게 [운영체제](https://en.wikipedia.org/wiki/Operating_system)에 의해 표현된 "가상"의 메모리이다. ([커널](Kernel.md)을 포함한) 모든 프로그램들은 가상 메모리에서 실행되며, 각 프로세스마다 주어진 [가상 주소 공간](Process.md#가상-주소-공간)에 메모리를 할당받는다. 가상 주소 공간에 할당된 가상 메모리의 주소는 프로세서에 내장된 [MMU](#메모리-관리-장치)에 의해 RAM의 물리 메모리 주소로 변환되어 접근된다.

가상 메모리를 활용한 시스템은 다음과 같은 이점을 지닌다:

1. 메모리 체계(예를 들어, 공유 메모리 등)를 커널에서 관리하기 때문에 프로그램 개발의 편리
1. [페이징](#페이징-파일) 기술을 통해 기존 RAM의 물리적 제약보다 더 많은 주소 공간 확보
1. 메모리 격리에 의한 시스템 보안 강화

## 페이지
**[페이지](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page)는 운영체제에서 관리하는 가장 작은 단위의 가상 메모리 조각이다 ([x86](https://en.wikipedia.org/wiki/X86) [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)의 경우 4 [KB](https://en.wikipedia.org/wiki/Kilobyte)). 페이지와 일대일 매핑된 RAM의 주소 영역을 **[페이지 프레임](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page frame; 일명 프레임)이라고 부른다. 페이지는 가상 메모리의 가용 여부를 결정하는 세 가지 [상태](https://learn.microsoft.com/en-us/windows/win32/memory/page-state)로 분류된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">가상 메모리의 페이지 상태</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">상태</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">여유<br/>(Free)</td><td>사용되고 있지 않는 가상 주소 공간의 페이지이다.</td></tr><tr><td style="text-align: center;">예약됨<br/>(Reserved)</td><td>가상 메모리로 사용 중이지만, 아직 물리 메모리가 할당되지 않은 가상 주소 공간의 페이지이다. 즉, 예약된 페이지는 시스템의 메모리 성능에 기여를 하지 않는다.<ul><li><i>가상 메모리는 할당 입도(allocation granularity) 경계의 첫 주소를 시작으로 페이지를 예약받는다.</i></li></ul></td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/introduction-to-the-page-file#system-committed-memory">커밋됨</a><br/>(Committed)</td><td>가상 메모리로 사용 중이면서 물리 메모리가 할당된 가상 주소 공간의 페이지이다. 커밋된 페이지는 시스템의 메모리 성능에 실질적으로 기여한다. 다만, 예약된 페이지 영역 전체를 할당하지 않아도 된다.<ul><li><i>커밋된 페이지에 최초의 R/W 시도가 있기 전까지 할당된 물리 메모리에 매핑되지 않는다.</i></li></ul></td></tr></tbody></table>

<sup>_† [x86](https://en.wikipedia.org/wiki/X86) [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)는 페이지 크기를 4 KB (0000'1000h), 그리고 할당 입도 경계 간격을 64 KB (0001'0000h)로 구분한다. 반면, 단종된 [IA-64](https://en.wikipedia.org/wiki/Itanium) 아키텍처는 페이지 크기를 8 KB로 정의한다._</sup>

커밋될 수 있는 최대 페이지 크기, 즉 커밋 한도(commit limit)는 결과적으로 물리 메모리의 용량에 의존한다. 다시 말해, 비록 가상 메모리일지라도 RAM과 [페이징 파일](#페이징-파일)이 제공하는 하드웨어 용량이 부족하면 추가 메모리 할당은 불가하다.

다음은 페이지 읽기, 쓰기, 그리고 실행 가능 여부를 결정하는 보호 속성들을 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">가상 메모리의 페이지 보호 속성</caption><colgroup><col style="width: 30%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 40%;"/></colgroup><thead><tr><th style="text-align: center;">보호 속성</th><th style="text-align: center;">읽기</th><th style="text-align: center;">쓰기</th><th style="text-align: center;">실행</th><th style="text-align: center;">부연 설명</th></tr></thead><tbody><tr><td><code>PAGE_NOACCESS</code></td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td>어떠한 접근 행위라도 <a href="https://en.wikipedia.org/wiki/Segmentation_fault">예외</a>를 일으킨다.</td></tr><tr><td><code>PAGE_READONLY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td rowspan="4">-</td></tr><tr><td><code>PAGE_READWRITE</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td></tr><tr><td><code>PAGE_EXECUTE</code></td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td style="text-align: center;">✔️</td></tr><tr><td><code>PAGE_EXECUTE_READ</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td style="text-align: center;">✔️</td></tr><tr><td><code>PAGE_EXECUTE_READWRITE</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td>어떠한 접근 행위라도 <a href="https://en.wikipedia.org/wiki/Segmentation_fault">예외</a>를 일으키지 않는다.</td></tr><tr><td><code>PAGE_WRITECOPY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td rowspan="2"><a href="https://en.wikipedia.org/wiki/Copy-on-write">Copy-on-write</a>로 페이지 데이터를 변경할 시 <a href="Process.md">프로세스</a> 전용 메모리에 페이지 복사본을 제공한다.</td></tr><tr><td><code>PAGE_EXECUTE_WRITECOPY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td></tr></tbody></table>

<sup>_† 참고: [Memory Protection Constants (WinNT.h) - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/memory/memory-protection-constants)_</sup>

> [**Copy-on-write**](https://learn.microsoft.com/en-us/windows/win32/memory/memory-protection) 보호는 공유 리소스를 관리하는 기법 중 하나이다. 어느 한 프로세스가 공유 리소스를 편집하려고 시도할 경우, 시스템은 해당 프로세스의 [VAS](Process.md#가상-주소-공간)에 한하여 페이지 복사본을 제공하고 변경 사항을 적용한다. 나머지 프로세스들은 아무런 영향을 받지 않으므로 메모리도 절약할 수 있다.

Windows OS는 [데이터 실행 방지](https://learn.microsoft.com/en-us/windows/win32/memory/data-execution-prevention)(Data Execution Prevention; DEP)를 활성화할 경우, 코드 실행이 의도된 메모리 영역에 `PAGE_EXECUTE_*` 보호 속성을 적용하는 등 멀웨어로부터 유해한 코드나 데이터를 삽입하는 행위를 방지한다.

### 페이징 파일
**[페이징 파일](https://learn.microsoft.com/en-us/windows/client-management/introduction-page-file)**(paging file)은 [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)나 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)와 같은 [저장 장치](Storage.md)에 상주하는 pagefile.sys 파일이며, RAM을 보조하는 일종의 물리 메모리이다. 하드웨어 기술 향상으로 옛 컴퓨터에 비해 RAM의 용량은 대폭 증가하였으나, 운영체제는 여전히 다음 페이징 파일 기능을 활발히 사용한다:

1. RAM에 여유 공간 확보가 필요하면 사용 빈도가 가장 낮은 [작업 집합](#작업-집합)을 페이징 파일로 이동시킨다.
1. [블루스크린](BSOD.md)이 발생하면 [메모리 덤프](Dump.md#커널-모드-덤프)를 ([파일 시스템](FileSystem.md) 거치지 않고) 직접 페이징 파일로 수집한다.

RAM과 페이징 파일 간 데이터가 이동하는 [페이징](https://en.wikipedia.org/wiki/Memory_paging) 기법에 대하여 간랸히 설명한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">페이징 기법 및 설명</caption><colgroup><col style="width: 15%;"/><col style="width: 25%;"/><col style="width: 60%;"/></colgroup><thead><tr><th style="text-align: center;">페이징</th><th style="text-align: center;">전달 방향</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">페이징 아웃<br/>(Paging out)</td><td style="text-align: center;">RAM → 페이징 파일</td><td>오랜 시간 RAM에서 사용되지 않은 가상 메모리의 페이지를 페이징 파일로 옮겨 메모리 여유를 확보한다.</td></tr><tr><td style="text-align: center;">페이징 인<br/>(Paging in)</td><td style="text-align: center;">페이징 파일 → RAM</td><td>페이징 파일은 메모리 주소가 없는 관계로, CPU가 접근해야 할 가상 메모리 페이지는 RAM으로 전달된다.</td></tr></tbody></table>

페이징 파일 크기 및 구성 설정은 아래 두 방법 중 하나로 이동하여 변경할 수 있다:

![가상 메모리 다이얼로그 창](./images/memory_pagefile.png)

* Systempropertiesadvanced.exe(고급 시스템 설정 보기)의 성능 옵션 중 *가상 메모리* 변경 설정
* [레지스트리 편집기](Registry.md#레지스트리)에서 HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management 하위 키의 값 설정

여기서 "시스템이 관리하는 크기(System managed size)"란, 몇 가지의 요인들을 기반하여 [세션 관리자](Process.md#세션-관리자)가 실시간으로 적합한 페이징 파일 크기를 유연하게 결정하는 옵션이다. 만일 [커밋된 메모리](#페이지)가 커밋 한도의 90%에 도달하면 페이징 파일은 두 조건에 따라 확장한다: (1) RAM 용량 혹은 4 GB 중 가장 큰 크기의 세 배까지 늘어날 수 있지만, (2) 상주하는 드라이브 용량의 1/8 크기를 초과할 수 없다.

윈도우 OS는 기본적으로 "모든 드라이브에 대한 페이징 파일 크기 자동 관리"하도록 설정되었다; Windows가 설치된 C 드라이브만 *시스템이 관리하는 크기*이고 나머지는 *페이징 파일 없음*과 동일하다.

### 페이지 부재
**[페이지 부재](https://en.wikipedia.org/wiki/Page_fault)**(page fault)는 접근하려는 가상 메모리의 페이지가 RAM에 상주하지 않아 물리 메모리 주소로 찾아갈 수 없을 시 발생하는 [예외](C.md#예외-처리)이다. 다만, 운영체제의 메모리 관리 과정에서 흔히 일어나는 매우 자연스러운 현상이기 때문에 일반적으로는 문제로 간주되지 않는다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="text-align: center;">페이지 부재 유형</capation><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Page_fault#Minor">소프트 페이지 부재</a>(soft page fault)</th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Page_fault#Major">하드 페이지 부재</a>(hard page fault)</th></tr></thead><tbody><tr><td>접근하려는 페이지가 아직 RAM에 상주하여 상대적으로 훨씬 빨리 해결될 수 있다.</td><td>접근하려는 페이지가 RAM에 존재하지 않아 페이징 파일로부터 불러와야 한다.</td></tr></tbody></table>

그러나 RAM 용량에 비해 너무 많은 페이지가 커밋되었을 경우, [페이징](#페이징-파일) 및 페이지 부재가 과도하게 발생하는 [스래싱](https://en.wikipedia.org/wiki/Thrashing_(computer_science))(thrashing) 현상이 일어난다. [커널 모드](Processor.md#커널-모드)의 메모리 관리를 수행하는 작업 횟수가 방대해진 탓에 실질적인 [사용자 모드](Processor.md#사용자-모드)의 [프로세스](Process.md) 작업이 뒷전으로 미루어진 성능 저하를 초래한다.

## 작업 집합
**[작업 집합](https://en.wikipedia.org/wiki/Working_set)**(working set)은 커밋된 가상 메모리 중 RAM에 할당된 [페이지](#페이지)들을 일컫는다. 작업 관리자의 메모리 성능 지표에서 "사용 중" 메모리는 시스템 전반의 작업 집합을 의미한다. Windows OS는 작업 집합이 일정 비율에 도달하여 RAM의 여유 메모리 확보가 필요할 경우, 오랜 기간 동안 사용되지 않은 페이지를 [페이징 파일](#페이징-파일)로 [트리밍](https://techcommunity.microsoft.com/blog/askperf/prf-memory-management-working-set-trimming/373758)(trimming)한다.

* [성능 카운터](Perfmon.md#성능-카운터): `\Process(*)\Working Set` 또는 `\Process(*)\Working Set - Private`

## 주소 창 확장
**[주소 창 확장](https://learn.microsoft.com/en-us/windows/win32/memory/address-windowing-extensions)**(address windowing extensions; AWE)은 Windows OS에서 제공하는 메모리 확장 기능으로, 32비트 운영체제 당시 2 GB로 제한된 사용자 주소 공간에서 프로그램이 4 GB 이상의 RAM을 활용할 수 있도록 한다.

* Allow applications to allocate RAM that is never swapped by the operating system to or from disk.
* Allow an application to access more RAM than fits within the process’ address space.

64비트 윈도우에서는 32비트보다 훨씬 큰 가상 주소 공간을 제공하기 때문에 페이징은 더 이상 문제가 되지 않으나, 혹여나 모를 페이징을 방지하고 훨씬 빠른 메모리 접근을 위해 SQL 등에서 여전히 사용되고 있다.

Only the owning process can use the allocated RAM pages; AWE does not allow the RAM pages to be mapped into another process’ address space. Therefore, you cannot share RAM blocks between processes.

To help protect the allocation of RAM, the [AllocateUserPhysicalPages](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-allocateuserphysicalpages) function requires the caller to have the Lock Pages In Memory user right granted and enabled, or the function fails.

# 메모리 맵 파일
> *참고: [File Mapping - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/memory/file-mapping)*

**[메모리 맵 파일](https://en.wikipedia.org/wiki/Memory-mapped_file)**(memory-mapped file)은 파일과 바이트 대 바이트 상관관계를 맺은 [가상 메모리](#가상-메모리)를 가리킨다. 프로그램은 메모리 매핑된 파일을

![디스크의 파일, 파일 매핑 개체, 그리고 파일일 뷰의 관계도](https://learn.microsoft.com/en-us/windows/win32/memory/images/fmap.png)

Win32의 경우 [CreateFileMapping](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga) 함수를 제공한다.


메모리에 매핑하고 싶은 디스크상 파일은 아무런 파일이 될 수 있으며, 또는 시스템 페이징 파일이 될 수 있다. 파일 매핑 개체는 파일 전체 혹은 일부만을 구성할 수 있다. 개체는 디스크 상 파일에 의해 보조된다; 즉, 파일 매핑 개체가 페이징 아웃될 시 변경된 사항들은 파일에 적용된다. 그리고 파일 매핑 개체 페이지가 다시 페이징 인될 시 변경이 저장된 파일을 다시 불러온다.

> (개인 의견) 개체가 페이징이 된다고 파일 콘텐츠를 함께 가져가는 게 아닌 걸 설명하는 걸로 보임. 개체는 단순히 파일과의 매핑 정보를 제공하고, 실제 콘텐츠 내용을 담고 있는 게 아님을 문맥상 추정됨.

The file on disk can be any file that you want to map into memory, or it can be the system page file. The file mapping object can consist of all or only part of the file. It is backed by the file on disk. This means that when the system swaps out pages of the file mapping object, any changes made to the file mapping object are written to the file. When the pages of the file mapping object are swapped back in, they are restored from the file.


[파일](FileSystem.md)과 바이트 단위로 일대일 매핑된 [가상 메모리](#가상-메모리)를 가리킨다. 파일과 메모리 주소 간 상관관계가 형성될 시, 프로그램은 메모리 매핑된 파일 영역을 마치 주기억장치인 마냥 취급할 수 있다.

프로세스를 실행할 때, 운영체제는 실행 이미지 및 관련 모듈을 메모리 맵 파일을 활용하여 메모리로 불러온다.

Perhaps the most common use for a memory-mapped file is the process loader in most modern operating systems (including Windows and Unix-like systems.) When a process is started, the operating system uses a memory mapped file to bring the executable file, along with any loadable modules, into memory for execution. Most memory-mapping systems use a technique called demand paging, where the file is loaded into physical memory in subsets (one page each), and only when that page is actually referenced.[13] In the specific case of executable files, this permits the OS to selectively load only those portions of a process image that actually need to execute.


이미지 및 데이터 파일을 불러올 때, 내용물 전체를 물리 메모리로 가져오게 된다면 

열려고 하는 파일을 RAM으로 복사하기보다, 해당 파일의 디스크 영역을 페이징 파일로 할당하여 가상 메모리에 매핑되는 게 성능 효율적이다.


메모리 매핑은 [페이징 파일](#페이징-파일)을 완전히 우회할 뿐만 아니라

A possible benefit of memory-mapped files is a "[lazy loading](https://en.wikipedia.org/wiki/Lazy_loading)", thus using small amounts of RAM even for a very large file. Trying to load the entire contents of a file that is significantly larger than the amount of memory available can cause severe thrashing as the operating system reads from disk into memory and simultaneously writes pages from memory back to disk. Memory-mapping may not only bypass the page file completely, but also allow smaller page-sized sections to be loaded as data is being edited, similarly to [demand paging](https://en.wikipedia.org/wiki/Demand_paging) used for programs.

# 메모리 풀
> *참고: [Pushing the Limits of Windows: Paged and Nonpaged Pool | Microsoft Learn](https://learn.microsoft.com/en-us/archive/blogs/markrussinovich/pushing-the-limits-of-windows-paged-and-nonpaged-pool)*

윈도우 NT 운영체제에서 [메모리 풀](https://learn.microsoft.com/en-us/windows/win32/memory/memory-pools)(memory pools)은 [커널](Kernel.md) 및 [드라이버](Driver.md)에서 시스템 공간에 할당되고 관리되는 커널 [힙](https://en.wikipedia.org/wiki/Memory_management#Manual_memory_management) 메모리이다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="text-align: center;">커널 메모리 풀 유형</capation><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">페이징 풀 (paged pool)</th><th style="text-align: center;">비페이징 풀 (nonpaged pool)</th></tr></thead><tbody><tr style="text-align: center;"><td>페이징 파일로 이동될 수 있는 커널 메모리이다.</td><td>페이징 파일로 이동될 수 없는 커널 메모리이다.</td></tr></tbody></table>

운영체제 및 장치 드라이버는 [ExAllocatePoolWithTag](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-exallocatepoolwithtag) 루틴에 의해 할당 당시 4바이트 크기의 태그가 지정되는데, 이를 통해 해당 메모리를 할당한 드라이버 및 목적을 파악할 수 있다. 태그 목록은 [pooltags.txt](./references/pooltag.txt) 파일에서 찾아볼 수 있다.

컴퓨터 과학에서 언급하는 "[메모리 풀](https://en.wikipedia.org/wiki/Memory_pool)"과 동일한 개념으로 페이징 및 비페이징 풀로부터 할당받을 수 있는 총 커널 메모리 크기는 한정되어 있다. 운영체제와 아키텍처에 따라 한정된 용량은 상이하는 데, 64비트 NT 10 (윈도우 10 & 11, 서버 2016 등) 경우에는 각각 16 TB 그리고 RAM과 동일하거나 혹은 16 GB 중에서 가장 작은 크기로 선정된다.

> 메모리 풀의 용량 한도를 확인할 수 있는 도구로 Sysinternals의 [프로세스 탐색기](Procexp.md)(Process Explorer) 유틸리티 프로그램을 사용할 수 있다.

![프로세스 탐색기로 확인된 페이징 및 비페이징 풀의 사용량과 한도](./images/sysinternals_procexp_memory.png)

### Driver Locked 메모리
[운영체제 구성요소](Windows.md#운영체제-구성요소) 또는 장치 드라이버에서 직접적으로 관리되는 페이징될 수 없는 메모리 영역이다. 문맥상 비페이징 풀과 유사하지만, 중요한 데이터의 빠른 접근성을 위해 장치 드라이버에 특별히 최적화되어 상대적으로 빠른 대신 더 많은 리소스가 요구된다. [하이퍼-V](HyperV.md) 또는 [VMware](https://www.vmware.com/)와 같은 하이퍼바이저의 가상 머신에 배정한 RAM 메모리가 호스트 머신에서는 Driver Locked 메모리로 할당된다.

# 데이터 정렬
**[데이터 정렬](https://en.wikipedia.org/wiki/Data_structure_alignment)**(data alignment)은 컴퓨터의 [프로세서](Processor.md)가 접근할 데이터를 [메모리](#메모리)에서 어떻게 배치하여 정렬시킬 건지 방법을 제시한다. 프로세서가 데이터 접근 시 하드웨어 성능 효율을 높이기 위해 "자연스럽게 정렬(naturally aligned)"시키는 게 데이터 정렬의 목적이다; $n$-바이트 자료형의 데이터가 $n$-바이트 배수 간격의 메모리 주소에 할당되었을 때 정렬이 자연스럽다고 설명한다.

아래는 데이터 정렬의 반영 사례를 보여주기 위한 [C 언어](C.md)의 [구조체](C.md#구조체) 예시이다.

```c
struct STRUCTURE {
    char  field1;
    int   field2;
};

printf("%d", sizeof(struct STRUCTURE));
```
```
8
```

대체적으로 자료형마다 지정된 정렬 크기는 해당 자료형 크기와 일치하는 편이다: `char`은 1바이트 정렬, `short`는 2바이트 정렬, `int` 및 `float`는 4바이트 정렬이다. 다양한 자료형 맴버들로 구성될 수 있는 구조체의 경우, 메모리 공간 절약보다 접근 효율이 우선시되기 때문에 맴버 자료형이 갖는 가장 큰 정렬 크기의 배수만큼 메모리를 할당받아 맴버들을 정의된 순서대로 정렬시킨다.

* 위의 예시에서 1바이트의 `char`과 4바이트의 `int`를 합산하면 총 5바이트가 계산된다. 하지만 가장 큰 자료형인 `int`에 따라 4바이트 정렬되었기 때문에, 실제로 할당받은 메모리 크기는 총 8바이트이다.

다음은 세 가지 시나리오에 따른 데이터 정렬 방식을 설명한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">자료형 구성에 따른 데이터 정렬 비교</caption><colgroup><col style="width: 33.3%;"/><col style="width: 33.4%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th style="text-align: center;">8바이트: <code>char</code>-<code>int</code> 구조</th><th style="text-align: center;">8바이트: <code>char</code>-<code>short</code>-<code>int</code> 구조</th><th style="text-align: center;">12바이트: <code>char</code>-<code>int</code>-<code>short</code> 구조</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[3];  // +3
// ------ Addr: 0x00000004
    int   field2;       // +4
// ------ Addr: 0x00000008
};
```
</td><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[1];  // +1
    short field2;       // +2
// ------ Addr: 0x00000004
    int   field3;       // +4
// ------ Addr: 0x00000008
};
```
</td><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[3];  // +3
// ------ Addr: 0x00000004
    int   field2;       // +4
// ------ Addr: 0x00000008
    short field3;       // +2
//  char  Padding2[2];  // +2
// ------ Addr: 0x0000000C
};
```
</td></tr><tr style="vertical-align: top;"><td>정렬에 의해 데이터 간 여분이 발생하면 메모리의 연속성을 위해 패딩으로 메워진다.</td><td>가장 큰 int 자료형에 의해 4바이트 정렬이 되는 한편, 경계가 char과 short 자료형을 모두 담아낼 수 있는 크기이기 때문에 1바이트 패딩만 사이에 추가한다.</td><td>구조체 자체의 정렬을 위해, 구조체 크기는 정렬 크기의 배수이어야 한다. 맨 마지막 맴버의 자료형 크기가 정렬 크기에 미치지 못하면 나머지를 패딩으로 채운다.</td></tr></tbody></table>

[x86](https://en.wikipedia.org/wiki/X86) 아키텍처는 CPU 하드웨어가 자체적으로 데이터 정렬 문제를 교정하여 표면적으로 드러내지 않으나, 기본적으로 비활성화된 [EFLAG](Assembly.md#플래그-레지스터)의 AC (Alignment Check) 플래그를 설정하면 17h [인터럽트](Processor.md#인터럽트)를 일으켜 운영체제에 알린다. 반면, 단종된 [Itanium](https://en.wikipedia.org/wiki/Itanium) 아키텍처는 CPU에서 이를 스스로 교정할 수 없어 [SetErrorMode](https://learn.microsoft.com/en-us/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode) 함수 등을 사용하여 소프트웨어에서 조치되어야 한다.

* [Windows OS](Windows.md)의 경우, 예외 코드 0x80000002 STATUS_DATATYPE_MISALIGNMENT로 나타난다.
* [리눅스](https://en.wikipedia.org/wiki/Linux)의 경우, 두 개의 [캐시 라인](Processor.md#cpu-캐시)을 거쳐 처리하는 도중에 외부 간섭을 방지하기 위한 조치로 "[split lock](https://lwn.net/Articles/790464/)"이란 [원자적 연산](Processor.md#원자적-연산)을 동원한다.

# 페이지 테이블
**[페이지 테이블](https://en.wikipedia.org/wiki/Page_table)**(page table)은 [가상 주소](Process.md#가상-주소-공간)와 [물리 주소](https://en.wikipedia.org/wiki/Physical_address) 간 매핑하는 [가상 메모리](#가상-메모리) 체계에서 활용하는 데이터 구조이다.

### 물리 주소 확장
> *참고: [Physical Address Extension - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/memory/physical-address-extension)*

**[물리 주소 확장](https://en.wikipedia.org/wiki/Physical_Address_Extension)**(physical address extension; PAE)은 [x86](https://en.wikipedia.org/wiki/X86) [프로세서](Processor.md)에서 제공하는 메모리 관리 기능이다. 기존 32비트 [페이지 테이블](#페이지-테이블)은 두 단계의 32개 엔트리를 거쳐 변환을 하였다면, PAE가 활성화된 32비트는 세 단계의 64개 엔트리를 겨쳐 주소를 변환한다. 결과적으로 CPU는 최대 64 GB의 물리 메모리 주소를 접근할 수 있다. 프로세서의 CR4 레지스터의 PAE 비트 설정에 따라 활성 여부가 결정된다.

* 단, PAE 활성화 여부와 무관하게 32비트 [Windows OS](Windows.md)의 가상 메모리는 여전히 4 GB로 유지한다.

64비트를 네이티브하게 지원하는 [x86-64](https://en.wikipedia.org/wiki/X86-64) 프로세서는 더 이상 PAE와 유사한 기능을 제공하지 않는다.

## x86 페이지 테이블
64비트 프로세서의 [워드](https://en.wikipedia.org/wiki/Word_(computer_architecture))가 8바이트이기 때문에, 32비트 페이징에 불과하고 인덱스의 엔트리는 8바이트 간격으로 구분되어 있음을 유의한다.

### 32비트 페이징, 4 KB 페이지, PAE 비활성
![No PAE, 4 KB pages](https://upload.wikimedia.org/wikipedia/commons/8/8e/X86_Paging_4K.svg)

### 32비트 페이징, 4 MB 페이지, PAE 비활성
![No PAE, 4 MB pages](https://upload.wikimedia.org/wikipedia/commons/d/d9/X86_Paging_4M.svg)

### 32비트 페이징, 4 KB 페이지, PAE 활성
![With PAE; 4 KB pages](https://upload.wikimedia.org/wikipedia/commons/0/0d/X86_Paging_PAE_4K.svg)

이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 그 전에 먼저 PAE가 활성화가 된 시스템이란 걸 아래와 같이 검증한다.

```
0: kd> .formats cr0
Evaluate expression:
  Hex:     80010033
  Decimal: -2147418061
  Decimal (unsigned) : 2147549235
  Octal:   20000200063
  Binary:  10000000 00000001 00000000 00110011
  Chars:   ...3
  Time:    ***** Invalid
  Float:   low -9.1907e-041 high -1.#QNAN
  Double:  -1.#QNAN
0: kd> .formats cr4
Evaluate expression:
  Hex:     001406e9
  Decimal: 1312489
  Decimal (unsigned) : 1312489
  Octal:   00005003351
  Binary:  00000000 00010100 00000110 11101001
  Chars:   ....
  Time:    Fri Jan 16 13:34:49 1970
  Float:   low 1.83919e-039 high 0
  Double:  6.48456e-318
```

다음은 System 프로세스와 nt!KeBugCheckEx 코드가 위치한 가상 메모리 주소와 이를 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name:  Not Found
Windows 10 Kernel Version 19041 MP (2 procs) Free x86 compatible
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.x86fre.vb_release.191206-1406
Kernel base = 0x81a70000 PsLoadedModuleList = 0x81d34d98

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 004f4454 00000000 00000000 00000000

Crashing Stack
============================================
 # ChildEBP RetAddr      
00 83452e08 81a34bde     nt!KeBugCheckEx
01 83452e38 81cfe89a     hal!HalBugCheckSystem+0x6d
02 83452ec4 81a3680d     nt!WheaReportHwError+0x418
03 83452ed8 81c0a6da     hal!HalHandleNMI+0xea
    <Intermediate frames may have been skipped due to lack of complete unwind>
04 83452ed8 81a1c76b (T) nt!KiTrap02+0x322
    <Intermediate frames may have been skipped due to lack of complete unwind>
05 83452fe0 8344699c (T) hal!HalProcessorIdle+0x7
WARNING: Frame IP not in any known module. Following frames may be wrong.
06 83452fe4 00000000     0x8344699c

0: kd> r cr3
cr3=001a8000
0: kd> !process 0 0 System
PROCESS 82354040  SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 001a8000  ObjectTable: 86002d80  HandleCount: 2327.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     81beef4c
  Decimal: -2118193332
  Decimal (unsigned) : 2176773964
  Octal:   20157567514
  Binary:  10000001 10111110 11101111 01001100
  Chars:   ...L
  Time:    ***** Invalid
  Float:   low -7.01384e-038 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 Page Directory를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x001a8000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (4 KB w/ PAE)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">3</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>10</code></td><td style="text-align: center;"><code>0x002</code></td><td><code>001ab001</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Directory</td><td style="text-align: center;"><code>000001 101</code></td><td style="text-align: center;"><code>0x00D</code></td><td><code>01b09063</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Table</td><td style="text-align: center;"><code>11110 1110</code></td><td style="text-align: center;"><code>0x1EE</code></td><td><code>02fde121</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>1111 01001100</code></td><td style="text-align: center;"><code>0xF4C</code></td><td><code>55</code> (BYTE)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                    VA 81beef4c
PDE at C0602068            PTE at C040DF70
contains 0000000001B09063  contains 0000000002DEC121
pfn 1b09      ---DA--KWEV  pfn 2dec      -G--A--KREV

0: kd> u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
81beef4c 55              push    ebp

0: kd> !kext.dd 00000000`001a8000+8*0x002 L1
# 1a8010 001ab001

0: kd> !kext.dd 00000000`001ab000+8*0x00D L1
# 1ab068 01b09063

0: kd> !kext.dd 00000000`01b09000+8*0x1EE L1
# 1b0af00 02fde121

0: kd> !kext.db 00000000`02dec000+1*0xF4C L1
# 2decf4c 55 U...............
```

### 32비트 페이징, 2 MB 페이지, PAE 활성
![With PAE; 2 MB pages](https://upload.wikimedia.org/wikipedia/commons/0/05/X86_Paging_PAE_2M.svg)

이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 그 전에 먼저 PAE가 활성화가 된 시스템이란 걸 아래와 같이 검증한다.

```
0: kd> .formats cr0
Evaluate expression:
  Hex:     80010033
  Decimal: -2147418061
  Decimal (unsigned) : 2147549235
  Octal:   20000200063
  Binary:  10000000 00000001 00000000 00110011
  Chars:   ...3
  Time:    ***** Invalid
  Float:   low -9.1907e-041 high -1.#QNAN
  Double:  -1.#QNAN
0: kd> .formats cr4
Evaluate expression:
  Hex:     001406e9
  Decimal: 1312489
  Decimal (unsigned) : 1312489
  Octal:   00005003351
  Binary:  00000000 00010100 00000110 11101001
  Chars:   ....
  Time:    Fri Jan 16 13:34:49 1970
  Float:   low 1.83919e-039 high 0
  Double:  6.48456e-318
```

다음은 System 프로세스와 nt!KeBugCheckEx 코드가 위치한 가상 메모리 주소와 이를 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name:  Not Found
Windows 10 Kernel Version 19041 MP (2 procs) Free x86 compatible
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.x86fre.vb_release.191206-1406
Kernel base = 0x82800000 PsLoadedModuleList = 0x82ac4d98

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 004f4454 00000000 00000000 00000000

Crashing Stack
============================================
 # ChildEBP RetAddr      
00 87c52e08 83032bde     nt!KeBugCheckEx
01 87c52e38 82a8e89a     hal!HalBugCheckSystem+0x6d
02 87c52ec4 8303480d     nt!WheaReportHwError+0x418
03 87c52ed8 8299a6da     hal!HalHandleNMI+0xea
    <Intermediate frames may have been skipped due to lack of complete unwind>
04 87c52ed8 82a59b77 (T) nt!KiTrap02+0x322
    <Intermediate frames may have been skipped due to lack of complete unwind>
05 87c52fe8 00000000 (T) nt!PpmIdleGuestExecute+0x1a

0: kd> r cr3
cr3=001a8000
0: kd> !process 0 0 System
PROCESS 8636c040  SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 001a8000  ObjectTable: 88202d80  HandleCount: 2307.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     8297ef4c
  Decimal: -2103972020
  Decimal (unsigned) : 2190995276
  Octal:   20245767514
  Binary:  10000010 10010111 11101111 01001100
  Chars:   ...L
  Time:    ***** Invalid
  Float:   low -2.23248e-037 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 Page Directory를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x001a8000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (2 MB w/ PAE)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">2</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>10</code></td><td style="text-align: center;"><code>0x002</code></td><td><code>001ab001</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Directory</td><td style="text-align: center;"><code>000010 100</code></td><td style="text-align: center;"><code>0x014</code></td><td><code>01b09063</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>10111 11101111 01001100</code></td><td style="text-align: center;"><code>0x17EF4C</code></td><td><code>55</code> (BYTE)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                    VA 8297ef4c
PDE at C06020A0            PTE at C0414BF0
contains 0000000002C009E3  contains 0000000000000000
pfn 2c00      -GLDA--KWEV    LARGE PAGE pfn 2d7e

0: kd> u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
8297ef4c 55              push    ebp

0: kd> !kext.dd 00000000`001a8000+8*0x002 L1
# 1a8010 001ab001

0: kd> !kext.dd 00000000`001ab000+8*0x014 L1
# 1ab068 01b09063

0: kd> !kext.db 00000000`02c00000+1*0x17EF4C L1
# 2d7ef4c 55 U...............
```

## x86-64 페이지 테이블

### 64비트 페이징, 4 KB 페이지
이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 KERNEL32!BaseThreadInitThunk 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 아래는 가상 메모리 주소로부터 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name: DESKTOP-TKOG2TV
Windows 10 Kernel Version 19041 MP (2 procs) Free x64
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.amd64fre.vb_release.191206-1406
Kernel base = 0xfffff800`02e00000 PsLoadedModuleList = 0xfffff800`03a2a770

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 00000000`004f4454 00000000`00000000 00000000`00000000 00000000`00000000

Crashing Stack
============================================
  *** Stack trace for last set context - .thread/.cxr resets it
 # Child-SP          RetAddr               Call Site
00 fffff800`09499b58 fffff800`032b965a     nt!KeBugCheckEx
01 fffff800`09499b60 fffff800`01f015b0     nt!HalBugCheckSystem+0x7a
02 fffff800`09499ba0 fffff800`033bb9ae     PSHED!PshedBugCheckSystem+0x10
03 fffff800`09499bd0 fffff800`032bdd12     nt!WheaReportHwError+0x46e
04 fffff800`09499cb0 fffff800`03312f64     nt!HalHandleNMI+0x142
05 fffff800`09499ce0 fffff800`0320a642     nt!KiProcessNMI+0x134
06 fffff800`09499d30 fffff800`0320a412     nt!KxNmiInterrupt+0x82
07 fffff800`09499e70 fffff800`0303920e     nt!KiNmiInterrupt+0x212
08 ffffd306`22f57330 fffff800`030370dd     nt!RtlpHpVsContextAllocateInternal+0xbe
09 ffffd306`22f57390 fffff800`037b7074     nt!ExAllocateHeapPool+0x6ed
0a ffffd306`22f574d0 fffff800`034b5395     nt!ExAllocatePoolWithTag+0x64
0b ffffd306`22f57520 fffff800`03447fd0     nt!PfpCopyUserPfnPrioRequest+0xe5
0c ffffd306`22f57580 fffff800`0341248c     nt!PfpPfnPrioRequest+0x60
0d ffffd306`22f57600 fffff800`0341090b     nt!PfQuerySuperfetchInformation+0x2ec
0e ffffd306`22f57740 fffff800`0340e8b7     nt!ExpQuerySystemInformation+0x1f0b
0f ffffd306`22f57a80 fffff800`03211138     nt!NtQuerySystemInformation+0x37
10 ffffd306`22f57ac0 00007ffe`4802d6a4     nt!KiSystemServiceCopyEnd+0x28
11 000000c6`b9779b88 00007ffe`406bed47     ntdll!NtQuerySystemInformation+0x14
12 000000c6`b9779b90 00007ffe`406a9766     sysmain!PfPdPfnsQuery+0x297
13 000000c6`b977ce70 00007ffe`406a954c     sysmain!PfRbMemoryQuery+0x162
14 000000c6`b977ceb0 00007ffe`406aa68f     sysmain!AgPdProcessorCallback+0x2c
15 000000c6`b977cee0 00007ffe`406aa3b1     sysmain!PfSvSuperfetchProcessAction+0x53
16 000000c6`b977cf60 00007ffe`406e6fe1     sysmain!PfSvSuperfetchProcessActions+0x59
17 000000c6`b977cfa0 00007ffe`406fb88e     sysmain!PfSvcMainThreadWorker+0xf91
18 000000c6`b977f600 00007ffe`406e81bf     sysmain!PfSvcMainThread+0x22
19 000000c6`b977f640 00007ff7`b4e84340     sysmain!SysMtServiceMain+0x10f
1a 000000c6`b977f680 00007ffe`4697dfd8     svchost!ServiceStarter+0x310
1b 000000c6`b977f7b0 00007ffe`47017344     sechost!ScSvcctrlThreadA+0x28
1c 000000c6`b977f7e0 00007ffe`47fe26b1     KERNEL32!BaseThreadInitThunk+0x14
1d 000000c6`b977f810 00000000`00000000     ntdll!RtlUserThreadStart+0x21

0: kd> r cr3
Last set context:
cr3=0000000018573000
0: kd> !process 0 0 System
PROCESS ffffe70601a7a080
    SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 007d4000  ObjectTable: ffff8f007c61fc80  HandleCount: 2454.
    Image: System

0: kd> .formats KERNEL32!BaseThreadInitThunk+0x14
Evaluate expression:
  Hex:     00007ffe`47017344
  Decimal: 140730089698116
  Decimal (unsigned) : 140730089698116
  Octal:   0000003777710700271504
  Binary:  00000000 00000000 01111111 11111110 01000111 00000001 01110011 01000100
  Chars:   ...G.sD
  Time:    Wed Jun 13 06:10:08.969 1601 (UTC + 9:00)
  Float:   low 33139.3 high 4.59149e-041
  Double:  6.95299e-310
```

System 프로세스의 메모리 주소 변환에 필요한 PML4를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x0000000018573000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (4 KB 페이지)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">4</td><td>PML4</td><td style="text-align: center;"><code>01111111 1</code></td><td style="text-align: center;"><code>0x0FF</code></td><td><code>0a000000`1857f867</code></td></tr><tr><td style="text-align: center;">3</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>1111110 01</code></td><td style="text-align: center;"><code>0x1F9</code></td><td><code>0a000000`18582867</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Directory</td><td style="text-align: center;"><code>000111 000</code></td><td style="text-align: center;"><code>0x038</code></td><td><code>0a000000`185c8867</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Table</td><td style="text-align: center;"><code>00001 0111</code></td><td style="text-align: center;"><code>0x017</code></td><td><code>01000000`0174a025</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>0011 01000100</code></td><td style="text-align: center;"><code>0x344</code></td><td><code>c88b</code> (WORD)</td></tr></tbody></table>

```windbg
0: kd> !pte KERNEL32!BaseThreadInitThunk+0x14
                                           VA 00007ffe47017344
PXE at FFFFCEE773B9D7F8    PPE at FFFFCEE773AFFFC8    PDE at FFFFCEE75FFF91C0    PTE at FFFFCEBFFF2380B8
contains 0A0000001857F867  contains 0A00000018582867  contains 0A000000185C8867  contains 010000000174A025
pfn 1857f     ---DA--UWEV  pfn 18582     ---DA--UWEV  pfn 185c8     ---DA--UWEV  pfn 174a      ----A--UREV

0: kd> u KERNEL32!BaseThreadInitThunk+0x14 L1
KERNEL32!BaseThreadInitThunk+0x14:
00007ffe`47017344 8bc8            mov     ecx,eax

0: kd> !kext.dq 00000000`18573000+8*0x0FF L1
# 185737f8 0a000000`1857f867

0: kd> !kext.dq 00000000`1857f000+8*0x1F9 L1
# 1857ffc8 0a000000`18582867

0: kd> !kext.dq 00000000`18582000+8*0x038 L1
# 185821c0 0a000000`185c8867

0: kd> !kext.dq 00000000`185c8000+8*0x017 L1
# 185c80b8 01000000`0174a025

0: kd> !kext.db 00000000`0174A000+1*0x344 L2
# 174a344 8b c8 ................
```

### 64비트 페이징, 2 MB 페이지
이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 아래는 가상 메모리 주소로부터 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name: DESKTOP-TKOG2TV
Windows 10 Kernel Version 19041 MP (2 procs) Free x64
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.amd64fre.vb_release.191206-1406
Kernel base = 0xfffff800`02e00000 PsLoadedModuleList = 0xfffff800`03a2a770

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 00000000`004f4454 00000000`00000000 00000000`00000000 00000000`00000000

Crashing Stack
============================================
  *** Stack trace for last set context - .thread/.cxr resets it
 # Child-SP          RetAddr               Call Site
00 fffff800`09499b58 fffff800`032b965a     nt!KeBugCheckEx
01 fffff800`09499b60 fffff800`01f015b0     nt!HalBugCheckSystem+0x7a
02 fffff800`09499ba0 fffff800`033bb9ae     PSHED!PshedBugCheckSystem+0x10
03 fffff800`09499bd0 fffff800`032bdd12     nt!WheaReportHwError+0x46e
04 fffff800`09499cb0 fffff800`03312f64     nt!HalHandleNMI+0x142
05 fffff800`09499ce0 fffff800`0320a642     nt!KiProcessNMI+0x134
06 fffff800`09499d30 fffff800`0320a412     nt!KxNmiInterrupt+0x82
07 fffff800`09499e70 fffff800`0303920e     nt!KiNmiInterrupt+0x212
08 ffffd306`22f57330 fffff800`030370dd     nt!RtlpHpVsContextAllocateInternal+0xbe
09 ffffd306`22f57390 fffff800`037b7074     nt!ExAllocateHeapPool+0x6ed
0a ffffd306`22f574d0 fffff800`034b5395     nt!ExAllocatePoolWithTag+0x64
0b ffffd306`22f57520 fffff800`03447fd0     nt!PfpCopyUserPfnPrioRequest+0xe5
0c ffffd306`22f57580 fffff800`0341248c     nt!PfpPfnPrioRequest+0x60
0d ffffd306`22f57600 fffff800`0341090b     nt!PfQuerySuperfetchInformation+0x2ec
0e ffffd306`22f57740 fffff800`0340e8b7     nt!ExpQuerySystemInformation+0x1f0b
0f ffffd306`22f57a80 fffff800`03211138     nt!NtQuerySystemInformation+0x37
10 ffffd306`22f57ac0 00007ffe`4802d6a4     nt!KiSystemServiceCopyEnd+0x28
11 000000c6`b9779b88 00007ffe`406bed47     ntdll!NtQuerySystemInformation+0x14
12 000000c6`b9779b90 00007ffe`406a9766     sysmain!PfPdPfnsQuery+0x297
13 000000c6`b977ce70 00007ffe`406a954c     sysmain!PfRbMemoryQuery+0x162
14 000000c6`b977ceb0 00007ffe`406aa68f     sysmain!AgPdProcessorCallback+0x2c
15 000000c6`b977cee0 00007ffe`406aa3b1     sysmain!PfSvSuperfetchProcessAction+0x53
16 000000c6`b977cf60 00007ffe`406e6fe1     sysmain!PfSvSuperfetchProcessActions+0x59
17 000000c6`b977cfa0 00007ffe`406fb88e     sysmain!PfSvcMainThreadWorker+0xf91
18 000000c6`b977f600 00007ffe`406e81bf     sysmain!PfSvcMainThread+0x22
19 000000c6`b977f640 00007ff7`b4e84340     sysmain!SysMtServiceMain+0x10f
1a 000000c6`b977f680 00007ffe`4697dfd8     svchost!ServiceStarter+0x310
1b 000000c6`b977f7b0 00007ffe`47017344     sechost!ScSvcctrlThreadA+0x28
1c 000000c6`b977f7e0 00007ffe`47fe26b1     KERNEL32!BaseThreadInitThunk+0x14
1d 000000c6`b977f810 00000000`00000000     ntdll!RtlUserThreadStart+0x21

0: kd> r cr3
Last set context:
cr3=0000000018573000
0: kd> !process 0 0 System
PROCESS ffffe70601a7a080
    SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 007d4000  ObjectTable: ffff8f007c61fc80  HandleCount: 2454.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     fffff800`031fd5b0
  Decimal: -8796040604240
  Decimal (unsigned) : 18446735277668947376
  Octal:   1777777600000307752660
  Binary:  11111111 11111111 11111000 00000000 00000011 00011111 11010101 10110000
  Chars:   ........
  Time:    ***** Invalid FILETIME
  Float:   low 4.69712e-037 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 PML4를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x0000000018573000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (2 MB 페이지)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">3</td><td>PML4</td><td style="text-align: center;"><code>11111000 0</code></td><td style="text-align: center;"><code>0x1F0</code></td><td><code>00000000`04709063</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>0000000 00</code></td><td style="text-align: center;"><code>0x000</code></td><td><code>00000000`0460a063</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Directory</td><td style="text-align: center;"><code>000011 000</code></td><td style="text-align: center;"><code>0x018</code></td><td><code>0a000000`02a001a1</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>11111 11010101 10110000</code></td><td style="text-align: center;"><code>0x1FD5B0</code></td><td><code>48894c2408</code> (5-Byte)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                                           VA fffff800031fd5b0
PXE at FFFFCEE773B9DF80    PPE at FFFFCEE773BF0000    PDE at FFFFCEE77E0000C0    PTE at FFFFCEFC00018FE8
contains 0000000004709063  contains 000000000460A063  contains 0A00000002A001A1  contains 0000000000000000
pfn 4709      ---DA--KWEV  pfn 460a      ---DA--KWEV  pfn 2a00      -GL-A--KREV  LARGE PAGE pfn 2bfd   

0: kd> u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
fffff800`031fd5b0 48894c2408      mov     qword ptr [rsp+8],rcx

0: kd> !kext.dq 00000000`18573000+8*0x1F0 L1
# 18573f80 00000000`04709063

0: kd> !kext.dq 00000000`04709000+8*0x000 L1
# 4709000 00000000`0460a063

0: kd> !kext.dq 00000000`0460A000+8*0x18 L1
# 460a0c0 0a000000`02a001a1

0: kd> !kext.db 00000000`02a00000+1*0x1FD5B0 L5
# 2bfd5b0 48 89 4c 24 08 H.L$............
```

## ARM64 페이지 테이블
