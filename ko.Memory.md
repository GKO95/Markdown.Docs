---
category: 윈도우
title: 메모리
alias: memory
visible: true
icon: windows.svg
---
# 메모리
[주기억장치](https://ko.wikipedia.org/wiki/주기억장치)(primary storage), 일명 메모리(memory)는 시스템에서 즉각적으로 사용할 데이터를 저장하는 하드웨어로 대표적인 예시가 [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리)이다. 데이터를 저장하는 또 다른 하드웨어로 [HDD](https://ko.wikipedia.org/wiki/하드_디스크_드라이브) 및 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브), [CD](https://ko.wikipedia.org/wiki/콤팩트_디스크) 또는 [DVD](https://ko.wikipedia.org/wiki/DVD), [플래시 메모리](https://ko.wikipedia.org/wiki/플래시_메모리)와 같은 [보조기억장치](ko.Storage.md)(secondaty storage)가 존재하나, 이 둘은 확연한 차이점을 지닌다.

<table style="table-layout: fixed; width: 60%; margin: auto;">
<caption style="caption-side: top;">주 및 보조기억장치의 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">주기억장치</th><th style="text-align: center;">보조기억장치</th></tr></thead>
<tbody style="text-align: center;">
<tr><td>컴퓨터 메모리</td><td>디스크 및 드라이브</td></tr>
<tr><td>동작속도가 매우 빠르다.</td><td>동작속도가 상대적으로 느리다.</td></tr>
<tr><td>물리적으로 CPU와 가까이 위치한다.</td><td>물리적으로 CPU와 멀리 위치한다.</td></tr>
<tr><td>대용량 제작이 어렵고 비싸다.</td><td>대용량 제작이 용이하고 저렴하다.</td></tr>
<tr><td><a href="https://ko.wikipedia.org/wiki/휘발성_메모리">휘발성</a>이다.</td><td><a href="https://ko.wikipedia.org/wiki/비휘발성_메모리">비휘발성</a>이다.</td></tr>
</tbody>
</table>

[CPU](ko.Processor.md)와 물리적으로 근접한 점과 빠른 데이터 접근속도는 순식간에 연산을 할 수 있도록 보조하기에 적합한 단기기억 역할을 담당한다. 하드웨어는 지속적으로 발전하고 있으나, 일반적으로 메모리와 디스크는 대략 100,000 배의 속도 차이를 갖는다. 그러므로 시스템의 성능을 결정하는 요소로 메모리가 함께 언급된다.

본문을 진행하기 전에 [가상 주소 공간](ko.Process.md#가상-주소-공간)을 읽을 것을 권장하며, 메모리의 이해를 돕기 위해 아래 [작업 관리자](https://ko.wikipedia.org/wiki/작업_관리자) 그림을 예시로 사용하여 설명한다.

![작업 관리자에서 확인한 메모리 성능 (<a href="https://ko.wikipedia.org/wiki/윈도우_11">윈도우 11</a>, 버전 22H2)](./images/memory_taskmgr.png)

설치된 48 GB의 RAM 중에서 본 시스템은 47.9 GB를 활용하여, 8.6 GB가 사용 중(in use)이고 나머지 39.0 GB는 아직 [사용 가능](#사용-가능한-메모리)(available)하다. 괄호 안에 있는 38.4 MB는 사용 중인 RAM 내에서 압축된 메모리 크기를 가리키는데, 예를 들어 본래 100 MB 데이터를 61.6 MB만큼 절약한 것이다.

## 페이징 파일
[페이징 파일](https://learn.microsoft.com/en-us/windows/client-management/introduction-page-file)(paging file)은 가상 주소 공간의 [페이지](ko.Process.md#페이지)를 RAM이 아닌 HDD 또는 SSD와 같은 보조기억장치에서 물리 메모리의 데이터 일부를 [페이징](https://ko.wikipedia.org/wiki/페이징)(paging) 기법으로 전달받아 원활한 시스템 성능을 유지하는데 기여하는 `pagefile.sys` 파일이다. 다음은 페이징 기법에 대한 간략한 설명이다.

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">페이징 기법 및 설명</caption>
<colgroup><col style="width: 15%;"/><col style="width: 30%;"/><col style="width: 55%;"/></colgroup>
<thead><tr><th style="text-align: center;">페이징 기법</th><th style="text-align: center;">방향성</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;">페이징 아웃<br/>(Paging out)</td><td style="text-align: center;">페이지 프레임 → 페이징 파일</td><td>물리 메모리에 오랜 시간동안 머물고 있으나 사용 중이지 않은 커밋된 페이지를 드라이브로 옮겨 메모리 여유를 확보한다.</td></tr>
<tr><td style="text-align: center;">페이징 인<br/>(Paging in)</td><td style="text-align: center;">페이징 파일 → 페이지 프레임</td><td>페이징 파일은 드라이브의 하드웨어적 한계로 인해 절대 물리 메모리를 대체할 수 없어, 참조되어야 할 페이지는 물리 메모리로 복귀되어야 한다.</td></tr>
</tbody></table>

[커밋된 메모리](#커밋된-메모리) 중 보조기억장치에서 찾아볼 수 없는 데이터 또한 페이징 파일에서 처리되는 걸 경향이 있다(예. 저장되지 않은 [메모장](https://ko.wikipedia.org/wiki/메모장_(소프트웨어)) 텍스트). 이와 반대로 `notepad.exe` 프로그램 이미지 혹은 저장된 `.txt` 파일 등과 같이 보조기억장치에 찾을 수 있는 데이터는 물리 메모리에서 곧바로 불러온다.

페이징 파일 크기를 지정하려면 View advanced system settings 검색 (혹은 `systempropertiesadvanced.exe` 실행) 이후, "성능(Performance)" 그룹 내의 설정 버튼을 클릭한다. "성능 옵션(Performance Options)" 창이 나타나면 고급(Advanced) 탭으로 이동하여 가상 메모리 설정 버튼을 클릭한다.

![가상 메모리 다이얼로그 창](./images/memory_pagefile.png)

기본적으로 시스템은 "모든 드라이브에 대한 페이징 파일 크기 자동 관리(Automatically manage paging file size for all drives)"로 설정되어 있다. 이는 OS 드라이브만 크기가 자동 조정되는 페이징 파일을 가지며, 나머지 드라이브에는 페이징 파일이 없는 것과 마찬가지이다. 각 드라이브마다 페이징 파일의 크기는 아래 세 가지 선택지로부터 지정된다.

* **사용자 지정 크기(Custom size)**

    사용자가 직접 페이징 파일의 처음 크기(Initial size) 및 확장될 수 있는 최대 크기(Maximum size)를 [메가바이트](https://ko.wikipedia.org/wiki/메가바이트) 단위로 지정한다 (참고: 1 [GB](https://ko.wikipedia.org/wiki/기가바이트) = 1024 MB).

* **시스템이 관리하는 크기(System manged size)**

    [세션 관리자 서브시스템](https://ko.wikipedia.org/wiki/세션_관리자_하위_시스템) `smss.exe`에 의해 페이징 파일의 크기가 결정된다. 특히 [자동 메모리 덤프](ko.Dump.md#자동-메모리-덤프)와 시너지를 발휘하여 페이징 파일 크기를 효율적으로 관리하되 상황에 따라 크기를 유연하게 확장할 수 있다:

    1. 커밋된 메모리 사용률이 90%에 도달하면 시스템은 페이징 파일을 RAM의 3배(최대 16 GB)까지 확장할 수 있다. 예를 들어, 4 GB와 8 GB 메모리의 시스템은 각각 페이징 파일이 12 GB와 16 GB까지 늘어날 수 있다. 그러나 페이징 파일이 확장될 수 있는 크기는 해당 드라이브 용량의 1/8로 제한된다.
    
    1. 시스템 충돌로 [BSOD](ko.BSOD.md)가 발생하면 RAM 데이터를 페이징 파일로 [덤핑](ko.BSOD.md#bsod-덤프-수집)하는 데, 만일 페이징 파일 크기가 부족하다면 페이징 파일 크기를 점차적으로 늘린다. 그러나 [커널](ko.Dump.md#커널-메모리-덤프), [전체](ko.Dump.md#전체-메모리-덤프), 그리고 [활성 메모리 덤프](ko.Dump.md#활성-메모리-덤프)로 설정하였다면 페이징 파일은 애초 부팅 당시부터 RAM과 동일한 크기로 증설된다.

* **페이징 파일 없음(No paging file)**

    페이징 파일을 사용하지 않는다.

지정된 크기만큼의 보조기억장치 용량을 페이징 파일로 사용하기 때문에 저장공간이 줄어든다는 단점이 있다. 그리고 물리 메모리의 성능 및 용량이 대폭 발전되고 운영체제의 아키텍처가 64비트로 전환하면서 페이징 파일의 역할이 상당히 퇴색되었다.

## 커밋된 메모리
커밋된 메모리(committed memory)는 시스템 메모리에서 사용 중으로 인식된 [페이지](ko.Process.md#페이지)이다. [프로세스](ko.Process.md)의 사용자 공간에 커밋된 메모리는 두 유형으로 나뉘어진다.

<table style="table-layout: fixed; width: 60%; margin: auto;">
<caption style="caption-side: top;">개인 및 공유 메모리의 차이점</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">개인 메모리 (private memory)</th><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/공유_메모리">공유 메모리</a> (shared memory)</th></tr></thead>
<tbody style="text-align: center;"><tr>
<td>오로지 해당 프로세스만 접근할 수 있는 메모리이다.</td>
<td>타 프로세스와 공유되는 메모리이며, 대표적으로 <code>.EXE</code> 혹은 <code>.DLL</code>와 같은 프로그램 이미지는 하나만으로 여러 프로세스가 공유한다.</td>
</tr></tbody>
</table>

시스템 관점에서 바라본 커밋된 메모리, 즉 커밋 총량(commit charge)은 모든 프로세스의 사용자 공간 및 커널로부터 커밋된 페이지의 합계이다. 커밋 총량이 도달할 수 있는 최대 크기인 커밋 한도(commit limit)는 페이지가 상주할 수 있는 "RAM + [페이징 파일](#페이징-파일)"로 계산된다. 커밋 총량이 한도에 도달할 시, 시스템은 여유 메모리가 생길 때까지 기다려야 하는 [응답 없음](https://ko.wikipedia.org/wiki/프리징_(컴퓨팅))(hang) 상태에 빠진다.

> 작업 관리자 예시에서 커밋 총량과 한도가 각각 12.5 GB 및 54.9 GB로 측정되었다. 페이징 파일을 계산하면 총 7.0 GB(= 54.9 - 47.9) 중에서 3.9 GB(= 12.5 - 8.6)가 사용되고 있다.

### 가상 메모리
가상 메모리(virtual memory)는 프로세스의 사용자 공간에 할당된 "커밋된 페이지 + 예약된 페이지(reserved pages)"이다. 시스템 아키텍처 및 운영체제에 따라 각 프로세스는 가상 주소 공간에 할당할 수 있는 최대 크기가 정해져 있는데, 이를 초과하면 오류 코드 0xC000012D와 함께 프로세스가 충돌하여 종료된다. 그러므로 단일 프로세스에 페이지를 얼마나 더 할당할 수 있는지 가상 메모리를 통해 알아볼 수 있다.

허나, 가상 메모리는 시스템 성능을 확인하는데 실질적으로 유용한 척도가 아니다:

![대략 12 GB로 측정된 커밋된 메모리, 그리고 471 TB로 측정된 총 가상 메모리](./images/memory_virtual_committed.png)

1. 예약된 페이지는 시스템 메모리와 아무런 상관이 없으므로, 시스템 관점에서는 무의미한 정보이다. 
2. 64비트 아키텍처부터 프로세스의 사용자 공간은 최대 128 TB라는 엄청난 크기로 확장하여, [메모리 누수](https://ko.wikipedia.org/wiki/메모리_누수)가 일어나지 않는 한 용량이 부족할 일이 거의 없다.

> 가상 메모리에 대한 정보는 작업 관리자에서도 찾아볼 수 없으며, 그 대신 [성능 모니터](ko.Performance_Monitor.md) 혹은 [Sysinternals](ko.Sysinternals.md)의 [VMMap](ko.VMMap.md) 유틸리티 프로그램 등으로 확인할 수 있다.

## 워킹 세트
[워킹 세트](https://en.wikipedia.org/wiki/Working_set)(working set)는 프로세스의 사용자 및 커널 공간을 불문하고 [가상 주소 공간](ko.Process.md#가상-주소-공간) 전체에 [커밋된 메모리](#커밋된-메모리)(= 개인 메모리 + 공유 메모리) 중에서 오로지 RAM에만 상주하고 있는 페이지를 가리킨다.

> 작업 관리자에서 사용 중(in use)인 RAM 크기에 대응하지만, `\Process(_Total)\Working Set` 카운터는 공유 메모리가 중복 계산되어 더 크게 측정된 점을 유의하도록 한다.

운영체제는 건전한 시스템 상태를 유지하기 위해, 워킹 세트가 특정 크기에 도달하면 오랜 기간동안 사용되지 않은 메모리를 페이징 아웃시키는 트리밍(trimming) 작업을 진행한다. [메모리 관리자](https://ko.wikipedia.org/wiki/메모리_관리_장치)는 프로세스 혹은 [스레드](ko.Process.md#스레드)에 주어진 [메모리 우선순위](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/ns-processthreadsapi-memory_priority_information)에 따라 낮은 순위부터 트리밍한다. 트리밍은 실행 중인 모든 프로세스에 거쳐 처리되는데, 이는 권한이 매우 높은 작업으로 트리밍이 전부 끝날 때까지 기다려야 한다.

### 페이지 부재
[페이지 부재](https://ko.wikipedia.org/wiki/페이지_부재)(page fault)는 프로세스의 페이지를 접근하려 하나 워킹 세트에 존재하지 않는 경우를 가리키며, 운영체제가 메모리를 관리하는 과정에서 일어나는 매우 자연스러운 현상이다. 페이지 부재는 하드웨어적 그리고 소프트웨어적 페이지 부재로 나뉘어진다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">하드 및 소프트 페이지 부재</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">하드 (혹은 메이저) 페이지 부재</th><th style="text-align: center;">소프트 (혹은 마이너) 페이지 부재</th></tr></thead>
<tbody style="text-align: center;"><tr>
<td>접근하려는 데이터가 보조기억장치의 페이징 파일로 상주하는 경우</td>
<td>접근하려는 데이터가 물리 메모리에 상주하나 <a href="#캐시-메모리">모종의 이유</a>로 본래와 다른 곳에 위치하는 경우</td></tr></tbody>
</table>

하드 페이지 부재는 페이징 작업이 필요한 반면, 소프트 페이지 부재는 물리 메모리 내에서 처리되기 때문에 상대적으로 훨씬 빨리 해결될 수 있다.

## 사용 가능한 메모리
RAM 중에서 사용 중인 영역이 있으면 이와 반대로 사용 가능한 메모리(available memory) 영역도 존재한다. 사용 가능한 메모리는 작업 관리자에 표시된 메모리 구성(memory composition), [리소스 모니터](https://en.wikipedia.org/wiki/Resource_Monitor), 혹은 Sysinternals의 [RAMMap](ko.RAMMap.md) 유틸리티 프로그램 등으로 확인할 수 있다.

![리소스 모니터에 표시된 프로세스 및 실제 RAM의 메모리 구성별 사용량](./images/memory_resmon.png)

커밋 총량에 비해 사용 가능한 메모리가 다소 여유로울 수 있는데, 비록 페이지가 커밋되었다 하여도 곧바로 물리 메모리를 할당받는 게 아니기 때문이다. 본 부문에서는 페이지 리스트(page list)란 용어가 자주 언급되는 데, 이는 RAM에 상주하는 페이지들의 묶음을 가리킨다.

> 결론부터 말하자면, 사용 가능한 메모리는 "[여유 메모리](#여유-메모리) (free memory) + [대기 페이지 리스트](#캐시-메모리) (standby page list)"로 계산된다.

### 여유 메모리
필요로 하는 프로세스의 워킹 세트로 메모리를 제공할 수 있는 페이지 리스트들을 지칭한다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">여유 메모리 유형 및 설명</caption>
<colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup>
<thead><tr><th style="text-align: center;">페이지 리스트</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;">영값 리스트<br/>(zero page list)</td><td>전부 영(0)으로 채워져 데이터가 없는 페이지들의 리스트이다. 휘발성의 RAM은 시스템이 부팅되기 직전에 아무런 데이터가 없으므로 모든 페이지가 영값 리스트에 해당한다.</td></tr>
<tr><td style="text-align: center;">해제 리스트<br/>(free page list)</td><td>종료된 프로세스의 워킹 세트로부터 해제된 페이지들의 리스트이다. 워킹 세트로 있을 당시 데이터를 여전히 갖고 있으나, 차후 영값 페이지 스레드(zero page thread)에 의해 페이지는 전부 영으로 채워져 데이터가 말소되고 영값 페이지 리스트로 이전된다. 만일 영값 페이지가 고갈되면 해제 페이지 리스트로부터 제공받지만, 우선 커널로부터 데이터가 정화되어야 하므로 시간이 다소 소모된다.</td></tr></tbody>
</table>

### 캐시 메모리
워킹 세트의 트리밍 과정에서 처리되는 RAM 페이지들을 임시로 모아둔 페이지 리스트들을 지칭한다. RAM이 디스크의 [캐시](https://ko.wikipedia.org/wiki/캐시) 역할을 하므로써, [페이지 부재](#페이지-부재)를 메이저에서 마이너로 대체하여 디스크 입출력 작업을 완화하는 효과를 가져온다. 캐시 메모리를 이해하기 위해서 디스크에 데이터가 존재하는지 여부에 따라 RAM 혹은 페이징 파일 중 어디서 처리되는지 특성을 파악하고 있어야 한다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">캐시 메모리 유형 및 설명</caption>
<colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup>
<thead><tr><th style="text-align: center;">페이지 리스트</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;">대기 페이지 리스트<br/>(standby page list)</td><td>트리밍되어 현재 사용 중이지 않는 페이지들의 리스트이다. 데이터를 말소하여 영값 페이지로 전환시킬 수 있으나, 그 전에 해당 데이터가 다시 필요하다면 곧바로 워킹 세트로 복귀될 수 있는 "대기" 상태이다. 대기 리스트에 속한 페이지로는 다음 유형들이 포함된다:<ol><li>디스크에 이미 존재하는 데이터를 담고 있는 페이지</li><li>트리밍되기 전에 이미 영으로 채워져 데이터 말소가 불필요한 영값 페이지</li></ol></td></tr>
<tr><td style="text-align: center;">수정된 페이지 리스트<br/>(modified page list)</td><td>트리밍된 페이지 중에서 디스크에 저장이 필요한 페이지들의 리스트이다. 수정된 페이지는 사용 가능한 메모리가 아니지만, 시스템에 의해 데이터가 페이징 파일에 저장된 이후에는 대기 페이지 리스트로 이전된다. 대기 리스트에 속한 페이지로는 다음 유형들이 포함된다:<ol><li>수정된 리스트에 속한 페이지로는 새로 생성되거나 기존 파일로부터 수정되어 디스크에 찾아볼 수 없는 데이터를 담고 있는 페이지</li></ol></td></tr></tbody>
</table>

## 메모리 풀
윈도우 NT 운영체제에서 [메모리 풀](https://learn.microsoft.com/en-us/windows/win32/memory/memory-pools)(memory pools)은 [커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅)) 혹은 [장치 드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-)에서 시스템 공간에 할당되고 관리되는 커널 [힙](https://ko.wikipedia.org/wiki/동적_메모리_할당#힙_영역) 메모리이다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">페이징 및 비페이징 풀</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">페이징 풀(paged pool)</th><th style="text-align: center;">비페이징 풀(nonpaged pool)</th></tr></thead>
<tbody style="text-align: center;"><tr><td>페이징 파일로 이동될 수 있는 커널 메모리이다.</td><td>페이징 파일로 이동될 수 없는 커널 메모리이다.</td></tr></tbody>
</table>

운영체제 및 장치 드라이버는 [`ExAllocatePoolWithTag`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-exallocatepoolwithtag) 루틴에 의해 할당 당시 4바이트 크기의 태그가 지정되는데, 이를 통해 해당 메모리를 할당한 드라이버 및 목적을 파악할 수 있다. 태그 목록은 [`pooltags.txt`](./references/pooltag.txt) 파일에서 찾아볼 수 있다.

컴퓨터 과학에서 언급하는 "[메모리 풀](https://ko.wikipedia.org/wiki/메모리_풀)"과 동일한 개념으로 페이징 및 비페이징 풀로부터 할당받을 수 있는 총 커널 메모리 크기는 한정되어 있다. 운영체제와 아키텍처에 따라 한정된 용량은 상이하는 데, 64비트 NT 10 (윈도우 10 & 11, 서버 2016 등) 경우에는 각각 16 TB 그리고 RAM과 동일하거나 혹은 16 GB 중에서 가장 작은 크기로 선정된다.

> 메모리 풀의 용량 한도를 확인할 수 있는 도구로 Sysinternals의 [프로세스 탐색기](ko.Process_Explorer.md)(Process Explorer) 유틸리티 프로그램을 사용할 수 있다.

![프로세스 탐색기로 확인된 페이징 및 비페이징 풀의 사용량과 한도](./images/sysinternals_procexp_memory.png)

### Driver Locked 메모리
[운영체제 구성요소](ko.WindowsNT.md#운영체제-구성요소) 또는 장치 드라이버에서 직접적으로 관리되는 페이징될 수 없는 메모리 영역이다. 문맥상 비페이징 풀과 유사하지만, 중요한 데이터의 빠른 접근성을 위해 장치 드라이버에 특별히 최적화되어 상대적으로 빠른 대신 더 많은 리소스가 요구된다. [하이퍼-V](ko.HyperV.md) 또는 [VMware](https://www.vmware.com/)와 같은 하이퍼바이저의 가상 머신에 배정한 RAM 메모리가 호스트 머신에서는 Driver Locked 메모리로 할당된다.

# 같이 보기
* [Pushing the Limits of Windows: Physical Memory](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-physical-memory/ba-p/723674)
* [Pushing the Limits of Windows: Paged and Nonpaged Pool](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-paged-and-nonpaged-pool/ba-p/723789)
