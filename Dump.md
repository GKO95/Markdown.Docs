# 사용자 모드 덤프
[사용자 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)(user-mode dump), 일명 어플리케이션 덤프는 충돌이 발생하는 등 특정 시점에서 [프로세스](Process.md)의 [가상 주소 공간](Process.md#가상-주소-공간) 중 [사용자 공간](Processor.md#권한-수준)에 포함된 정보가 수집된 파일이다. 사용자 모드 덤프를 통해 당시 어플리케이션이 어떠한 작업을 수행하고 있었는지 파악할 수 있으나, [커널](Kernel.md#커널)측 정보는 살펴볼 수 없다.

아래 프로그램들을 사용자 모드 덤프를 수집하는 데 사용된다:

<ul><li><dl><b>시스템 기본 프로그램</b><ul><li><a href="WER.md">Windows Error Reporting</a></li><li><a href="https://ko.wikipedia.org/wiki/작업_관리자">작업 관리자</a></li></ul></dl></li>
<li><dl><b>설치 프로그램</b><ul><li><a href="WinDbg.md">WinDbg</a></li><li><a href="https://www.microsoft.com/en-us/download/details.aspx?id=103453">Debug Diagnostic Tool v2</a></li></ul></dl></li>
<li><dl><b><a href="Sysinternals.md">Sysinternals</a></b><ul><li><a href="ProcDump.md">ProcDump</a></li><li><a href="Process_Monitor.md">프로세스 탐색기</a></li></ul></dl></li></ul>

다음은 사용자 모드 덤프의 [종류](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)를 소개하며, 덤프 수집에 필요한 설정은 위에서 언급한 프로그램 문서를 참고하도록 한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;">
<caption style="text-align: center;">사용자 모드 덤프 종류</capation>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#full">전체 덤프</a> (Full User-Mode Dump)</th><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#minidumps">미니 덤프</a> (Minidump)</th></tr></thead>
<tbody><tr><td>프로세스의 사용자 주소 공간에 커밋된 <a href="Memory.md">메모리</a>를 전부 수집한 어플리케이션 덤프이다. 프로그램 실행 이미지, <a href="Process.md#핸들">핸들</a> 테이블, 그리고 덤프가 수집되었을 당시 스택을 재현하는 데 유용한 추가 정보 등을 포함한다.</td><td>증상에 따라 프로세스의 사용자 주소 공간에서 정보를 선택적으로 수집한다. 일반적으로 <a href="Process.md#스레드">스레드 스택</a> 위주로 수집되지만, 경우에 따라 전체 덤프보다 더 많이 혹은 참조된 모듈 단 하나만을 포함하기도 한다.</td></tr></tbody>
</table>

덤프를 [WinDbg](WinDbg.md)로 열어보면 아래와 같은 문구를 통해 전체 혹은 미니 덤프인지 식별할 수 있다.

```windbg
Loading Dump File [C:\Users\Administrator\Downloads\Application.exe.1000.dmp]
User Mini Dump File with Full Memory: Only application data is available
```

## 시간여행 디버깅
[시간여행 디버깅](https://aka.ms/ttd)(Time-Travel Debugging; TTD)은 프로세스가 실행되는 일정 구간동안 수집된 덤프와 추적 정보를 토대로 자유자재 시점을 이동시킬 수 있는 디버거 세션이다. 아래는 TTD 추적 정보를 수집할 수 있는 프로그램을 나열한다.

* **tttracer.exe**: 윈도우 10, 버전 1809 또는 서버 2019 이후부터 `%WinDir%\System32` 시스템 폴더에 추가되었으나 성능이 매우 제한적이다.
* **WinDbg**: 설치 폴더에는 TTD.exe 프로그램이 존재하며, Launch executable (advanced) 및 Attach to process 옵션에서 TTD 수집이 가능하다.

# 커널 모드 덤프
[커널 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-mode-dump-files)(kernel-mode dump), 일명 메모리 덤프는 특정 시점에서 시스템이 기여한 [물리 메모리](https://en.wikipedia.org/wiki/Computer_memory) (즉, [RAM](https://en.wikipedia.org/wiki/Random-access_memory)) 내의 데이터를 수집한 파일이다. 메모리 덤프를 통해 운영체제가 당시 어떠한 작업을 수행하고 있었는지 파악할 수 있다. 본 내용은 메모리 덤프 종류에 대한 설명을 위주로 소개하며, 메모리 덤프 설정 방법은 [여기](BSOD.md#bsod-설정)를 참고하도록 한다.

아래의 커널 모드 덤프를 생성하는 방법들을 나열한다:

* [블루스크린](BSOD.md)
* [LiveKD](LiveKD.md)

### 전체 메모리 덤프
[전체 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/complete-memory-dump)(Complete Memory Dump)는 커널 및 사용자 공간 주소를 모두 포함한 물리 메모리 전체를 수집한다. 시스템이 생성할 수 있는 가장 큰 덤프이며, [페이징 파일](Memory.md#페이징-파일)은 최소한 RAM 크기 + 덤프 헤더 크기(1 MB) + 잠재적 [보조 덤프 정보](#보조-덤프-정보)(256 MB)의 합계만큼 확보되어야 한다.

### 커널 메모리 덤프
[커널 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-memory-dump)(Kernel Memory Dump)는 물리 메모리에서 사용자 공간을 제외한 커널 공간 주소의 데이터만을 수집한다. 비록 [전체 메모리 덤프](#전체-메모리-덤프)보다 필요한 페이징 파일이 작지만, RAM 크기와 할당된 [메모리 풀](Memory.md#메모리-풀) 용량 등 경우에 따라 수집되는 덤프 크기가 천차만별인 관계로 수치화된 정량이 없다.

<table style="table-layout: fixed; width: 60%; margin-left: auto; margin-right: auto;">
<caption style="caption-side: top;">커널 메모리 덤프에 수집되는 정보</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">포함</th><th style="text-align: center;">제외</th></tr></thead>
<tbody style="text-align: center;">
<tr><td><a href="Kernel.md">윈도우 커널</a></td><td>비할당 메모리</td></tr>
<tr><td><a href="Driver.md">커널 모드 드라이버</a></td><td>사용자 모드 어플리케이션</td></tr>
<tr><td>커널 모드 프로그램</td><td>-</td></tr>
<tr><td><a href="Kernel.md#하드웨어-추상-계층">하드웨어 추상화 계층</a></td><td>-</td></tr>
</tbody>
</table>

### 작은 메모리 덤프
[작은 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/small-memory-dump)(Small Memory Dump)는 [KB](https://ko.wikipedia.org/wiki/킬로바이트) 단위의 저용량 덤프를 생성하는데, 운영체제에 따라 크기가 64 KB, 128 KB, 혹은 256 KB 이상이 될 수 있다. 데이터는 제한적이지만, [커널 메모리 덤프](#커널-메모리-덤프) 수집이 불가할 정도로 디스크 여유 공간이 부족한 경우에 아래와 같은 정보를 제공하여 증상을 파악하는데 도움이 될 수 있다.

> 커널 모드 덤프 중에서 유일하게 다른 경로(기본: `%SystemRoot%\Minidump`)에 저장되며, 자세한 내용은 [블루스크린](BSOD.md#crashcontrol-작은-메모리-덤프) 문서를 확인한다.

* 충돌이 발생한 스레드의 커널 맥락 및 정보, 그리고 커널 모드 호출 스택
* 충돌이 발생한 프로세스의 커널 맥락 및 정보
* 충돌이 발생한 프로세서의 맥락
* 버그 검사 코드 및 매개변수
* 로드된 드라이버 목록
* 로드 및 언로드된 모듈 목록<sub>(윈도우 XP부터)</sub>

### 자동 메모리 덤프
[자동 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/automatic-memory-dump)(Automatic Memory Dump)는 실질적으로 [커널 메모리 덤프](#커널-메모리-덤프)와 동일하지만, 페이징 파일 크기를 결정하는 방식이 다르다. "시스템이 관리하는 크기"로 설정되었을 시, 커널 메모리 덤프는 RAM 전체 크기만큼 할당받지만, 자동 메모리 덤프는 충분히 수집이 가능한 작은 크기를 택하려 한다. 만일 페이징 파일 용량이 부족하면 [`PagefileTooSmall`](BSOD.md#bsod-설정) 레지스트리 값을 생성하고 다음 재부팅 때 페이징 파일 크기를 확장시킨다.

### 활성 메모리 덤프
[활성 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/active-memory-dump)(Active Memory Dump)는 트러블슈팅에 필요하지 않는 데이터가 필터링된 [전체 메모리 덤프](#전체-메모리-덤프)의 일종으로, 덤프 파일 크기는 전체 메모리 덤프보다 작다. 엄청난 크기의 물리 메모리가 탑재되었거나 (예를 들어 256 GB 이상의 RAM이 탑재된 서버), [하이퍼바이저](https://ko.wikipedia.org/wiki/하이퍼바이저) 호스트를 대상으로 덤프를 수집해야 하는 경우 등에 활용된다.

<table style="table-layout: fixed; width: 60%; margin-left: auto; margin-right: auto;">
<caption style="caption-side: top;">활성 메모리 덤프에 수집되는 정보</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">포함</th><th style="text-align: center;">제외</th></tr></thead>
<tbody style="text-align: center;">
<tr><td><a href="Kernel.md">윈도우 커널</a></td><td>비할당 메모리</td></tr>
<tr><td><a href="Driver.md">커널 모드 드라이버</a></td><td>게스트 VM 페이지</td></tr>
<tr><td>커널 모드 프로그램</td><td>파일 캐시</td></tr>
<tr><td>사용자 모드 어플리케이션</td><td>-</td></tr>
<tr><td><a href="Kernel.md#하드웨어-추상-계층">하드웨어 추상화 계층</a></td><td>-</td></tr>
</tbody>
</table>

## 보조 덤프 정보
> *참고: [Bugcheck Secondary Dump Data | Microsoft Learn](https://learn.microsoft.com/en-us/shows/inside/bugcheck-secondary-dump-data)*

보조 덤프 정보(Secondary dump data)는 [전체 메모리 덤프](#전체-메모리-덤프) 외의 메모리 덤프에서 RAM으로부터 데이터를 선택적으로 가져오는 도중 분석에 반드시 필요한 연관된 [장치 드라이버](Driver.md) 정보를 놓친 경우를 대비한 데이터이다. 이들은 덤프 파일의 또 다른 데이터 블록에 저장되기 때문에 "보조" 덤프 정보라고 칭하는 것이다. 해당 정보는 장치 드라이버 개발자가 [`KBUGCHECK_SECONDARY_DUMP_DATA`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_kbugcheck_secondary_dump_data) 구조체에 정보를 기입하고, [`KBUGCHECK_REASON_CALLBACK_ROUTINE`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nc-wdm-kbugcheck_reason_callback_routine) [콜백 함수](C.md#콜백-함수)를 등록해야 BSOD 발생 시 수집될 수 있다.

# 참조
* [Enabling a Kernel-Mode Dump File - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/enabling-a-kernel-mode-dump-file)
* [How to determine the appropriate page file size for 64-bit versions of Windows - Windows Client Management &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/client-management/determine-appropriate-page-file-size)
