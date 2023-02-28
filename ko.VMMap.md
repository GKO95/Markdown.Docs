---
category: 마이크로소프트
title: VMMap
alias: null
visible: true
icon: sysinternals.png
---
# VMMap
[VMMap](https://learn.microsoft.com/en-us/sysinternals/downloads/vmmap)는 [프로세스](ko.Process.md)의 [가상 메모리](ko.Process.md#가상-주소-공간)를 분석하는 [Sysinternals](ko.Sysinternals.md) 유틸리티 프로그램이다.

![VMMap 유틸리티 프로그램](/images/sysinternals_vmmap.png)

> 본 프로그램을 이해하려면 [윈도우](ko.Windows.md) 운영체제를 [컴퓨터 구조론](https://ko.wikipedia.org/wiki/컴퓨터_구조) 관점에서의 개념이 확립되어야 한다.

VMMap은 선택된 단 하나의 프로세스에서 커널 공간을 제외한 가상 주소 공간, 즉 사용자 공간을 어떻게 사용하고 있는지 보여준다. 만일 64비트 프로세스라면 128 TB([윈도우 서버 2012](https://ko.wikipedia.org/wiki/윈도우_서버_2012)까지는 8 TB)의 사용자 공간이 VMMap에 표시된다. 아래는 창 상단에 위치한 세 개의 그래프가 각각 무엇을 의미하는지 설명한다.

<table style="width: 90%; margin: auto;">
<caption style="caption-side: top;">VMMap 요약 그래프에 대한 개요</caption>
<colgroup><col style="width: 25%"/><col style="width: 75%"/></colgroup>
<thead><tr><th style="text-align: center;">그래프</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><a href="ko.Memory.md#커밋된-메모리">커밋</a> 요약 그래프<br/>(Commit Summary Graph)</td><td>메모리 유형마다 프로세스의 가상 주소 공간에 할당된 커밋된 용량을 표시한다. 즉, 여유(free) 및 예약된(reserved) <a href="ko.Process.md#페이지">페이지</a>를 제외한 오로지 커밋된 페이지만이 계산된 수치이다.</td></tr>
<tr><td style="text-align: center;">개인 요약 그래프<br/>(Private Summary Graph)</td><td>커밋된 메모리 중에서 공유되지 않은 프로세스 개인만을 위한 용량을 표시한다. 성능 카운터: <code>\Process(*)\Private Bytes</code></td></tr>
<tr><td style="text-align: center;"><a href="ko.Memory.md#워킹-세트">워킹 세트</a> 요약 그래프<br/>(Working Set Summary Graph)</td><td>커밋된 메모리 중에서 <a href="ko.Memory.md">RAM</a>에 상주하고 있는 워킹 세트 용량을 표시한다. 성능 카운터: <code>\Process(*)\Working Set</code></td></tr>
</tbody>
</table>

그래프에 표시된 각 색상들은 메모리 유형들을 나타내며, 이들에 대한 수치적 정보는 그래프 아래에 위치한 두 개의 도표들로부터 확인할 수 있다.

* **요약 항목(Summary View)**: 메모리 유형마다 가상 주소 공간에서 확보된 용량을 나열한다.
* **상세 항목(Details View)**: 프로세스 가상 주소 공간에 상주하는 메모리 영역들을 순서대로 보여준다.

## 메모리 유형
다음은 VMMap에서 다루는 메모리 유형들을 소개한다.

* **여유(Free)**

    프로세스 가상 주소 공간에서 커밋으로 할당되지 않은 영역이다. 커밋 요약 그래프에는 사용 불가한(unusable)과 함께 표시되지 않는 메모리 유형이다.

* **개인(Private)**

    [윈도우 API](ko.WinAPI.md)의 [`VirtualAlloc`](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc) 함수로부터 할당되어 다른 프로세스와 공유될 수 없는 메모리이다. 커밋 한도에 의한 크기 제한이 있으며, 일반적으로 어플리케이션 데이터를 저장한다.

    > 힙 메모리 관리자 또는 [.NET](https://ko.wikipedia.org/wiki/닷넷) 런타임으로부터 할당된 메모리는 별도 메모리 유형으로 분류된다.

<table style="width: 70%; margin: auto;">
<caption style="caption-side: top;">개인 메모리 하위 유형</caption>
<colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup>
<thead><tr><th style="text-align: center;">개인 메모리</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/동적_메모리_할당#힙_영역">힙</a><br/>(Heap)</td><td><a href="https://www.ibm.com/docs/en/i/7.2?topic=memory-heap-manager">힙 관리자</a>(Heap Manager)로부터 관리되며, 흔히 <a href="https://ko.wikipedia.org/wiki/런타임_라이브러리">C 런타임 라이브러리</a>(CRT)의 <a href="https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/malloc"><code>malloc</code></a> 또는 윈도우 API의 <a href="https://learn.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heapalloc"><code>HeapAlloc</code></a> 및 <a href="https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-localalloc"><code>LocalAlloc</code></a> 등으로 할당받는다.</td></tr>
<tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/쓰레기_수집_(컴퓨터_과학)">관리된 힙</a><br/>(Managed Heap)</td><td>.NET <a href="https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/">가비지 컬렉터</a>(garbage collector)로부터 할당 및 사용되는 메모리이다.</td></tr>
<tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/콜_스택">스택</a><br/>(Stack)</td><td>개별 <a href="ko.Process.md#스레드">스레드</a>마다 함수의 매개변수, 변수, 그리고 함수의 호출이 기록되하며 필요에 따라 용량이 커질 수 있다.</td></tr>
<tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/페이지_테이블">페이지 테이블</a><br/>(Page Table)</td><td>프로세스의 페이지 정보를 관리하는 커널 모드의 워킹 세트 메모리이다.</td></tr>
</tbody>
</table>

* **공유 가능한(Shareable)**

    개인 메모리와 달리, 타 프로세스에서도 함께 사용될 수 있으며 (만일 존재한다면) 페이징 파일에 상주한다. 일반적으로 다른 프로세스에 상주하는 DLL에 공유되는 데이터 혹은 [프로세스 간 통신](https://ko.wikipedia.org/wiki/프로세스_간_통신) 메시지를 저장한다. 개인 메모리와 마찬가지로, 커밋 한도에 의해 메모리 크기가 제한받는다.

    > 윈도우 API에서는 해당 유형의 메모리를 "페이징 파일로부터 데이터 공유를 지원받는 메모리 섹션([pagefile-backed section](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/file-backed-and-page-file-backed-sections))"이라 언급한다.

    아래는 공유 가능한 메모리에 속하는 하위 유형의 메모리들을 소개한다:

<table style="width: 70%; margin: auto;">
<caption style="caption-side: top;">굥유 가능한 메모리 하위 유형</caption>
<colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup>
<thead><tr><th style="text-align: center;">공유 가능한 메모리</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/메모리_맵_파일">매핑된 파일</a><br/>(Mapped File)</td><td>디스크에 있는 파일들을 나태낸 메모리이다. 상세 항목에서 해당 파일의 경로를 확인할 수 있다.</td></tr>
</tbody>
</table>

* **이미지(Image)**

    [이미지 로더](https://www.oreilly.com/library/view/windows-internals-sixth/9780735671294/ch03s10.html)(image loader)에 의해 프로세스 가상 주소 공간에 올라간 `.EXE` 또는 `.DLL` 등의 확장자를 갖는 실행 파일이 저장되는 메모리이며, 그 안에는 프로그램 이미지 코드와 같은 공유 가능한 메모리도 포함될 수 있다. 허나, 단순히 데이터 파일로써 가상 주소 공간에 매핑된 이미지는 공유 가능한(shareable) 메모리에 속한다. 상세 항목에서 프로그램 이미지가 위치한 파일 경로를 확인할 수 있다.

    > 만일 초기화 정보 등의 일부 이미지 메모리 영역에 변동사항이 생길 시, 이미지 메모리를 수정하는 게 아니라 개인 메모리를 생성하여 수정된 내용을 기록한다.

### 문자열 메모리
간혹 메모리에 저장된 [문자열](ko.C.md#문자열)로부터 해당 메모리 영역이 어떠한 목적으로 사용되고 있는지 파악할 수 있다. VMMap은 창 메뉴에서 View > Strings 선택지(단축키: `CTRL+T`)를 클릭하여 세 개 이상의 문자로 구성된 [ASCII](https://ko.wikipedia.org/wiki/ASCII) 혹은 [유니코드](https://ko.wikipedia.org/wiki/유니코드) 문자열을 확인할 수 있다.
