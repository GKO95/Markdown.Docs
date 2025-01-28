# 사용자 모드 덤프
**[사용자 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)**(user-mode dump), 일명 어플리케이션 덤프는 충돌이 발생하는 등 특정 시점에서 [프로세스](Process.md)의 [가상 주소 공간](Process.md#가상-주소-공간) 중 [사용자 공간](Processor.md#권한-수준)에 포함된 정보가 수집된 파일이다. 사용자 모드 덤프를 통해 당시 어플리케이션이 어떠한 작업을 수행하고 있었는지 파악할 수 있으나, [커널](Kernel.md#커널)측 정보는 살펴볼 수 없다.

다음은 사용자 모드 덤프의 [종류](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files)를 소개한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">사용자 모드 덤프 종류</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">유형</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#full">전체 덤프</a><br/>(full user-mode dump)</td><td>프로세스의 사용자 주소 공간에 커밋된 <a href="Memory.md">메모리</a>를 전부 수집한 어플리케이션 덤프이다. 프로그램 실행 이미지, <a href="Process.md#핸들">핸들</a> 테이블, 그리고 덤프가 수집되었을 당시 스택을 재현하는 데 유용한 추가 정보 등을 포함한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/user-mode-dump-files#minidumps">미니 덤프</a><br/>(minidump)</td><td>증상에 따라 프로세스의 사용자 주소 공간에서 정보를 선택적으로 수집한다. 일반적으로 <a href="Thread.md">스레드 스택</a> 위주로 수집되지만, 경우에 따라 전체 덤프보다 더 많이 혹은 참조된 모듈 단 하나만을 포함하기도 한다.</td></tr></tbody></table>

덤프를 [WinDbg](WinDbg.md)로 열어보면 아래와 같은 문구를 통해 전체 혹은 미니 덤프인지 식별할 수 있다 (아래 예시는 전체 덤프에 해당).

```windbg
Loading Dump File [C:\Users\Administrator\Downloads\Application.exe.1000.dmp]
User Mini Dump File with Full Memory: Only application data is available
```

다음 프로그램들은 사용자 모드 덤프를 수집하는 데 사용된다:

* **시스템 기본 프로그램**
    * [Windows Error Reporting](WER.md)
    * [작업 관리자](TaskMgr.md)
* **설치 프로그램**
    * [WinDbg](WinDbg.md)
    * <s>Debug Diagnostic Tool v2</s>&nbsp;<sub>(최신 [윈도우 OS](Windows.md)에서는 부적합 판단)</sub>
* **[Sysinternals](Sysinternals.md)**
    * [ProcDump](ProcDump.md)
    * [프로세스 탐색기](Procexp.md)

## 시간여행 디버깅
**[시간여행 디버깅](https://aka.ms/ttd)**(time-travel debugging; TTD)은 [프로세스](Process.md)가 실행되는 일정 구간 동안에 수집된 덤프와 시간에 따른 추적 정보를 토대로 타임라인을 형성하여 원하는 시점으로 자유롭게 이동할 수 있는 디버거 세션이다. 아래는 TTD 추적 정보를 수집할 수 있는 프로그램을 나열한다.

* tttracer.exe: 윈도우 10, 버전 1809 또는 서버 2019 이후부터 System32 시스템 폴더에 추가되었으나 기능은 제한적이다.
* WinDbg: TTD.exe 프로그램이 설치 폴더에 존재하며, Launch executable (advanced) 및 Attach to process 옵션에서 TTD 수집이 가능하다.

# 커널 모드 덤프
**[커널 모드 덤프](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-mode-dump-files)**(kernel-mode dump), 일명 **메모리 덤프**(memory dump)는 특정 시점에서 시스템이 기여한 [RAM](Memory.md) 내의 데이터를 수집한 파일이다. 메모리 덤프를 통해 운영체제가 당시 어떠한 작업을 수행하고 있었는지 파악할 수 있다. 단, [페이징](Memory.md#페이징-파일)된 [가상 메모리](Memory.md#가상-메모리)는 더 이상 RAM에 상주하지 않기 때문에 커널 모드 덤프에서 확인 불가하다.

다음은 커널 모드 덤프의 [종류](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/varieties-of-kernel-mode-dump-files)를 소개한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">커널 모드 덤프 종류</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">유형</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/complete-memory-dump">전체 메모리 덤프</a><br/>(complete memory dump)</td><td>커널 및 사용자 공간 주소를 모두 포함한 RAM 전체를 수집한다. 시스템이 생성할 수 있는 가장 큰 덤프이며, <a href="Memory.md#페이징-파일">페이징 파일</a>은 최소한 RAM 크기 + 덤프 헤더 크기(1 MB) + 잠재적 <a href="#보조-덤프-정보">보조 덤프 정보</a>(256 MB)의 합계만큼 확보되어야 한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/active-memory-dump">활성 메모리 덤프</a><br/>(active memory dump)</td><td>트러블슈팅에 필요하지 않는 데이터가 필터링된 전체 메모리 덤프의 일종으로, 덤프 파일 크기는 전체 메모리 덤프보다 작다. 엄청난 크기의 RAM이 탑재되었거나 (예를 들어 256 GB 이상의 RAM이 탑재된 서버), <a href="Hypervisor.md">하이퍼바이저</a> 호스트를 대상으로 덤프를 수집해야 하는 경우 등에 활용된다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/kernel-memory-dump">커널 메모리 덤프</a><br/>(kernel memory dump)</td><td>RAM에서 사용자 공간을 제외한 커널 공간 주소의 데이터만을 수집한다. 비록 전체 메모리 덤프보다 필요한 페이징 파일이 작지만, RAM 크기와 할당된 <a href="Memory.md#메모리-풀">메모리 풀</a> 용량 등 경우에 따라 수집되는 덤프 크기가 천차만별인 관계로 수치화된 정량이 없다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/automatic-memory-dump">자동 메모리 덤프</a><br/>(automatic memory dump)</td><td>실질적으로 커널 메모리 덤프와 동일하지만, 페이징 파일 크기를 결정하는 방식이 다르다. "시스템이 관리하는 크기"로 설정되었을 시, 커널 메모리 덤프는 RAM 전체 크기만큼 할당받지만, 자동 메모리 덤프는 충분히 수집이 가능한 작은 크기를 택하려 한다. 만일 페이징 파일 용량이 부족하면 <a href="BSOD.md#bsod-설정"><code>PagefileTooSmall</code></a> 레지스트리 값을 생성하고 다음 재부팅 때 페이징 파일 크기를 확장시킨다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/small-memory-dump">작은 메모리 덤프</a><br/>(small memory dump)</td><td><a href="https://en.wikipedia.org/wiki/Kilobyte">킬로바이트</a> 단위의 극히 제한된 정보만을 담는 저용량 덤프이며 %SystemRoot%\Minidump 폴더에 저장된다. 충돌이 발생하였을 때의 <a href="Processor.md">프로세서</a>, <a href="Process.md">프로세스</a>, 그리고 <a href="Thread.md">스레드</a> 컨텍스트 정보를 제공하여 최소한 증상 파악에 도움이 될 수 있으나, 분석에는 권장되지 않는 덤프이다.</td></tr></tbody></table>

<sup>_† 메모리 덤프를 설정하는 방법은 [BSOD 설정](BSOD.md#bsod-설정) 문서를 참고하도록 한다._</sup>

아래의 커널 모드 덤프를 생성하는 방법들을 나열한다:

* [블루스크린](BSOD.md)
* [LiveKD](LiveKD.md)

## 보조 덤프 정보
> *참고: [Bugcheck Secondary Dump Data | Microsoft Learn](https://learn.microsoft.com/en-us/shows/inside/bugcheck-secondary-dump-data)*

**보조 덤프 정보**(Secondary dump data)는 [전체 메모리 덤프](#커널-모드-덤프) 외의 메모리 덤프에서 RAM으로부터 데이터를 선택적으로 가져오는 도중 분석에 반드시 필요한 연관된 [장치 드라이버](Driver.md) 정보를 놓친 경우를 대비한 데이터이다. 이들은 덤프 파일의 또 다른 데이터 블록에 저장되기 때문에 "보조" 덤프 정보라고 칭하는 것이다. 해당 정보는 장치 드라이버 개발자가 [`KBUGCHECK_SECONDARY_DUMP_DATA`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_kbugcheck_secondary_dump_data) 구조체에 정보를 기입하고, [`KBUGCHECK_REASON_CALLBACK_ROUTINE`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nc-wdm-kbugcheck_reason_callback_routine) [콜백 함수](C.md#콜백-함수)를 등록해야 BSOD 발생 시 수집될 수 있다.

# 참조
* [Enabling a Kernel-Mode Dump File - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/enabling-a-kernel-mode-dump-file)
* [How to determine the appropriate page file size for 64-bit versions of Windows - Windows Client Management &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/client-management/determine-appropriate-page-file-size)
