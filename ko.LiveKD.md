---
category: Sysinternals
title: LiveKD
---
# LiveKD
[LiveKD](https://aka.ms/livekd)는 현 시스템 상태를 분석하기 위해 강제로 [BSOD](ko.BSOD.md)를 일으켜 [메모리 덤프](ko.Dump.md#커널-모드-덤프)를 생성할 필요 없이 곧바로, 즉 "라이브"로 [커널](ko.Kernel.md#커널)을 디버깅할 수 있도록 지원하는 Sysinternals 유틸리티 프로그램이다. 디버깅은 오로지 LiveKD를 실행한 찰나에 국한되며, 실시간으로 시스템 상태가 그대로 반영되는 게 절대 아니다.

> 만일 시스템을 실시간으로 디버깅을 해야 한다면 WinDbg의 "Attach to kernel" 옵션을 활용하도록 한다.

LiveKD는 단독적으로 사용될 수 없으며 반드시 [kd.exe](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debuggers-in-the-debugging-tools-for-windows-package#kd) (또는 [windbg.exe](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debuggers-in-the-debugging-tools-for-windows-package#windbg-classic)) 디버거 도구가 함께 동반되어야 한다. [윈도우 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/) 다운로드 및 설치 과정에서 Debugging Tools for Windows 옵션을 선택하여 디버거 도구를 확보한다. 아래는 kd.exe가 설치된 경로를 명시하여 LiveKD를 실행한 모습이다.

![LiveKD 유틸리티 프로그램](./images/sysinternals_livekd.png)

마이크로소프트의 [트러블슈팅 스크립트](https://learn.microsoft.com/en-us/troubleshoot/windows-client/windows-troubleshooters/introduction-to-troubleshootingscript-toolset-tss), 일명 TSS는 LiveKD를 활용하여 메모리 덤프를 수집하는 옵션을 제공한다. TSS.zip 압축 파일 안에는 kd.exe를 포함한 트러블슈팅에 활용되는 다양한 이진 파일들이 찾아볼 수 있다. 다음은 [파워셸](ko.PowerShell.md) 명령을 입력하여 LiveKD 메모리 덤프 수집이 가능하다.

```powershell
. .\TSS.ps1 -LiveKD Stop
```

### 대안 프로그램
* [작업 관리자](https://ko.wikipedia.org/wiki/작업_관리자_(윈도우))(Task Manager): [윈도우 11](https://ko.wikipedia.org/wiki/윈도우_11), [22H2](https://ko.wikipedia.org/wiki/윈도우_11_버전_역사#22H2)의 2023년 7월 업데이트, 즉 [KB5027303](https://support.microsoft.com/en-us/topic/july-11-2023-kb5028185-os-build-22621-1992-605fa18f-bd49-41d8-80b1-245080e26c3d)부터 작업 관리자에서 시스템 프로세스(PID 4)의 우클릭 메뉴를 열면 라이브 메모리 덤프를 생성하는 선택지가 추가되었다. 자세한 내용은 [마이크로소프트 공식 문서](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/task-manager-live-dump)를 확인하도록 한다.
