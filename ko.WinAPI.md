---
category: 윈도우
title: 윈도우 API
---
# 윈도우 API
[윈도우 API](https://ko.wikipedia.org/wiki/윈도우_API)(간단히 WinAPI)는 [사용자 모드](ko.Processor.md#보호-링)에서 [윈도우](ko.Windows.md)가 제공하는 기능들을 활용할 수 있도록 하는 [어플리케이션 프로그래밍 인터페이스](https://ko.wikipedia.org/wiki/API)(application programming interface; API)이다. 개발자는 [마이크로소프트](https://www.microsoft.com/) [공식 문서](https://learn.microsoft.com/en-us/windows/win32/)를 참고하여 윈도우가 제공하는 리소스를 적극적으로 활용할 수 있다. WinAPI는 [C](ko.C.md) 프로그래밍 언어로 작성되었으나 타 프로그래밍 언어를 통해서도 사용이 가능하다.

아직 [16비트](https://ko.wikipedia.org/wiki/16비트) 시스템이 대중화된 시기에 [32비트](https://ko.wikipedia.org/wiki/32비트) 윈도우 운영체제를 지원하는 API라는 것을 명시하기 위해 Win32라고 불렀다. 그러나 [64비트](https://ko.wikipedia.org/wiki/64비트)의 [x64](https://ko.wikipedia.org/wiki/X86-64) 및 [ARM64](https://ko.wikipedia.org/wiki/ARM_아키텍처)도 지원하면서 특정 아키텍처에 종속되지 않는 명칭으로 변경되었다.

## 시스템 서비스
[시스템 서비스](https://ko.wikipedia.org/wiki/시스템_호출)(system services)는 윈도우 [커널](ko.Kernel.md)에서 제공하는 기능을 [사용자 모드](ko.Processor.md#보호-링)에서 호출하는 일련의 절차를 가리킨다. 사용자 모드 프로세스는 [보호 링](ko.Processor.md#보호-링) 구조에 의해 커널 함수, 일명 [루틴](https://ko.wikipedia.org/wiki/함수_(컴퓨터_과학))(routine)을 직접 접근할 수 없으므로 [`Ntdll.dll`](#ntdlldll)에 내포된 진입점을 통해 [`Ntoskrnl.exe`](ko.Kernel.md) 커널 이미지에 정의된 루틴을 호출하게 된다.

다음은 윈도우 API 중에서 `CreateFileW` 함수를 호출할 때의 시스템 서비스가 진행되는 과정을 순서대로 나열한다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">WinAPI <code>CreateFileW</code> 함수의 시스템 서비스 호출 과정</caption>
<colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 20%;"/><col style="width: 20%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">바이너리</th><th style="text-align: center;">이름</th><th style="text-align: center;">함수</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;">1</td><td style="text-align: center;"><code>Kernel32.dll</code></td><td style="text-align: center;"><a href="ko.Subsystem.md#환경-서브시스템">환경 서브시스템 DLL</a></td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilew"><code>CreateFileW</code></a></td><td>시스템 서비스를 호출하는 WinAPI 함수; 그 외에 <code>User32.dll</code>, <code>Gdi32.dll</code> 등이 해당</td></tr>
<tr><td style="text-align: center;">2</td><td style="text-align: center;"><code>Ntdll.dll</code></td><td style="text-align: center;"><a href="ko.Windows.md#ntdlldll">Native API 라이브러리</a></td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntcreatefile"><code>NtCreateFile</code></a></td><td>WinAPI로부터 요청한 사용자 모드의 루틴 진입점</td></tr>
<tr><td style="text-align: center;">3</td><td style="text-align: center;"><code>Ntoskrnl.exe</code></td><td style="text-align: center;">Executive</td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-zwcreatefile"><code>ZwCreateFile</code></a></td><td>WinAPI로부터 요청한 커널 모드의 루틴</td></tr></tbody>
</table>

`NtCreateFile` 그리고 `ZwCreateFile`은 사실상 동일한 함수이다:
* `Nt` 접두사는 [네이티브](ko.Subsystem.md#네이티브-이미지)(native)를 의미하며, 어떠한 [환경 서브시스템](ko.Subsystem.md#환경-서브시스템)에도 종속되지 않은 사용자 모드에서 호출할 수 있는 최저급 진입점에 불과하다.
* `Zw` 접두사는 실질적인 코드가 정의된 루틴이며, (WinAPI가 아닌) [드라이버 API](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/)로써 [장치 드라이버](ko.Driver.md#장치-드라이버) 개발자가 호출하여 사용할 수 있다.

시스템 서비스를 호출한 [스레드](ko.Process.md#스레드)는 [syscall](https://ko.wikipedia.org/wiki/X86_호출_규약#syscall) 명령에 의해 사용자 모드에서 커널 모드로 전환되어, 제약에 걸렸던 모든 [프로세서](ko.Processor.md) 명령어들을 활용할 수 있게 된다. 그리고 커널 모드에서 작업이 완료되면 다시 사용자 모드로 돌아가는 원리로 동작한다.

## 컴포넌트 오브젝트 모델
> *참조: [The Component Object Model - Win32 apps &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model)*

[컴포넌트 오브젝트 모델](https://ko.wikipedia.org/wiki/컴포넌트_오브젝트_모델)(Component Object Model; COM)은 마이크로소프트가 1993년에 프로세스 간 통신에 호환성을 보장하기 위해 표준화한 [어플리케이션 "이진" 인터페이스](https://ko.wikipedia.org/wiki/응용_프로그램_이진_인터페이스)이다. 본 내용을 진행하기 전에 API와 ABI의 차이점을 간단히 소개할 필요가 있다.

<table style="width: 60%; margin: auto;">
<caption style="caption-side: top;">API와 ABI의 차이점</caption>
<colgroup><col style="width: 30%;"/><col style="width: 35%;"/><col style="width: 35%;"/></colgroup>
<thead><tr><th style="text-align: center;">비교</th><th style="text-align: center;">API</th><th style="text-align: center;">ABI</th></tr></thead>
<tbody><tr><td>라이브러리 연관성</td><td style="text-align: center;">함수는 무엇이 존재하며 인자의 개수와 순서가 어떤지 선언</td><td style="text-align: center;">함수는 어떻게 접근되며 인자는 어떻게 전달이 되는지 정의</td></tr>
<tr><td>인터페이스 영역</td><td style="text-align: center;">소스 코드</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/기계어">머신 코드</a></td></tr>
<tr><td>하드웨어 독립</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tr>
</tbody>
</table>

라이브러리에 정의된 데이터와 함수를 소스 코드로 불러올 때는 API가 활용되나, 이를 컴파일 된 이진 파일에서 접근 및 호출할 때에는 ABI가 활용된다. 이는 개발에 사용된 프로그래밍 언어와 컴파일러 상관없이, 서로 달리 제작된 어플리케이션이나 라이브러리 사이에서도 상호작용을 가능케 만든다. 단, COM 제작 및 사용을 할 수 있는 프로그래밍 언어로써는 [가상 메소드 테이블](https://ko.wikipedia.org/wiki/가상_메소드_테이블)을 활용할 수 있는 [포인터](ko.C.md#포인터)가 지원되어야 한다.

[C#](ko.Csharp.md) 프로그래밍 언어의 [인터페이스](ko.Csharp.md#인터페이스) 및 [클래스](en.Csharp.md#클래스)에 대한 지식은 개념의 유사성으로 COM을 이해하는데 도움이 된다: COM 클래스는 도입된 COM 인터페이스의 텅 빈 가상 메소드에 실질적인 함수 코드(일명 컴포넌트, 혹은 COM 오브젝트)를 정의하는데, 해당 컴포넌트를 사용하기 위해서는 오로지 COM 인터페이스로만 접근될 수 있다. 그리고 COM 오브젝트 호출하는 자를 COM 클라이언트, 그리고 제공하는 자를 COM 서버라고 지칭한다.

# 같이 보기
* [Windows API Index](https://learn.microsoft.com/en-us/windows/win32/apiindex/api-index-portal)
* [COM Technical Overview](https://learn.microsoft.com/en-us/windows/win32/com/com-technical-overview)
