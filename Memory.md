# 메모리
**[메모리](https://en.wikipedia.org/wiki/Computer_memory)**(memory)는 시스템에서 즉각적으로 사용할 데이터를 저장하는 공간이다. 흔히 [주기억장치](https://en.wikipedia.org/wiki/Computer_data_storage#Primary_storage)의 대표격인 [RAM](https://en.wikipedia.org/wiki/Random-access_memory)을 언급하는 경우가 대다수이며, 작업 속도가 매우 빠르고 [휘발성](https://en.wikipedia.org/wiki/Volatile_memory)인 특징을 가지고 있다. 또한 [프로세서](Processor.md)와 물리적으로 근접하여 연산시 데이터 접근 속도가 순식간이기 때문에 단기기억 역할을 담당한다. 그러므로 메모리는 시스템의 성능을 결정하는 중요한 요소로 작용한다.

본문에서 사용되는 메모리 용어들을 다음과 같이 정리한다.

* **[가상 메모리](#가상-메모리)**: *(하드웨어와 독립적으로) [운영체제](https://en.wikipedia.org/wiki/Operating_system)로부터 표현된 메모리이며, 프로세서에 의해 처리되려면 "물리 메모리"가 반드시 필요하다.*
    * **물리 메모리**: *가상 메모리를 담을 수 있는 하드웨어 실체가 있는 메모리이며, [RAM](https://en.wikipedia.org/wiki/Random-access_memory) 및 [페이징 파일](#페이징-파일)이 해당한다.*
* **[보조 메모리](Storage.md)**: *데이터를 오래 저장할 수 있는 [비휘발성](https://en.wikipedia.org/wiki/Non-volatile_random-access_memory) 메모리이며, [HDD](Storage.md#디스크) 및 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브) 등의 저장 장치가 해당한다.*

각 메모리가 의미하는 바가 무엇인지 이해를 돕기 위해 다음 8 GB RAM이 설치된 시스템의 [작업 관리자](TaskMgr.md)를 예시로 설명한다.

![작업 관리자에서 확인한 메모리 성능 (<a href="Windows.md">윈도우 11</a>, 버전 22H2)](./images/memory_taskmgr.png)

설치된 RAM 중에서 4.2 GB 메모리를 사용하며, 아래와 같이 수치를 살펴볼 수 있다. 자세한 내용을 이해하려면 하기 부문들을 읽기를 적극 권장한다.

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">작업 관리자의 메모리 구성원 소개</caption><colgroup><col style="width: 15%;"/><col style="width: 12%;"/><col style="width: 8%;"/><col/></colgroup><thead><tr><th style="text-align: center;">메모리 구성</th><th style="text-align: center;">그룹</th><th style="text-align: center;">유형</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><b><a href="#작업-집합">사용 중</a></b><br/>(1.2 GB)</td><td style="text-align: center;">-</td><td style="text-align: center;">-</td><td><a href="Kernel.md">커널</a> 및 <a href="Driver.md">드라이버</a>, <a href="Process.md#프로세스">프로세스</a>을 포함한 시스템 전반에서 사용 중인 RAM 크기이다.<ul><li>사용 중인 메모리 내에서도 공간 절약이 가능하여 <a href="https://en.wikipedia.org/wiki/Virtual_memory_compression">압축된 메모리</a><sup>†</sup>는 151 MB이다.</li></ul></td></tr><tr><td rowspan="3" style="text-align: center;"><b>사용 가능</b><br/>(2.4 GB)</td><td rowspan="2" style="text-align: center;"><i>여유</i><br/>(448 MB)</td><td style="text-align: center;">영값</td><td>메모리가 전부 영값으로 비워져 (혹은 채워져) 아무런 데이터가 없어 곧장 사용될 수 있다.<ul><li>오로지 영값으로 정리된 페이지만 타 프로그램에서 확보하여 사용할 수 있다.</li></ul></td></tr><tr><td style="text-align: center;">해제</td><td>프로그램이 종료되면서 함께 해제된 메모리이다. 당시 데이터가 잔여하기 때문에 차후 시스템 스레드에 의해 영값으로 정리된다.<ul><li>영값 페이지가 고갈될 시 대신 제공하지만, 데이터를 정리하는 데 시간이 다소 소모된다.</li></ul></td></tr><tr><td rowspan="2" style="text-align: center;"><i><a href="https://en.wikipedia.org/wiki/Cache_(computing)">캐시</a></i><br/>(2.5 GB)</td><td style="text-align: center;">대기</td><td>프로세스의 가상 메모리와 매핑되었을 당시 데이터가 잔여하여, 만일 프로세스가 해당 페이지를 다시 필요할 경우 단순 매핑만으로 곧바로 사용할 수 있다.</td></tr><tr><td style="text-align: center;"><b>기타</b><br/>(544 MB)</td><td style="text-align: center;">수정</td><td>프로세스의 가상 메모리와 매핑되었을 당시 데이터를 먼저 디스크에 저장을 완료한 다음에 대기 페이지로 전환되기 때문에 <i>사용 중</i> 또는 <i>사용 가능</i> 메모리에 속하지 않는다.</td></tr></tbody></table>

<sup>_† [윈도우 10](https://en.wikipedia.org/wiki/Windows_10)의 [참가자 빌드 10525](https://blogs.windows.com/windows-insider/2015/08/18/announcing-windows-10-insider-preview-build-10525/)부터 소개되어, [작업 집합](#작업-집합)의 가상 메모리를 [디스크](Storage.md)로 [페이징](#페이징-파일)하기 전에 앞서 [페이지](#페이지)를 압축하여 가능한 RAM에 상주하는 기법을 꾀하기 위해 도입된 "메모리 압축(Memory Compression)" [프로세스](Process.md)이다. 해당 프로세스의 작업 집합은 작업 관리자의 메모리 성능에 표시된 '(압축)' 크기와 일치한다._</sup>

## 메모리 관리 장치
> *참고: [Converting Virtual Addresses to Physical Addresses - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/converting-virtual-addresses-to-physical-addresses)*

**[메모리 관리 장치](https://en.wikipedia.org/wiki/Memory_management_unit)**(memory management unit; MMU)는 [프로세서](Processor.md)에 내장되어 있는 반도체로 가상 메모리에서 RAM의 물리 메모리로 주소를 변환하는 역할을 전담한다. 메모리 주소를 변환하는 절차는 결국 프로세서 아키텍처에 의해 정의되며, 변환 과정에 대한 상세한 내용은 *[페이지 테이블](PageTable.md)* 문서를 참고한다.

# 가상 메모리
> *참고: [Pushing the Limits of Windows: Virtual Memory | Microsoft Learn](https://learn.microsoft.com/en-us/archive/blogs/markrussinovich/pushing-the-limits-of-windows-virtual-memory)*

**[가상 메모리](https://en.wikipedia.org/wiki/Virtual_memory)**(virtual memory)는 일종의 [메모리 관리](https://en.wikipedia.org/wiki/Memory_management_(operating_systems)) 기술이며, 하드웨어 매체의 실체와 무관하게 [운영체제](https://en.wikipedia.org/wiki/Operating_system)에 의해 표현된 "가상"의 메모리이다.<sup>[[참고](https://learn.microsoft.com/en-us/windows/win32/memory/virtual-memory-functions)]</sup> ([커널](Kernel.md)을 포함한) 모든 프로그램들은 가상 메모리에서 실행되며, 각 프로세스마다 주어진 [가상 주소 공간](Process.md#가상-주소-공간)에 메모리를 할당받는다. 가상 주소 공간에 할당된 가상 메모리의 주소는 프로세서에 내장된 [MMU](#메모리-관리-장치)에 의해 RAM의 물리 메모리 주소로 변환되어 접근된다.

가상 메모리를 활용한 시스템은 다음과 같은 이점을 지닌다:<sup>[[참고](https://learn.microsoft.com/en-us/windows/win32/memory/virtual-memory-functions)]</sup>

1. 메모리 체계(예를 들어, 공유 메모리 등)를 커널에서 관리하기 때문에 프로그램 개발의 편리
1. [페이징](#페이징-파일) 기술을 통해 기존 RAM의 물리적 제약보다 더 많은 주소 공간 확보
1. 메모리 격리에 의한 시스템 보안 강화

## 페이지
**[페이지](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page)는 운영체제에서 관리하는 가장 작은 단위의 가상 메모리 조각이다 ([x86](https://en.wikipedia.org/wiki/X86) [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)의 경우 4 [KB](https://en.wikipedia.org/wiki/Kilobyte)). 페이지와 일대일 매핑된 RAM의 주소 영역을 **[페이지 프레임](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page frame; 일명 프레임)이라고 부른다. 페이지는 가상 메모리의 가용 여부를 결정하는 세 가지 [상태](https://learn.microsoft.com/en-us/windows/win32/memory/page-state)로 분류된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">가상 메모리의 페이지 상태</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">상태</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">여유<br/>(Free)</td><td>사용되고 있지 않는 가상 주소 공간의 페이지이다.</td></tr><tr><td style="text-align: center;">예약됨<br/>(Reserved)</td><td>가상 메모리로 사용 중이지만, 아직 물리 메모리가 할당되지 않은 가상 주소 공간의 페이지이다. 즉, 예약된 페이지는 시스템의 메모리 성능에 기여를 하지 않는다.<ul><li><i>가상 메모리는 할당 입도(allocation granularity) 경계의 첫 주소를 시작으로 페이지를 예약받는다.</i></li></ul></td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/introduction-to-the-page-file#system-committed-memory">커밋됨</a><br/>(Committed)</td><td>가상 메모리로 사용 중이면서 물리 메모리가 할당된 가상 주소 공간의 페이지이다. 커밋된 페이지는 시스템의 메모리 성능에 실질적으로 기여한다. 다만, 예약된 페이지 영역 전체를 할당하지 않아도 된다.<ul><li><i>커밋된 페이지에 최초의 R/W 시도가 있기 전까지 할당된 물리 메모리에 매핑되지 않는다.</i></li><li><i>RAM의 성능 효율과 시스템 안정성을 위해, 가급적 페이징 파일에 커밋하여 RAM으로 가져온다.</i></li></ul></td></tr></tbody></table>

<sup>_† [x86](https://en.wikipedia.org/wiki/X86) [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)는 페이지 크기를 4 KB (00001000h), 그리고 할당 입도 경계 간격을 64 KB (00010000h)로 구분한다. 반면, 단종된 [IA-64](https://en.wikipedia.org/wiki/Itanium) 아키텍처는 페이지 크기를 8 KB로 정의한다._</sup>

커밋될 수 있는 최대 페이지 크기, 즉 커밋 한도(commit limit)는 결과적으로 물리 메모리의 용량에 의존한다. 다시 말해, 비록 가상 메모리일지라도 RAM과 [페이징 파일](#페이징-파일)이 제공하는 하드웨어 용량이 부족하면 추가 메모리 할당은 불가하다.

다음은 페이지 읽기, 쓰기, 그리고 실행 가능 여부를 결정하는 보호 속성들을 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">가상 메모리의 페이지 보호 속성</caption><colgroup><col style="width: 30%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 40%;"/></colgroup><thead><tr><th style="text-align: center;">보호 속성</th><th style="text-align: center;">읽기</th><th style="text-align: center;">쓰기</th><th style="text-align: center;">실행</th><th style="text-align: center;">부연 설명</th></tr></thead><tbody><tr><td><code>PAGE_NOACCESS</code></td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td>어떠한 접근 행위라도 <a href="https://en.wikipedia.org/wiki/Segmentation_fault">예외</a>를 일으킨다.</td></tr><tr><td><code>PAGE_READONLY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td rowspan="4" style="text-align: center;">-</td></tr><tr><td><code>PAGE_READWRITE</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td></tr><tr><td><code>PAGE_EXECUTE</code></td><td style="text-align: center;">❌</td><td style="text-align: center;">❌</td><td style="text-align: center;">✔️</td></tr><tr><td><code>PAGE_EXECUTE_READ</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td style="text-align: center;">✔️</td></tr><tr><td><code>PAGE_EXECUTE_READWRITE</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td>어떠한 접근 행위라도 <a href="https://en.wikipedia.org/wiki/Segmentation_fault">예외</a>를 일으키지 않는다.</td></tr><tr><td><code>PAGE_WRITECOPY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">❌</td><td rowspan="2"><a href="#copy-on-write">Copy-on-write</a>로 페이지 데이터를 변경할 시 <a href="Process.md">프로세스</a> 전용 메모리에 페이지 복사본을 제공한다.</td></tr><tr><td><code>PAGE_EXECUTE_WRITECOPY</code></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td></tr></tbody></table>

<sup>_† 참고: [Memory Protection Constants (WinNT.h) - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/memory/memory-protection-constants)_</sup>

Windows OS는 [데이터 실행 방지](https://learn.microsoft.com/en-us/windows/win32/memory/data-execution-prevention)(Data Execution Prevention; DEP)를 활성화할 경우, 코드 실행이 의도된 메모리 영역에 `PAGE_EXECUTE_*` 보호 속성을 적용하는 등 멀웨어로부터 유해한 코드나 데이터를 삽입하는 행위를 방지한다.

* **[가드 페이지](https://learn.microsoft.com/en-us/windows/win32/memory/creating-guard-pages)**(guard page)는 메모리 접근 시 단발성 알람을 제공하는 특수한 페이지이다. 대용량 동적 자료 구조의 성장 모니터링이 필요한 응용 프로그램에 매우 유용하다. 대표적인 활용 사례로 운영체제는 가드 페이지를 [호출 스택](Thread.md#호출-스택)에 활용하여 [스택 오버플로우](Thread.md#스택-오버플로우)를 감시한다.

### 페이징 파일
**[페이징 파일](https://learn.microsoft.com/en-us/windows/client-management/introduction-page-file)**(paging file)은 [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)나 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)와 같은 [저장 장치](Storage.md)에 상주하는 pagefile.sys 파일이며, RAM을 보조하는 일종의 물리 메모리이다. 하드웨어 기술 향상으로 옛 컴퓨터에 비해 RAM의 용량은 대폭 증가하였으나, 운영체제는 여전히 다음 페이징 파일 기능을 활발히 사용한다:

1. RAM에 여유 공간 확보가 필요하면 사용 빈도가 가장 낮은 [작업 집합](#작업-집합)을 페이징 파일로 이동시킨다.
1. 커밋한 [페이지](#페이지)는 RAM의 성능 효율 및 시스템 안정성을 위해 가능한 페이징 파일에 할당하여 불러온다.
1. [블루스크린](BSOD.md)이 발생하면 [메모리 덤프](Dump.md#커널-모드-덤프)를 ([파일 시스템](FileSystem.md) 거치지 않고) 직접 페이징 파일로 수집한다.

RAM과 페이징 파일 간 데이터가 이동하는 [페이징](https://en.wikipedia.org/wiki/Memory_paging) 기법에 대하여 간랸히 설명한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">페이징 기법 및 설명</caption><colgroup><col style="width: 15%;"/><col style="width: 25%;"/><col style="width: 60%;"/></colgroup><thead><tr><th style="text-align: center;">페이징</th><th style="text-align: center;">전달 방향</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">페이징 아웃<br/>(Paging out)</td><td style="text-align: center;">RAM → 페이징 파일</td><td>오랜 시간 RAM에서 사용되지 않은 가상 메모리의 페이지를 페이징 파일로 옮겨 메모리 여유를 확보한다.</td></tr><tr><td style="text-align: center;">페이징 인<br/>(Paging in)</td><td style="text-align: center;">페이징 파일 → RAM</td><td>페이징 파일은 메모리 주소가 없는 관계로, CPU가 접근해야 할 가상 메모리 페이지는 RAM으로 전달된다.</td></tr></tbody></table>

페이징 파일 크기 및 구성 설정은 아래 두 방법 중 하나로 이동하여 변경할 수 있다:

![가상 메모리 다이얼로그 창](./images/memory_pagefile.png)

* Systempropertiesadvanced.exe(고급 시스템 설정 보기)의 성능 옵션 중 *가상 메모리* 변경 설정
* [레지스트리 편집기](Registry.md#레지스트리)에서 HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management 하위 키의 값 설정

여기서 "시스템이 관리하는 크기(System managed size)"란, 몇 가지의 요인들을 기반하여 [세션 관리자](Process.md#세션-관리자)가 실시간으로 적합한 페이징 파일 크기를 유연하게 결정하는 옵션이다. 만일 [커밋된 메모리](#페이지)가 커밋 한도의 90%에 도달하면 페이징 파일은 두 조건에 따라 확장한다: (1) RAM 용량 혹은 4 GB 중 가장 큰 크기의 세 배까지 늘어날 수 있지만, (2) 상주하는 드라이브 용량의 1/8 크기를 초과할 수 없다.

윈도우 OS는 기본적으로 "모든 드라이브에 대한 페이징 파일 크기 자동 관리"하도록 설정되었다; Windows가 설치된 C 드라이브만 *시스템이 관리하는 크기*이고 나머지는 *페이징 파일 없음*과 동일하다.

### 작업 집합
**[작업 집합](https://en.wikipedia.org/wiki/Working_set)**(working set)은 커밋된 가상 메모리 중 RAM에 할당된 [페이지](#페이지)들을 일컫는다. 작업 관리자의 메모리 성능 지표에서 "사용 중" 메모리는 시스템 전반의 작업 집합을 의미한다. Windows OS는 작업 집합이 일정 비율에 도달하여 RAM의 여유 메모리 확보가 필요할 경우, 오랜 기간 동안 사용되지 않은 페이지를 [페이징 파일](#페이징-파일)로 [트리밍](https://techcommunity.microsoft.com/blog/askperf/prf-memory-management-working-set-trimming/373758)(trimming)한다.

* [성능 카운터](Perfmon.md#성능-카운터): `\Process(*)\Working Set` 또는 `\Process(*)\Working Set - Private`

### 페이지 부재
**[페이지 부재](https://en.wikipedia.org/wiki/Page_fault)**(page fault)는 접근하려는 가상 메모리의 페이지가 RAM에 상주하지 않아 물리 메모리 주소로 찾아갈 수 없을 시 발생하는 [예외](C.md#예외-처리)이다. 다만, 운영체제의 메모리 관리 과정에서 흔히 일어나는 매우 자연스러운 현상이기 때문에 일반적으로는 문제로 간주되지 않는다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="text-align: center;">페이지 부재 유형</capation><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Page_fault#Minor">소프트 페이지 부재</a>(soft page fault)</th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Page_fault#Major">하드 페이지 부재</a>(hard page fault)</th></tr></thead><tbody><tr><td>접근하려는 페이지가 아직 RAM에 상주하여 상대적으로 훨씬 빨리 해결될 수 있다.</td><td>접근하려는 페이지가 RAM에 존재하지 않아 페이징 파일로부터 불러와야 한다.</td></tr></tbody></table>

그러나 RAM 용량에 비해 너무 많은 페이지가 커밋되었을 경우, [페이징](#페이징-파일) 및 페이지 부재가 과도하게 발생하는 [스래싱](https://en.wikipedia.org/wiki/Thrashing_(computer_science))(thrashing) 현상이 일어난다. [커널 모드](Processor.md#커널-모드)의 메모리 관리를 수행하는 작업 횟수가 방대해진 탓에 실질적인 [사용자 모드](Processor.md#사용자-모드)의 [프로세스](Process.md) 작업이 뒷전으로 미루어진 성능 저하를 초래한다.

## 힙
> 본 장은 [C](C.md)/[C++](Cpp.md) 프로그래밍 언어의 [동적 할당](C.md#동적-할당)을 다룬 내용으로, 동명의 [자료 구조](https://en.wikipedia.org/wiki/Heap_(data_structure))와는 아무런 관련이 없다.

**[힙](https://en.wikipedia.org/wiki/Memory_management#HEAP)**(heap)은 [프로세스](Process.md)의 [사용자 공간](Process.md#가상-주소-공간)에 해당하는 [가상 메모리](#가상-메모리) 영역 중 하나이며, 여러 작은 [메모리 블록](#메모리-블록)들을 취급하는데 특화되어 있다; [페이지](#페이지)의 할당 입도와 경계 간격을 전혀 신경쓰지 않고 힙 관리자(heap manager)에 메모리 블록의 할당과 해제를 전적으로 맡긴다. 그러나 메모리 블록의 할당 및 해제는 다른 메모리 유형에 비해 오버헤드가 있으며, 더 이상 물리 메모리상 커밋 여부를 직접 제어할 수 없다.

[프로세스](Process.md)는 초기화 과정에 시스템에 의해 한 개의 "기본 힙(default heap)"을 생성하여 다음 데이터 및 메모리 블록을 취급한다:

* CRT [malloc](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/malloc) 및 C++ [`new`](https://learn.microsoft.com/en-us/cpp/cpp/new-operator-cpp) 연산자 등의 동적 할당
* ANSI에서 유니코드 변환하기 위한 임시 버퍼
* 컴파일된 이미지 설정
* 프로세스 실행 명령
* 환경 변수
* 기타 등등

위와 같이 프로세스 실행에 필연적이기 때문에 프로세스가 종료될 때까지 제거될 수 없다. 일반적으로 1 MB 용량으로 시작하여, 부족하다면 시스템은 해당 프로세스의 기본 힙 영역을 확장시킨다. 한편, DLL은 프로세스가 아니므로 기본 힙을 가지지 않지만 Win32의 [HeapCreate](https://learn.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heapcreate) 함수를 통해 추가 힙 영역을 생성할 수 있다.

## 주소 윈도잉 확장
**[주소 윈도잉 확장](https://learn.microsoft.com/en-us/windows/win32/memory/address-windowing-extensions)**(address windowing extensions; AWE)은 [가상 메모리](#가상-메모리)를 RAM에 상주하는 비페이징 메모리에 매핑, 즉 "[윈도잉](https://en.wikipedia.org/wiki/Windowing)(windowing)"하여 사용할 수 있도록 제공된 [Win32 API](WinAPI.md) 집합이다. AWE는 아래 두 가지 성능적 특징으로 데이터 처리 비중이 높은 프로그램(대표적으로 [SQL](SQL.md))에서 자주 활용된다.

* 페이징될 수 없는 메모리를 RAM에 할당하여 [페이지 부재](#페이지-부재)로 인한 [오버헤드](https://en.wikipedia.org/wiki/Overhead_(computing))를 원천적으로 방지한다.
* 가상 메모리 테이블을 리매핑하여 [프로세스 주소 공간](Process.md#가상-주소-공간)보다 큰 RAM의 물리 메모리를 접근할 수 있다.

AWE의 구현 방식을 간단히 설명하면 순서와 같다:

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">핸들 테이블을 묘사한 예시</caption><colgroup><col style="width: 10%;"/><col style="width: 25%;"/><col/></colgroup><thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">Win32 함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc">VirtualAlloc</a></td><td>MEM_PHYSICAL 플래그를 추가하여 가상 메모리의 페이지를 예약한다.</td></tr><tr><td style="text-align: center;">2</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-allocateuserphysicalpages">AllocateUserPhysicalPages</a></td><td>사용할 RAM의 페이지 프레임을 미리 할당받아 확보한다.</td></tr><tr><td style="text-align: center;">3</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapuserphysicalpages">MapUserPhysicalPages</a></td><td>가상 메모리의 페이지와 RAM의 페이지 프레임을 매핑한다.</td></tr></tbody></table>

<sup>_† RAM에 페이지 프레임 할당 남용을 방지하기 위해, 사용자는 AWE 활용 프로그램을 실행하기 전에 반드시 "메모리에 페이지 잠금(Lock Pages In Memory)" 권한을 획득해야 한다._</sup>

오로지 윈도잉한 프로세스만 해당 RAM 페이지 프레임에 접근할 수 있기 때문에, 그 외 프로세스와 공유 불가하다.

# 메모리 맵 파일
> *참고: [File Mapping - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/memory/file-mapping)*

**[메모리 맵 파일](https://en.wikipedia.org/wiki/Memory-mapped_file)**(memory-mapped file)은 파일과 바이트 대 바이트 상관관계를 맺은 [가상 메모리](#가상-메모리)를 가리킨다. 가상 메모리가 커밋될 때 [페이징 파일](#페이징-파일)로 할당되는 거와 달리, 메모리 맵 파일은 [페이지](#페이지)를 [디스크](Storage.md#디스크)상 지정된 [파일](FileSystem.md)에 직접 할당한다. 단, 이는 메모리 매핑된 파일이 마치 [RAM](https://en.wikipedia.org/wiki/Random-access_memory)처럼 활용될 수 있음을 의미하지 않는다; 기술의 본질은 [파일 입출력](FileSystem.md#파일-입출력)이며, 파일에 커밋된 페이지 또한 마찬가지로 RAM에 상주해야 [프로세서](Processor.md)가 접근할 수 있다.

다음은 위에서 소개한 메모리 맵 파일의 특징에 의해 나타나는 성질을 나열한다:

1. 파일을 매번 읽거나 쓰기 위해 필요한 [시스템 호출](WinAPI.md#시스템-서비스) 횟수를 획기적으로 줄여 [오버헤드](https://en.wikipedia.org/wiki/Overhead_(computing))로 인한 성능 저하를 감소시킨다.
1. 이미 메모리 매핑된 파일을 불러올 경우, 기존 파일 매핑을 그대로 활용하기 때문에 가상 메모리를 더 이상 예약하지 않아 RAM을 절약한다.
1. [프로세스](Process.md) 간 데이터를 공유하는 매커니즘(RPC, COM, 윈도우 메시지, 클립보드, [메일슬롯](Process.md#메일슬롯), [파이프](Process.md#파이프), 소켓 등)은 전부 메모리 매핑 파일에 기반한다.

Windows OS에서의 메모리 맵 파일은 아래 그림과 같이 구성 및 구현된다.

![디스크의 파일, 파일 매핑 개체, 그리고 파일일 뷰의 관계도](https://learn.microsoft.com/en-us/windows/win32/memory/images/fmap.png)

* [파일](FileSystem.md)
* [파일 매핑 개체](#파일-매핑-개체)
* [파일 뷰](#파일-뷰)

### 파일 매핑 개체
**[파일 매핑 개체](https://learn.microsoft.com/en-us/windows/win32/memory/creating-a-file-mapping-object)**(file mapping object)는 파일과 물리 메모리를 서로 매핑하는, [CreateFileMapping](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga) 함수에 의해 생성되는 [커널 개체](Kernel.md#커널-개체)이다. ([페이징 파일](#페이징-파일)을 포함한) 어느 파일이라도 가능하며, 파일 전체 혹은 일부만 메모리로 매핑시킬 수 있다. 즉, 파일에 매핑할 물리 메모리의 크기 또한 지정할 수 있다. 페이지의 보호 속성이 PAGE_READWRITE 처럼 쓰기 권한을 가지면 기존 파일보다 크게 설정하는 게 허용된다.

물리 메모리가 할당된 파일 매핑 개체의 페이지도 페이징될 수 있다:

1. 페이징 아웃 시, 변경 사항은 매핑된 파일에 저장된다.
1. 페이징 인 시, 저장된 변경 사항을 매핑된 파일로부터 다시 불러온다.

해당 커널 개체는 "파일과 물리 메모리를 매핑"하는 역할에 불과하며, 프로세스에서 이를 접근하려면 [파일 뷰](#파일-뷰)가 필요하다.

### 파일 뷰
**[파일 뷰](https://learn.microsoft.com/en-us/windows/win32/memory/creating-a-file-view)**(file view)는 커널 개체인 [파일 매핑](#파일-매핑-개체)을 [프로세스](Process.md)에서 접근할 수 있도록 지원하며, [MapViewOfFile](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile) 함수는 파일 매핑의 어디서부터 얼만큼 파일 뷰가 접근할지 오프셋 및 크기를 지정한다. 단, 오프셋은 할당 입도의 배수이여야 한다. 파일 뷰는 [커널 개체](Kernel.md#커널-개체)가 아니다; Win32 함수는 [핸들](Process.md#핸들)이 아닌 [포인터](C.md#포인터)를 반환한다. 파일 뷰는 타 프로세스에게 상속되거나 전달될 수 없으며, 해당 파일 매핑로 접근이 필요하면 파일 뷰를 새로 생성해야 한다.

## Copy-on-write
**[Copy-on-write](https://learn.microsoft.com/en-us/windows/win32/memory/memory-protection)**(일명 COW) 보호는 공유 리소스를 관리하는 기법 중 하나이다. 어느 한 프로세스가 공유 리소스를 편집하려고 시도할 경우, 시스템은 해당 프로세스의 [VAS](Process.md#가상-주소-공간)에 한하여 페이지 복사본을 제공하고 변경 사항을 적용한다. 나머지 프로세스들은 아무런 영향을 받지 않으므로 메모리도 절약할 수 있다.

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
* [리눅스](https://en.wikipedia.org/wiki/Linux)의 경우, 두 개의 [캐시 라인](Processor.md#cpu-캐시)을 거쳐 처리하는 도중에 외부 간섭을 방지하기 위한 조치로 "[split lock](https://lwn.net/Articles/790464/)"이란 [원자성 연산](Processor.md#원자성-연산)을 동원한다.
