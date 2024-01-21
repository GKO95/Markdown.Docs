# 메모리
**[메모리](https://en.wikipedia.org/wiki/Computer_memory)**(memory)는 시스템에서 즉각적으로 사용할 데이터를 저장하는 [하드웨어](https://en.wikipedia.org/wiki/Computer_hardware)이며, 대표적으로 [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리)이 있다. 메모리는 작업 속도가 매우 빠르며 [휘발성](https://en.wikipedia.org/wiki/Volatile_memory)인 특징을 가지고 있다. 또한 [프로세서](Processor.md)와 물리적으로 근접하여 연산시 데이터 접근 속도가 순식간이기 때문에 단기기억 역할을 담당한다. 그러므로 메모리는 시스템의 성능을 결정하는 중요한 요소로 작용한다.

> [HDD](Storage.md#디스크) 및 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브) 등의 [저장 장치](Storage.md)도 데이터를 저장하는 메모리로 분류되지만, 작업 속도가 느리고 [비휘발성](https://en.wikipedia.org/wiki/Non-volatile_random-access_memory)인 관계로 "보조 메모리"라고 부른다.

메모리의 이해를 돕기 위해 아래 [작업 관리자](https://ko.wikipedia.org/wiki/작업_관리자) 그림을 예시로 사용하여 설명한다.

![작업 관리자에서 확인한 메모리 성능 (<a href="https://ko.wikipedia.org/wiki/윈도우_11">윈도우 11</a>, 버전 22H2)](./images/memory_taskmgr.png)

48 GB의 물리 메모리(즉, RAM)가 설치된 본 시스템은 47.9 GB를 활용하며, 그 중에서 8.6 GB가 사용 중이고 나머지 39.0 GB는 [사용 가능](#사용-가능한-메모리)하다. 사용 중인 메모리에서 공간 절약이 가능하다면 압축을 하는데, 괄호 안의 38.4 MB는 결과적으로 [압축된 메모리](Process.md#시스템-프로세스) 크기를 가리킨다.

## 가상 메모리
**[가상 메모리](https://en.wikipedia.org/wiki/Virtual_memory)**(virtual memory)는 일종의 [메모리 관리](https://en.wikipedia.org/wiki/Memory_management_(operating_systems)) 기술이며, 컴퓨터에 탑재된 실제 물리 메모리와 무관하게 [운영체제](https://en.wikipedia.org/wiki/Operating_system)에 의해 표현된 "가상"의 메모리이다. 운영체제에서 실행된 ([커널](Kernel.md)을 포함한) 모든 프로그램들은 가상 메모리에서 실행되며, 가상 메모리의 [주소](#가상-주소-공간)는 프로세서에 탑재된 [MMU](Processor.md#메모리-관리-장치)에 의해 물리 메모리의 주소로 매핑되어 사용된다. 그리고 가상 메모리는 운영체제가 관리하는 [*가상 주소 공간*](Process.md#가상-주소-공간)에서 할당된다.

> 가상 주소 공간(virtual address space; VAS)은 매우 중요한 개념 중 하나이지만, 자세한 내용은 *[프로세스](Process.md)* 문서를 참고하도록 한다.

가상 메모리를 활용한 시스템은 다음과 같은 이점을 지닌다:

1. 메모리 체계(예를 들어, 공유 메모리 등)를 커널에서 관리하기 때문에 프로그램 개발의 편리
1. [페이징](#페이징-파일) 기술을 통해 물리적으로 사용할 수 있는 메모리보다 더 많은 주소 공간 확보
1. 메모리 격리에 의한 시스템 보안 강화

### 페이지
**[페이지](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page)는 가장 작은 단위의 가상 메모리 블록이며, 이와 매핑된 하나의 물리 메모리 조각을 **[페이지 프레임](https://en.wikipedia.org/wiki/Page_(computer_memory))**(page frame) 또는 간단히 **프레임**(frame)이라고 부른다. 일반적으로 페이지 (및 프레임) 크기는 4 KB로 고정된다. 페이지에는 세 가지 [상태](https://learn.microsoft.com/en-us/windows/win32/memory/page-state)가 존재하며, 이에 따라 가상 메모리의 가용 여부가 결정된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">가상 메모리의 페이지 상태</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">상태</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">여유<br/>(Free)</td><td>사용되고 있지 않는 가상 메모리이다.</td></tr><tr><td style="text-align: center;">예약됨<br/>(Reserved)</td><td>가상 주소 공간에 할당되었으나, 시스템 성능에 실질적으로 기여하지 않는 가상 메모리이다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/introduction-to-the-page-file#system-committed-memory">커밋됨</a><br/>(Committed)</td><td>가상 주소 공간에는 할당된 동시에 시스템 성능에 실질적으로 기여하는 가상 메모리이다. 단, 해당 페이지에 R/W 작업이 있기 전에는 물리 메모리가 매핑되지 않는다. "물리 메모리 + <a href="#페이징-파일">페이징 파일</a>"이 커밋 한도(commit limit)이다.</td></tr></tbody></table>

## 페이징 파일
**[페이징 파일](https://learn.microsoft.com/en-us/windows/client-management/introduction-page-file)**(page file)은 [가상 메모리](#가상-메모리)의 [페이지](Process.md#페이지)를 물리 메모리가 아닌 [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)나 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)와 같은 [저장 장치](Storage.md)로 전달받을 수 있는 pagefile.sys 파일이다. 프로그램을 실행하거나 저장된 파일을 열 때 물리 메모리에 로드되지만, 파일을 편집하였으나 아직 저장되지 않는 등의 경우에는 아예 페이징 파일에서 처리되는 경향이 있다. 물리 메모리 용량이 대폭 증가하며 페이징 파일의 입지는 예전에 비해 상당히 퇴색되었으나, 일부 프로그램이나 메모리 덤프 수집에 여전히 필요한 존재이다.

물리 메모리와 페이징 파일 간 데이터가 이동하는 [페이징](https://en.wikipedia.org/wiki/Memory_paging) 기법에 대하여 간랸히 설명한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">페이징 기법 및 설명</caption>
<colgroup><col style="width: 15%;"/><col style="width: 25%;"/><col style="width: 60%;"/></colgroup><thead><tr><th style="text-align: center;">메모리 페이징</th><th style="text-align: center;">방향성</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">페이징 아웃<br/>(Paging out)</td><td style="text-align: center;">물리 메모리 → 페이징 파일</td><td>오랜 시간 물리 메모리에서 사용되지 않은 페이지를 저장 장치의 페이징 파일로 옮겨 메모리 여유를 확보한다.</td></tr><tr><td style="text-align: center;">페이징 인<br/>(Paging in)</td><td style="text-align: center;">페이징 파일 → 물리 메모리</td><td>페이징 파일은 저장 장치의 기술적 한계로 물리 메모리를 대체할 수 없으므로, 참조되어야 할 페이지는 물리 메모리로 복귀된다.</td></tr></tbody></table>

윈도우 OS는 페이징 파일 크기를 설정할 수 있으며 View advanced system settings 검색 (혹은 `systempropertiesadvanced.exe` 실행) 이후에 *Performance* 그룹의 설정 버튼을 클릭한다. Performance Options 창이 나타나면 Advanced 탭으로 이동하여 *Virtual memory* 그룹 하에 변경 버튼을 클릭한다.

![가상 메모리 다이얼로그 창](./images/memory_pagefile.png)

> 시스템은 기본적으로 "Automatically manage paging file size for all drives" 체크 박스가 설정되어 있으며, 이는 OS 드라이브만 *시스템이 관리하는 크기*인 한편 나머지는 *페이징 파일 없음*과 동일하다.

다음은 각 드라이브마다 설정할 수 있는 페이징 파일 크기에 대한 세 가지 선택지를 소개한다:

1. **사용자 지정 크기**(Custom size)

    직접 페이징 파일의 처음 크기(Initial size)와 확장될 수 있는 최대 크기(Maximum size)를 [메가바이트](https://en.wikipedia.org/wiki/Megabyte) 단위로 지정한다. 단, 지정한 크기만큼 저장 장치의 여유 공간이 줄어든다는 점은 충분히 인지하고 변경하도록 한다.

1. **시스템이 관리하는 크기**(System managed size)

    [세션 관리자](Process.md#세션-관리자)는 몇 가지의 요인들을 기반하여 실시간으로 적합한 페이징 파일 크기를 유연하게 결정한다. 만일 시스템 메모리가 커밋 한도의 90%에 도달하면 페이징 파일의 크기가 확장되는데, 물리 메모리(혹은 4 GB 중 가장 큰 걸로 선정)의 세 배까지 늘어날 수 있다. 하지만 페이징 파일이 확장하여도 상주하는 드라이브 용량의 1/8로 크기가 제한된다.

    [커널](Dump.md#커널-메모리-덤프), [전체](Dump.md#전체-메모리-덤프), 그리고 [활성 메모리 덤프](Dump.md#활성-메모리-덤프)로 구성되었으면 [BSOD](BSOD.md) 발생 시 덤프를 모두 수집할 수 있도록 페이징 파일 크기를 물리 메모리와 동일하게 조정한다. [자동 메모리 덤프](Dump.md#자동-메모리-덤프)일 경우, 커널 주소 공간을 수집하는 데 일반적으로 충분하다고 판단되는 크기로 조정한다. 반면 불충분하다면 `PagefileTooSmall` 레지스트리 서브키를 생성하는데, 해당 레지스트리 값의 존재 여부에 따라 [부팅](Boot.md) 때 페이징 파일 크기를 물리 메모리와 동일하게 설정한다.

3. **페이징 파일 없음**(No paging file)

## 워킹 세트
[워킹 세트](https://en.wikipedia.org/wiki/Working_set)(working set)는 프로세스의 사용자 및 커널 공간을 불문하고 [가상 주소 공간](Process.md#가상-주소-공간) 전체에 [커밋된 메모리](#커밋된-메모리)(= 개인 메모리 + 공유 메모리) 중에서 오로지 RAM에만 상주하고 있는 페이지를 가리킨다.

> 작업 관리자에서 사용 중(in use)인 RAM 크기에 대응하지만, `\Process(_Total)\Working Set` 카운터는 공유 메모리가 중복 계산되어 더 크게 측정된 점을 유의하도록 한다.

운영체제는 건전한 시스템 상태를 유지하기 위해, 워킹 세트가 특정 크기에 도달하면 오랜 기간동안 사용되지 않은 메모리를 페이징 아웃시키는 트리밍(trimming) 작업을 진행한다. [메모리 관리자](https://ko.wikipedia.org/wiki/메모리_관리_장치)는 프로세스 혹은 [스레드](Process.md#스레드)에 주어진 [메모리 우선순위](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/ns-processthreadsapi-memory_priority_information)에 따라 낮은 순위부터 트리밍한다. 트리밍은 실행 중인 모든 프로세스에 거쳐 처리되는데, 이는 권한이 매우 높은 작업으로 트리밍이 전부 끝날 때까지 기다려야 한다.

### 페이지 부재
[페이지 부재](https://ko.wikipedia.org/wiki/페이지_부재)(page fault)는 프로세스의 페이지를 접근하려 하나 워킹 세트에 존재하지 않는 경우를 가리키며, 운영체제가 메모리를 관리하는 과정에서 일어나는 매우 자연스러운 현상이다. 페이지 부재는 하드웨어적 그리고 소프트웨어적 페이지 부재로 나뉘어진다.

* **하드 페이지 부재**: 일명 *메이저 페이지 부재*; 접근하려는 데이터가 보조기억장치의 페이징 파일로 상주하는 경우
* **소프트 페이지 부재**: 일명 *마이너 페이지 부재*; 접근하려는 데이터가 물리 메모리에 상주하나 [모종의 이유](#캐시-메모리)로 본래와 다른 곳에 위치하는 경우

하드 페이지 부재는 페이징 작업이 필요한 반면, 소프트 페이지 부재는 물리 메모리 내에서 처리되기 때문에 상대적으로 훨씬 빨리 해결될 수 있다.

## 사용 가능한 메모리
RAM 중에서 사용 중인 영역이 있으면 이와 반대로 사용 가능한 메모리(available memory) 영역도 존재한다. 사용 가능한 메모리는 작업 관리자에 표시된 메모리 구성(memory composition), [리소스 모니터](https://en.wikipedia.org/wiki/Resource_Monitor), 혹은 Sysinternals의 [RAMMap](RAMMap.md) 유틸리티 프로그램 등으로 확인할 수 있다.

![리소스 모니터에 표시된 프로세스 및 실제 RAM의 메모리 구성별 사용량](./images/memory_resmon.png)

커밋 총량에 비해 사용 가능한 메모리가 다소 여유로울 수 있는데, 비록 페이지가 커밋되었다 하여도 곧바로 물리 메모리를 할당받는 게 아니기 때문이다. 본 부문에서는 페이지 리스트(page list)란 용어가 자주 언급되는 데, 이는 RAM에 상주하는 페이지들의 묶음을 가리킨다.

> 결론부터 말하자면, 사용 가능한 메모리는 "[여유 메모리](#여유-메모리) (free memory) + [대기 페이지 리스트](#캐시-메모리) (standby page list)"로 계산된다.

### 여유 메모리
필요로 하는 프로세스의 워킹 세트로 메모리를 제공할 수 있는 페이지 리스트들을 지칭한다.

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">여유 메모리 유형 및 설명</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">페이지 리스트</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">영값 리스트<br/>(zero page list)</td><td>전부 영(0)으로 채워져 데이터가 없는 페이지들의 리스트이다. 휘발성의 RAM은 시스템이 부팅되기 직전에 아무런 데이터가 없으므로 모든 페이지가 영값 리스트에 해당한다.</td></tr><tr><td style="text-align: center;">해제 리스트<br/>(free page list)</td><td>종료된 프로세스의 워킹 세트로부터 해제된 페이지들의 리스트이다. 워킹 세트로 있을 당시 데이터를 여전히 갖고 있으나, 차후 영값 페이지 스레드(zero page thread)에 의해 페이지는 전부 영으로 채워져 데이터가 말소되고 영값 페이지 리스트로 이전된다. 만일 영값 페이지가 고갈되면 해제 페이지 리스트로부터 제공받지만, 우선 커널로부터 데이터가 정화되어야 하므로 시간이 다소 소모된다.</td></tr></tbody></table>

### 캐시 메모리
워킹 세트의 트리밍 과정에서 처리되는 RAM 페이지들을 임시로 모아둔 페이지 리스트들을 지칭한다. RAM이 디스크의 [캐시](https://ko.wikipedia.org/wiki/캐시) 역할을 하므로써, [페이지 부재](#페이지-부재)를 메이저에서 마이너로 대체하여 디스크 입출력 작업을 완화하는 효과를 가져온다. 캐시 메모리를 이해하기 위해서 디스크에 데이터가 존재하는지 여부에 따라 RAM 혹은 페이징 파일 중 어디서 처리되는지 특성을 파악하고 있어야 한다.

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">캐시 메모리 유형 및 설명</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">페이지 리스트</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">대기 페이지 리스트<br/>(standby page list)</td><td>트리밍되어 현재 사용 중이지 않는 페이지들의 리스트이다. 데이터를 말소하여 영값 페이지로 전환시킬 수 있으나, 그 전에 해당 데이터가 다시 필요하다면 곧바로 워킹 세트로 복귀될 수 있는 "대기" 상태이다. 대기 리스트에 속한 페이지로는 다음 유형들이 포함된다:<ol><li>디스크에 이미 존재하는 데이터를 담고 있는 페이지</li><li>트리밍되기 전에 이미 영으로 채워져 데이터 말소가 불필요한 영값 페이지</li></ol></td></tr><tr><td style="text-align: center;">수정된 페이지 리스트<br/>(modified page list)</td><td>트리밍된 페이지 중에서 디스크에 저장이 필요한 페이지들의 리스트이다. 수정된 페이지는 사용 가능한 메모리가 아니지만, 시스템에 의해 데이터가 페이징 파일에 저장된 이후에는 대기 페이지 리스트로 이전된다. 대기 리스트에 속한 페이지로는 다음 유형들이 포함된다:<ol><li>수정된 리스트에 속한 페이지로는 새로 생성되거나 기존 파일로부터 수정되어 디스크에 찾아볼 수 없는 데이터를 담고 있는 페이지</li></ol></td></tr></tbody></table>

## 메모리 풀
윈도우 NT 운영체제에서 [메모리 풀](https://learn.microsoft.com/en-us/windows/win32/memory/memory-pools)(memory pools)은 [커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅)) 혹은 [장치 드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-)에서 시스템 공간에 할당되고 관리되는 커널 [힙](https://ko.wikipedia.org/wiki/동적_메모리_할당#힙_영역) 메모리이다.

* **페이징 풀**(paged pool): 페이징 파일로 이동될 수 있는 커널 메모리이다.
* **비페이징 풀**(nonpaged pool): 페이징 파일로 이동될 수 없는 커널 메모리이다.

운영체제 및 장치 드라이버는 [`ExAllocatePoolWithTag`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-exallocatepoolwithtag) 루틴에 의해 할당 당시 4바이트 크기의 태그가 지정되는데, 이를 통해 해당 메모리를 할당한 드라이버 및 목적을 파악할 수 있다. 태그 목록은 [`pooltags.txt`](./references/pooltag.txt) 파일에서 찾아볼 수 있다.

컴퓨터 과학에서 언급하는 "[메모리 풀](https://ko.wikipedia.org/wiki/메모리_풀)"과 동일한 개념으로 페이징 및 비페이징 풀로부터 할당받을 수 있는 총 커널 메모리 크기는 한정되어 있다. 운영체제와 아키텍처에 따라 한정된 용량은 상이하는 데, 64비트 NT 10 (윈도우 10 & 11, 서버 2016 등) 경우에는 각각 16 TB 그리고 RAM과 동일하거나 혹은 16 GB 중에서 가장 작은 크기로 선정된다.

> 메모리 풀의 용량 한도를 확인할 수 있는 도구로 Sysinternals의 [프로세스 탐색기](Process_Explorer.md)(Process Explorer) 유틸리티 프로그램을 사용할 수 있다.

![프로세스 탐색기로 확인된 페이징 및 비페이징 풀의 사용량과 한도](./images/sysinternals_procexp_memory.png)

### Driver Locked 메모리
[운영체제 구성요소](Windows.md#운영체제-구성요소) 또는 장치 드라이버에서 직접적으로 관리되는 페이징될 수 없는 메모리 영역이다. 문맥상 비페이징 풀과 유사하지만, 중요한 데이터의 빠른 접근성을 위해 장치 드라이버에 특별히 최적화되어 상대적으로 빠른 대신 더 많은 리소스가 요구된다. [하이퍼-V](ko.HyperV.md) 또는 [VMware](https://www.vmware.com/)와 같은 하이퍼바이저의 가상 머신에 배정한 RAM 메모리가 호스트 머신에서는 Driver Locked 메모리로 할당된다.

# 같이 보기
* [Pushing the Limits of Windows: Physical Memory](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-physical-memory/ba-p/723674)
* [Pushing the Limits of Windows: Paged and Nonpaged Pool](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-paged-and-nonpaged-pool/ba-p/723789)
