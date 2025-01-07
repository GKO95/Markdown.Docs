# 프로세스
**[프로세스](https://en.wikipedia.org/wiki/Process_(computing))**(process)는 [프로그램](https://en.wikipedia.org/wiki/Computer_program)을 실행하기 위해 필요한 데이터와 리소스를 담는 컨테이너이다. 프로그램은 동작 코드가 [기계어](https://en.wikipedia.org/wiki/Machine_code)로 컴파일된 [이미지](https://en.wikipedia.org/wiki/Executable)에 불과하며, [윈도우](Windows.md)에서의 대표적인 프로그램 확장자로 [.exe](https://en.wikipedia.org/wiki/.exe) 및 [.dll](https://en.wikipedia.org/wiki/Dynamic-link_library) 등이 존재한다. [운영체제](https://en.wikipedia.org/wiki/Operating_system)는 실행하려는 프로그램 이미지를 (필요한 라이브러리들과 함께) [커널](Kernel.md)에 의해 생성된 [가상 주소 공간](#가상-주소-공간)에 로드하여 프로세스를 실행한다.

### 시스템 프로세스
**시스템 프로세스**(system process)는 PID가 4로 고정된 Windows OS의 특수한 프로세스이다. [NT 커널](Kernel.md#nt-커널) 및 [드라이버](Driver.md)가 속한 [커널 모드](Processor.md#권한-수준)에서 생성된 "스레드들의 집합"을 가리킨다. 해당 프로세스의 스레드는 [프로세스 공간](#가상-주소-공간)의 주소가 존재하지 않으며 오로지 [시스템 공간](#가상-주소-공간)의 코드만 실행한다. 어떤 프로세스라도 장치 드라이버를 통해 생성한 스레드는 시스템 프로세스에 속하게 된다.

## 가상 주소 공간
> *참고: [Pushing the Limits of Windows: Virtual Memory](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-virtual-memory/ba-p/723750)*

**[가상 주소 공간](https://en.wikipedia.org/wiki/Virtual_address_space)**(virtual address space; VAS)은 프로세스마다 [가상 메모리](Memory.md#가상-메모리)가 할당되는 주소 공간이다. 프로세스마다 주어지는 개별적인 메모리 공간이기 때문에, 일반적으로 타 프로세스에서 접근이 불가하여 어떠한 영향을 미치거나 받지 않는다. 비록 프로세스별 주어지는 주소 공간이지만 *사용자 공간* 그리고 *커널 공간*으로 나뉘어지며 이들에 대한 설명은 다음과 같다:

* **[사용자 공간](https://en.wikipedia.org/wiki/User_space_and_kernel_space)**(user space)

    개별 프로세스의 데이터 및 리소스가 저장되는 "프로세스 공간"이다. 사용자 공간은 모든 프로세스마다 각각 주어지며, 타 프로세스에서 단순 접근이 절대로 불가한 구조이다. 프로세스가 충돌하여도 시스템 및 나머지 프로세스에는 아무런 타격이 없으며 온전히 실행된다. 즉, 가상 주소 공간의 격리성은 바로 사용자 공간의 특성에서 비롯된 것이다.

* **[커널 공간](https://en.wikipedia.org/wiki/User_space_and_kernel_space)**(kernel space)

    프로세스가 실행되기 위해 반드시 필요한 운영체제 [커널](Kernel.md)의 데이터 및 리소스가 저장되는 "시스템 공간"이다. 커널 공간은 성능 및 효율성을 위해 모든 프로세스가 하나의 커널 공간을 공용하여 동일한 정보를 가진다. 그러나 프로세스는 ([드라이버](Driver.md)를 활용하지 않는 이상) 하드웨어 차원의 보안 조치에 의해 커널 공간을 함부로 접근할 수 없다.

가상의 메모리 공간이므로 설치된 [물리 메모리](Memory.md) 용량에 제약을 받지 않는다. 32비트와 64비트 [운영체제](https://en.wikipedia.org/wiki/Operating_system)는 이론상 각각 4 GB (= 2<sup>32</sup>) 그리고 16 EB (= 2<sup>64</sup>) 범위의 주소를 표현할 수 있다. 

<table style="table-layout: fixed; width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">운영체제 및 프로세스 아키텍처에 따른 VAS의 <a href="https://learn.microsoft.com/en-us/windows/win32/memory/memory-limits-for-windows-releases#memory-and-address-space-limits">메모리 제한</a></caption><colgroup><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 40%;"/><col style="width: 40%;"/></colgroup><thead><tr><th colspan="2" style="text-align: center;">프로세스</th><th style="text-align: center;">32비트 운영체제</th><th style="text-align: center;">64비트 운영체제</th></tr></thead><tbody style="vertical-align: top;"><tr><td rowspan="2" style="text-align: center; vertical-align: middle;">32비트</td><td style="text-align: right; vertical-align: middle;">사용자 공간</td><td><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 해제 (기본값)<br/><ul><li>윈도우 NT: 2 GB</li></ul><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b><ul><li>윈도우 NT: 3 GB</li></ul></td><td><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 해제 (기본값)<br/><ul><li>윈도우 NT: 2 GB</li></ul><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 설정<ul><li>윈도우 NT: 4 GB</li></ul></td></tr><tr><td style="text-align: right; vertical-align: middle;">커널 공간</td><td><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 해제 (기본값)<br/><ul><li>윈도우 NT: 2 GB</li></ul><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 설정<ul><li>윈도우 NT: 1 GB</li></ul></td><td><ul><li>윈도우 8 및 이전: 8 TB</li><li>윈도우 8.1 및 이후: 128 TB</li></ul></td></tr><tr><td rowspan="2" style="text-align: center; vertical-align: middle;">64비트</td><td style="text-align: right; vertical-align: middle;">사용자 공간</td><td style="text-align: center; vertical-align: middle;">-</td><td><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 해제<ul><li>윈도우 NT: 2 GB</li></ul><b>IMAGE_FILE_LARGE_ADDRESS_AWARE</b> 설정 (기본값)<br/><ul><li>윈도우 8 및 이전: 8 TB</li><li>윈도우 8.1 및 이후: 128 TB</li></ul></td></tr><tr><td style="text-align: right; vertical-align: middle;">커널 공간</td><td style="text-align: center; vertical-align: middle;">-</td><td ><ul><li>윈도우 8 및 이전: 8 TB</li><li>윈도우 8.1 및 이후: 128 TB</li></ul></td></tr></tbody></table>

여기서 IMAGE_FILE_LARGE_ADDRESS_AWARE 플래그는 프로세스의 사용자 공간을 2 GB 제한보다 확장할 지 여부를 결정하는 어플리케이션 빌드 항목이다. 비주얼 스튜디오에서는 프로젝트 속성 중 `/LARGEADDRESSAWARE` 구성을 아래 그림과 같이 참고한다.

![비주얼 스튜디오의 프로젝트 속성 중 IMAGE_FILE_LARGE_ADDRESS_AWARE 플래그](./images/process_large_addresses.png)

## 핸들
> *참고: [Pushing the Limits of Windows: Handles - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-handles/ba-p/723848)*

**[핸들](https://en.wikipedia.org/wiki/Handle_(computing))**(handle)은 프로세스 차원에서 [파일](FileSystem.md)이나 [레지스트리](Registry.md) 등의 [커널 개체](Kernel.md#커널-개체) 리소스를 접근할 수 있도록 하는 "추상적인" [참조](C.md#포인터)이다. 할당된 커널 개체의 포인터는 사용자 모드에 직접 노출되지 않는 대신 연동된 핸들을 통해 개체를 참조하도록 설계되었다. [커널 모드](Processor.md#권한-수준)에는 각 프로세스마다 주어진 **핸들 테이블**이 존재하며, 핸들과 해당 커널 개체의 포인터를 아래와 같이 관리한다.

<table style="width: 65%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">핸들 테이블을 묘사한 예시</caption><colgroup><col style="width: 10%;"/><col style="width: 30%;"/><col style="width: 30%;"/><col style="width: 30%;"/></colgroup><thead><tr><th style="text-align: center;">인덱스</th><th style="text-align: center;">커널 개체 포인터</th><th style="text-align: center;">접근 마스크</th><th style="text-align: center;">플래그</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td style="text-align: center;">0xF0000000</td><td style="text-align: center;">0x????????</td><td style="text-align: center;">0x00000000</td></tr><tr><td style="text-align: center;">2</td><td style="text-align: center;">NULL</td><td style="text-align: center;">(N/A)</td><td style="text-align: center;">(N/A)</td></tr><tr><td style="text-align: center;">3</td><td style="text-align: center;">0xF0000010</td><td style="text-align: center;">0x????????</td><td style="text-align: center;">0x00000001</td></tr></tbody></table>

<sup>_† 출처: [A Process’ Kernel Object Handle Table - Windows® via C/C++, 5th Edition](https://www.oreilly.com/library/view/windows-via-cc/9780735639904/ch03s02.html)_</sup>

커널 개체의 접근을 위해 [CreateFile](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilew), [CreateNamedPipe](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createnamedpipea) 등의 함수를 호출할 시, 만일 핸들 테이블에 존재하지 않으면 (접근 마스크 및 플래그 일치 여부 포함) 해당 커널 개체를 위한 메모리를 초기화하고 새로운 인덱스 번호를 할당 및 반환한다. 즉, 테이블의 인덱스가 바로 핸들이며 테이블에 의해 매핑된 커널 개체로 접근이 가능한 것이다.

핸들은 포인터와 달리 추상적으로 참조하여 다음과 같은 특성을 지닌다.

1. 핸들 테이블은 프로세스마다 각각 존재하며, 이들의 인덱스(즉, 핸들)와 커널 개체 포인터 간의 매핑은 서로 상이한다.
1. 한 커널 개체에 대한 핸들은 프로세스마다 다르기 때문에, 해당 데이터의 무단 접근은 기존 핸들을 단순히 타 프로세스로 전달하는 걸로 불가하다.

커널 개체의 포인터에 핸들을 연동 및 끊는 행위를 "핸들을 열다(open)" 그리고 "핸들을 닫다(close)"라고 표현한다. 만일 더 이상 사용하지 않는 핸들을 제때 닫지 않으면 커널 개체가 잔여하여 [핸들 누수](https://en.wikipedia.org/wiki/Handle_leak)가 일어나고, 상황에 따라 [메모리 누수](https://en.wikipedia.org/wiki/Memory_leak)를 함께 야기한다.

* *[Debug Tutorial Part 5: Handle Leaks - CodeProject](https://www.codeproject.com/articles/6988/debug-tutorial-part-5-handle-leaks)*
* *[Reference counting - Wikipedia](https://en.wikipedia.org/wiki/Reference_counting)*

# 프로세스 생명주기
본 장은 [C](C.md) 언어로 개발된 [윈도우](Windows.md) 어플리케이션을 위주로 설명한다. 즉, [Win32 API](WinAPI.md) 이외에 [C 런타임 라이브러리](C.md#c-런타임-라이브러리)(CRT)가 함께 언급되어 개념을 설명한다.

## 프로세스 생성
[CreateProcess](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessw) 함수는 [Win32 API](WinAPI.md), 즉 [윈도우 OS](Windows.md)에서 자체적으로 제공하는 [프로세스](#프로세스) 생성 함수이다. 해당 함수는 가장 먼저 프로세스 [커널 개체](Kernel.md#커널-개체), 그 다음 [가상 주소 공간](#가상-주소-공간)을 생성한다. 확보된 가상 주소 공간에 프로그램 실행에 필요한 이미지와 관련 모듈들을 로드한다. 프로그램 이미지에는 시작 함수(startup function)가 내재되어 [진입 함수](C.md#진입점) 실행 외에도 CRT에서 필요한 [메모리](Memory.md) 할당 및 [객체](Cpp.md#클래스) 생성 등을 사전에 작업한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">어플리케이션 유형에 따라 <a href="https://en.wikipedia.org/wiki/Microsoft_Visual_C++">MSVC</a> <a href="https://en.wikipedia.org/wiki/Linker_(computing)">링커</a>가 지정하는 프로그램 시작 함수</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">시작 함수</th><th style="text-align: center;">기본 대상</th></tr></thead><tbody><tr><td style="text-align: center;"><code>mainCRTStartup</code><br/>(혹은 <code>wmainCRTStartup</code>)</td><td><a href="https://learn.microsoft.com/en-us/cpp/build/reference/subsystem-specify-subsystem">/SUBSYSTEM:CONSOLE</a>를 사용하는, 즉 <a href="https://en.wikipedia.org/wiki/Command-line_interface">CLI</a> 어플리케이션<ul><li><code>main</code> (혹은 <code>wmain</code>) 함수를 진입점으로 호출한다.</li></ul></td></tr><tr><td style="text-align: center;"><code>WinMainCRTStartup</code><br/>(혹은 <code>wWinMainCRTStartup</code>)</td><td><a href="https://learn.microsoft.com/en-us/cpp/build/reference/subsystem-specify-subsystem">/SUBSYSTEM:WINDOWS</a>를 사용하는, 즉 <a href="https://en.wikipedia.org/wiki/Graphical_user_interface">GUI</a> 어플리케이션<ul><li><code>__stdcall</code>로 정의된 <code>WinMain</code> (혹은 <code>wWinMain</code>) 함수를 진입점으로 호출한다.</li></ul></td></tr></tbody></table>

<sup>_† 참고: [/ENTRY (Entry-Point Symbol) | Microsoft Learn](https://learn.microsoft.com/en-us/cpp/build/reference/entry-entry-point-symbol)<br/>†† 참고: [How does the linker decide whether to call WinMain or wWinMain? - The Old New Thing](https://devblogs.microsoft.com/oldnewthing/20241004-00/?p=110338)_</sup>

그러므로 CreateProcess 함수는 이후 [기본 스레드](Thread.md#기본-스레드) 커널 개체를 생성하여 위에서 소개한 시작 함수를 본격적으로 실행한다. 이 과정에서 생성된 프로세스 및 기본 스레드 [핸들](#핸들)은 각각 [PROCESS_INFORMATION](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/ns-processthreadsapi-process_information) 구조체의 `hProcess`와 `hThread` 필드에 할당된다. 비록 방금 생성된 프로세스이지만 핸들의 참조 카운트가 두 개로 확인된 이유가 이러한 동작 원리에 의한 것이다.

* 프로세스 및 기본 스레드가 생성된 다음, 추가 [스레드](Thread.md)의 생성 및 종료 과정은 [*스레드 생명주기*](Thread.md#스레드-생명주기) 내용을 참고하도록 한다.

## 프로세스 종료
CRT 구조상 _tWinMain 혹은 _tmain 진입점 함수가 종료하여 반환한 상태 코드는 `nMainRetVal` 변수로 전달된다. 그리고 이를 인자로 전달받은 CRT의 [exit](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/exit-exit-exit) 함수는 생성된 [객체](Cpp.md#클래스) 소멸 및 할당된 [메모리](Memory.md) 해제를 진행한 다음, 마무리로 Win32의 [ExitProcess](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitprocess) 함수를 호출하여 프로세스를 종료한다. 이때 프로세스 커널 개체의 참조 카운트가 차감되어, 만일 카운트가 0에 달하면 개체는 OS에 의해 자동 소멸된다. 하지만 모종의 이유로 참조 카운트가 1 이상의 경우, 프로세스가 종료되었지만 커널 개체가 소멸될 수 없어 잔여하는 [좀비 프로세스](https://en.wikipedia.org/wiki/Zombie_process)가 되어버린다.

### 잘못된 프로세스 종료 방법
프로세스를 종료하는 가장 올바른 방법은 진입 함수를 실행하는 기본 스레드가 [반환문](C.md#return-반환문)을 호출하여 종료되는 것이다. 그 외에 프로세스를 종료하는 방법이 몇 가지 존재하며 이들의 문제점을 함께 설명한다.

1. *기본 스레드에서 Win32의 [ExitThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread) 함수 호출* : 아직 실행 중인 스레드가 있을 경우, 어플리케이션은 더 이상 동작하지 않을지언정 종료되지 않는다.

1. *한 스레드에서 Win32의 ExitProcess 함수 호출* : 나머지 스레드들을 Win32의 [TerminateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminatethread)로 강제 종료하는 것과 마찬가지로, CRT 객체 및 메모리를 정리하는 과정 등을 건너뛴다. 단, CRT에서 처리하지 못한 종료된 프로세스의 시스템 리소스는 결국 운영 체제에서 자체적으로 정리한다.

1. *타 프로세스에서 Win32의 [TerminateProcess](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminateprocess) 함수 호출* : (이하 동문)
