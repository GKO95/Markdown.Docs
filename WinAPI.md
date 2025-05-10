# 윈도우 API
**[윈도우 API](https://en.wikipedia.org/wiki/Windows_API)**, 일명 **WinAPI**는 [사용자 모드](Processor.md#사용자-모드)의 [어플리케이션](https://en.wikipedia.org/wiki/Computer_program)이 시스템 리소스를 활용할 수 있도록 [Windows OS](Windows.md)에서 제공하는 [프로그래밍 인터페이스](https://en.wikipedia.org/wiki/API)이다. **Win32 API**라른 이름으로도 널리 알려져 있으며, 아직 ([MS-DOS](https://en.wikipedia.org/wiki/MS-DOS) 및 [Windows 9x](https://en.wikipedia.org/wiki/Windows_9x) 등) [16비트](https://en.wikipedia.org/wiki/16-bit_computing) 시스템이 대중화된 당시에 새로운 아키텍처를 지닌 [32비트](https://en.wikipedia.org/wiki/32-bit_computing) 시스템을 위해 설계된 API라는 걸 명시하였다. 현재는 [x86-64](https://en.wikipedia.org/wiki/X86-64) 및 [ARM64](https://en.wikipedia.org/wiki/ARM_architecture_family) 계열도 지원을 확장하며 [아키텍처](https://en.wikipedia.org/wiki/Computer_architecture)에 종속되지 않는 명칭으로 변경되었다. C 프로그래밍 언어로 제작되었기 때문에 저급 프로그래밍 언어의 특성에 따라 타 프로그래밍 언어에서도 WinAPI를 활용할 수 있다.

## 시스템 서비스
**[시스템 서비스](https://en.wikipedia.org/wiki/System_call)**(system services)는 [윈도우 커널](Kernel.md#nt-커널)에서 제공하는 기능을 [사용자 모드](Processor.md#권한-수준)에서 호출하는 일련의 절차를 가리킨다. 사용자 모드 프로세스는 [보호 링](Processor.md#권한-수준) 구조에 의해 커널 함수, 일명 [루틴](https://ko.wikipedia.org/wiki/함수_(컴퓨터_과학))(routine)을 직접 접근할 수 없으므로 `Ntdll.dll`에 내포된 진입점을 통해 [`Ntoskrnl.exe`](Kernel.md#nt-커널) 커널 이미지에 정의된 루틴을 호출하게 된다.

다음은 윈도우 API 중에서 `CreateFileW` 함수를 호출할 때의 시스템 서비스가 진행되는 과정을 순서대로 나열한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">WinAPI <code>CreateFileW</code> 함수의 시스템 서비스 호출 과정</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 20%;"/><col style="width: 20%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">바이너리</th><th style="text-align: center;">이름</th><th style="text-align: center;">함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td style="text-align: center;"><code>Kernel32.dll</code></td><td style="text-align: center;"><a href="Windows.md#환경-서브시스템">환경 서브시스템 DLL</a></td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilew"><code>KERNEL32!CreateFileW</code></a></td><td>시스템 서비스를 호출하는 WinAPI 함수; 그 외에 <code>User32.dll</code>, <code>Gdi32.dll</code> 등이 해당</td></tr><tr><td style="text-align: center;">2</td><td style="text-align: center;"><code>Ntdll.dll</code></td><td style="text-align: center;"><a href="Windows.md#네이티브-이미지">Native API 라이브러리</a></td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntcreatefile"><code>ntdll!NtCreateFile</code></a></td><td>WinAPI로부터 요청한 사용자 모드의 루틴 진입점</td></tr><tr><td style="text-align: center;">3</td><td style="text-align: center;"><code>Ntoskrnl.exe</code></td><td style="text-align: center;"><a href="Kernel.md#nt-커널">Executive</a></td><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/nf-ntifs-ntcreatefile"><code>nt!NtCreateFile</code></a></td><td>WinAPI로부터 요청한 커널 모드의 루틴</td></tr></tbody></table>

시스템 서비스를 호출한 [스레드](Thread.md)는 [syscall](https://ko.wikipedia.org/wiki/X86_호출_규약#syscall) 명령에 의해 사용자 모드에서 커널 모드로 전환되어, 제약에 걸렸던 모든 [프로세서](Processor.md) 명령어들을 활용할 수 있게 된다. 그리고 커널 모드에서 작업이 완료되면 다시 사용자 모드로 돌아가는 원리로 동작한다.

### `Nt`와 `Zw` 접두사 시스템 서비스 비교
> *출처: [Using Nt and Zw Versions of the Native System Services Routines - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/using-nt-and-zw-versions-of-the-native-system-services-routines)*

위의 예시에서 `NtCreateFile`을 소개하였으나, 그 외에도 [`ZwCreateFile`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/nf-ntifs-ntcreatefile)이란 시스템 서비스 API도 존재한다. `Nt`는 "네이티브(native)" 용어에서 유래되었으나, `Zw`는 다른 API 함수명과 혼돈을 방지하기 위해 선택된 알파벳 조합이다. 두 버전은 유사한 (혹은 동일한) 작업을 수행하며, 둘 다 사용자 및 커널 모드에서 호출할 수 있다. 차이점은 커널 모드에서 매개변수로 전달된 인자의 유효성 검증 여부이다.

## 컴포넌트 오브젝트 모델
> *참조: [The Component Object Model - Win32 apps &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model)*

[컴포넌트 오브젝트 모델](https://ko.wikipedia.org/wiki/컴포넌트_오브젝트_모델)(Component Object Model; COM)은 마이크로소프트가 1993년에 프로세스 간 통신에 호환성을 보장하기 위해 표준화한 [어플리케이션 "이진" 인터페이스](https://ko.wikipedia.org/wiki/응용_프로그램_이진_인터페이스)이다. 본 내용을 진행하기 전에 API와 ABI의 차이점을 간단히 소개할 필요가 있다.

<table style="width: 60%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">API와 ABI의 차이점</caption><colgroup><col style="width: 30%;"/><col style="width: 35%;"/><col style="width: 35%;"/></colgroup><thead><tr><th style="text-align: center;">비교</th><th style="text-align: center;">API</th><th style="text-align: center;">ABI</th></tr></thead><tbody><tr><td>라이브러리 연관성</td><td style="text-align: center;">함수는 무엇이 존재하며 인자의 개수와 순서가 어떤지 선언</td><td style="text-align: center;">함수는 어떻게 접근되며 인자는 어떻게 전달이 되는지 정의</td></tr><tr><td>인터페이스 영역</td><td style="text-align: center;">소스 코드</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/기계어">머신 코드</a></td></tr><tr><td>하드웨어 독립</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tr></tbody></table>

라이브러리에 정의된 데이터와 함수를 소스 코드로 불러올 때는 API가 활용되나, 이를 컴파일 된 이진 파일에서 접근 및 호출할 때에는 ABI가 활용된다. 이는 개발에 사용된 프로그래밍 언어와 컴파일러 상관없이, 서로 달리 제작된 어플리케이션이나 라이브러리 사이에서도 상호작용을 가능케 만든다. 단, COM 제작 및 사용을 할 수 있는 프로그래밍 언어로써는 [가상 메소드 테이블](https://ko.wikipedia.org/wiki/가상_메소드_테이블)을 활용할 수 있는 [포인터](C.md#포인터)가 지원되어야 한다.

[C#](Csharp.md) 프로그래밍 언어의 [인터페이스](Csharp.md#인터페이스) 및 [클래스](en.Csharp.md#클래스)에 대한 지식은 개념의 유사성으로 COM을 이해하는데 도움이 된다: COM 클래스는 도입된 COM 인터페이스의 텅 빈 가상 메소드에 실질적인 함수 코드(일명 컴포넌트, 혹은 COM 오브젝트)를 정의하는데, 해당 컴포넌트를 사용하기 위해서는 오로지 COM 인터페이스로만 접근될 수 있다. 그리고 COM 오브젝트 호출하는 자를 COM 클라이언트, 그리고 제공하는 자를 COM 서버라고 지칭한다.

# 같이 보기
* [Windows API Index](https://learn.microsoft.com/en-us/windows/win32/apiindex/api-index-portal)
* [COM Technical Overview](https://learn.microsoft.com/en-us/windows/win32/com/com-technical-overview)
