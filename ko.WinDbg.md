---
category: 디버깅
title: 윈도우 디버거
---
# WinDbg
[WinDbg](https://ko.wikipedia.org/wiki/WinDbg)<sub>([다운로드](https://apps.microsoft.com/store/detail/windbg-preview/9PGJGD53TN86))</sub>, 일명 윈도우 디버거(Windows Debugger)는 윈도우 운영체제에서 구동되는 어플리케이션 및 커널 등을 [디버깅](https://ko.wikipedia.org/wiki/디버그)하는 트러블슈팅 프로그램이다.

> 만일 윈도우 7 혹은 8.1 운영체제를 사용하거나, 혹은 Preview가 아닌 버전을 설치하려면 윈도우 [SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)를 통해 설치를 진행한다.

![WinDbg의 간단한 활용 예시: Surface Pro X에서 생성된 <a href="ko.Dump.md#커널-모드-덤프">커널 덤프</a> 분석](./images/windbg_bugcheck_sample.png)

WinDbg는 흔히 어플리케이션 충돌이나 [블루스크린](ko.BSOD.md)으로 생성된 [덤프](ko.Dump.md) 파일을 분석하는 데 사용되며, 그 외에도 실시간 디버깅 및 TTD (Time Travel Debugging; 시간여행 디버깅) 등이 가능하다. 단, WinDbg는 트러블슈팅을 하기 위해 보조하는 도구에 불과하며 윈도우에서 발생한 모든 문제를 해결해 주는 게 아니다.

## 환경 변수 설정
WinDbg로부터 원활한 디버깅 작업을 진행하려면 아래와 같이 시스템 환경 변수를 설정하기를 권장한다.

1. 환경 변수 `_NT_SYMBOL_PATH`에 [심볼](ko.Symbol.md)(symbol) 서버 및 캐시 경로를 지정한다.
1. 환경 변수 `_NT_DEBUGGER_EXTENSION_PATH`에 WinDbg에 활용되는 디버깅 확장도구(예를 들어 [MEX](https://www.microsoft.com/en-us/download/details.aspx?id=53304))가 위치한 폴더 경로를 명시한다. 아래 예시는 확장도구가 `%localappdata%\DBG\EngineExtensions\`에 존재한다고 가정한다.

    ![환경 변수<code>_NT_DEBUGGER_EXTENSION_PATH</code>의 예시](./images/windbg_environment_extension.png)

# 같이 보기
* [Debugger Commands - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-commands)
