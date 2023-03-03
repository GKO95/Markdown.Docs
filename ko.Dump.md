---
category: 디버깅
title: 덤프
---
# 사용자 모드 덤프
[사용자 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)(user-mode dump), 일명 어플리케이션 덤프는 충돌이 발생하는 등 특정 시점에서 [프로세스](ko.Process.md)의 [가상 주소 공간](ko.Process.md#가상-주소-공간) 중 [사용자 공간](ko.Processor.md#보호-링)에 포함된 정보가 수집된 파일이다. 사용자 모드 덤프를 통해 당시 어플리케이션이 어떠한 작업을 수행하고 있었는지 파악할 수 있으나, 커널측 정보는 살펴볼 수 없다. 아래 프로그램들을 사용자 모드 덤프를 수집하는 데 사용된다:

<ul><li><dl><b>시스템 기본 프로그램</b><ul><li><a href="ko.WER.md">Windows Error Reporting</a></li><li><a href="https://ko.wikipedia.org/wiki/작업_관리자">작업 관리자</a></li></ul></dl></li>
<li><dl><b>설치 프로그램</b><ul><li><a href="ko.WinDbg.md">WinDbg</a></li><li><a href="https://www.microsoft.com/en-us/download/details.aspx?id=103453">Debug Diagnostic Tool v2</a></li></ul></dl></li>
<li><dl><b><a href="ko.Sysinternals.md">Sysinternals</a></b><ul><li><a href="ko.ProcDump.md">ProcDump</a></li><li><a href="ko.Process_Monitor.md">프로세스 탐색기</a></li></ul></dl></li></ul>

## 사용자 모드 덤프 종류
본 부문은 사용자 모드 덤프의 [종류](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)를 소개하며, 덤프 수집에 필요한 설정은 각 프로그램 문서를 참고하도록 한다.

### 전체 덤프
[전체 사용자 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#full)(Full User-Mode Dump), 일명 풀 덤프(full dump)는 프로세스의 사용자 주소 공간에 커밋된 메모리를 전부 수집한 어플리케이션 덤프이다. 덤프에 포함된 정보들은 프로그램 실행 이미지, [핸들](ko.Process.md#핸들) 테이블, 그리고 덤프가 수집되었을 당시 스택을 재현하는 데 유용한 추가 정보 등이 있다.

> 전체 덤프와 이름이 비슷한 "[전체 메모리 덤프](#전체-메모리-덤프)"와 혼돈하지 않도록 주의한다.

### 미니 덤프
[미니 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#minidumps)(Minidump)는 프로세스의 사용자 주소 공간 중에서 덤프가 수집된 당시 증상에 따라 정보를 선택적으로 수집한 어플리케이션 덤프이다. 일반적으로 [스레드 스택](ko.Process.md#스레드) 위주로 수집되지만, 경우에 따라 사용자 주소 공간 전체 혹은 참조된 모듈 단 하나만이 수집되기도 한다.

> 비록 명칭은 "미니 덤프"이지만, 간혹 전체 사용자 모드 덤프보다 더 많은 정보를 수집하는 경우도 있다.

# 커널 모드 덤프
[커널 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-mode-dump-files)(kernel-mode dump), 일명 메모리 덤프는 특정 시점에서 시스템이 기여한 [물리 메모리](https://en.wikipedia.org/wiki/Computer_memory) (즉, [RAM](https://en.wikipedia.org/wiki/Random-access_memory)) 내의 데이터를 수집한 파일이다. 메모리 덤프를 통해 운영체제가 당시 어떠한 작업을 수행하고 있었는지 파악할 수 있다. 아래의 커널 모드 덤프를 생성하는 방법들을 나열한다:

* [블루스크린](ko.BSOD.md)
* [LiveKD](ko.LiveKD.md)

## 커널 모드 덤프 종류
본 부문은 커널 모드 덤프의 [종류](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/varieties-of-kernel-mode-dump-files)를 소개하며, 덤프 수집에 필요한 설정은 [블루스크린](ko.BSOD.md) 문서를 참고하도록 한다.

### 전체 메모리 덤프
[전체 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/complete-memory-dump)(Complete Memory Dump)는 커널 및 사용자 공간 주소를 모두 포함한 물리 메모리 전체를 수집한다. 시스템이 생성할 수 있는 가장 큰 덤프이며, 덤프 수집에 사용되는 [페이징 파일](ko.BSOD.md#가상-메모리)은 최소 RAM 크기 + 257 [MB](https://ko.wikipedia.org/wiki/메가바이트)(256 MB의 장치 드라이버에 대한 보조 덤프 정보와 1 MB의 헤더 크기 합계)가 확보되어야 한다.

### 커널 메모리 덤프
[커널 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-memory-dump)(Kernel Memory Dump)는 물리 메모리 중에서 사용자 공간을 제외한 오로지 커널 공간 주소의 데이터만을 수집한다. 생성된 덤프의 크기는 수집된 데이터 양에 따라 다르지만 [전체 메모리 덤프](#전체-메모리-덤프)보다 훨씬 작다.

> 비록 전체 메모리 덤프보다 파일 크기는 작으나, "시스템이 관리하는 크기"로 가상 메모리를 설정하면 RAM 용량만큼의 페이징 파일 크기가 확보된다.

<table style="table-layout: fixed; width: 60%; margin: auto;">
<caption style="caption-side: top;">커널 메모리 덤프에 수집되는 정보</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">포함</th><th style="text-align: center;">제외</th></tr></thead>
<tbody style="text-align: center;">
<tr><td>윈도우 커널</td><td>비할당 메모리</td></tr>
<tr><td>커널 모드 드라이버</td><td>사용자 모드 어플리케이션</td></tr>
<tr><td>커널 모드 프로그램</td><td>-</td></tr>
<tr><td>하드웨어 추상화 계층 (HAL)</td><td>-</td></tr>
</tbody>
</table>

### 작은 메모리 덤프
[작은 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/small-memory-dump)(Small Memory Dump)는 [KB](https://ko.wikipedia.org/wiki/킬로바이트) 단위의 저용량 덤프를 생성하는데, 운영체제에 따라 크기가 64 KB, 128 KB, 혹은 256 KB 이상이 될 수 있다. 비록 데이터는 제한적이지만 제한된 저장공간에서 [커널 메모리 덤프](#커널-메모리-덤프) 수집이 불가한 환경에서 1 MB 페이징 파일만으로 문제점을 파악하기에 충분한 정보를 제공할 수 있다:

> 커널 모드 덤프 중에서 유일하게 다른 경로(기본: `%SystemRoot%\Minidump`)에 저장되며, 자세한 내용은 [블루스크린](ko.BSOD.md#crashcontrol-작은-메모리-덤프) 문서를 확인한다.

* 충돌이 발생한 스레드의 커널 맥락 및 정보, 그리고 커널 모드 호출 스택
* 충돌이 발생한 프로세스의 커널 맥락 및 정보
* 충돌이 발생한 프로세서의 맥락
* 버그 검사 코드 및 매개변수
* 로드된 드라이버 목록
* 로드 및 언로드된 모듈 목록<sub>(윈도우 XP부터)</sub>

### 자동 메모리 덤프
[자동 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/automatic-memory-dump)(Automatic Memory Dump)는 실질적으로 [커널 메모리 덤프](#커널-메모리-덤프)와 동일하지만, 시스템에서 페이징 파일을 크기를 결정하는 방식에서 차이가 있다. 페이징 파일은 "시스템이 관리하는 크기"로 설정되어야 하며, 블루스크린이 발생할 때 시스템은 커널 메모리 덤프를 수집하는 데 충분히 작은 크기를 택하려 한다. 만일 부족하다면 시스템은 페이징 파일의 크기를 RAM 용량 (혹은 최대 32 GB까지)만큼 확장시키고 QWORD 레지스트리 `PagefileTooSmall`을 생성을 생성한다.

> 확장된 가상 메모리 크기는 4주간 유지되며, 당장 원래대로 되돌리고 싶다면 레지스트리 편집기에서 `PagefileTooSmall` 값을 삭제하고 재부팅한다.

### 활성 메모리 덤프
[활성 메모리 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/active-memory-dump)(Active Memory Dump)는 트러블슈팅에 필요하지 않는 데이터가 필터링된 [전체 메모리 덤프](#전체-메모리-덤프)의 일종으로, 덤프 파일 크기는 전체 메모리 덤프보다 작은 관계로 RAM 크기가 매우 크거나 [하이퍼바이저](https://ko.wikipedia.org/wiki/하이퍼바이저) 호스트를 대상으로 덤프를 수집할 때 매우 유용하다.

<table style="table-layout: fixed; width: 60%; margin: auto;">
<caption style="caption-side: top;">활성 메모리 덤프에 수집되는 정보</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">포함</th><th style="text-align: center;">제외</th></tr></thead>
<tbody style="text-align: center;">
<tr><td>윈도우 커널</td><td>비할당 메모리</td></tr>
<tr><td>커널 모드 드라이버</td><td>게스트 VM 페이지</td></tr>
<tr><td>커널 모드 프로그램</td><td>파일 캐시</td></tr>
<tr><td>사용자 모드 어플리케이션</td><td>-</td></tr>
<tr><td>하드웨어 추상화 계층 (HAL)</td><td>-</td></tr>
</tbody>
</table>

# 참조
* [Enabling a Kernel-Mode Dump File - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/enabling-a-kernel-mode-dump-file)
* [How to determine the appropriate page file size for 64-bit versions of Windows - Windows Client Management &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/client-management/determine-appropriate-page-file-size)
