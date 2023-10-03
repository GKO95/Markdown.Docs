# 어셈블리
[어셈블리](https://ko.wikipedia.org/wiki/어셈블리어)(Assembly)는 프로그래밍 언어의 명령 코드가 [아키텍처](https://ko.wikipedia.org/wiki/컴퓨터_구조)(예를 들어 [x86](https://ko.wikipedia.org/wiki/X86), [x64](https://ko.wikipedia.org/wiki/X86-64), [ARM64](https://ko.wikipedia.org/wiki/ARM_아키텍처) 등)의 [기계어](https://ko.wikipedia.org/wiki/기계어) [명령](https://ko.wikipedia.org/wiki/명령어_집합)과 매우 잘 부합하는 저급 프로그래밍 언어를 일컫는다. 비록 [C++](Cpp.md), [C#](Csharp.md), [파이썬](Python.md) 등의 고급 프로그래밍 언어에 비해 코딩 속도가 느리지만, 기계어를 직접 프로그래밍 하기 때문에 실행 속도는 [C](C.md) 언어보다 빠르다. 그러므로 운영체제 중에서 빠른 처리 속도가 요구되는 일부 기능은 어셈블리 언어로 제작된다.

어셈블리 언어는 다음 컴퓨터 과학 및 구조에 대한 상당한 이해가 전제되어야 한다:

* [프로세서](Processor.md)
* [메모리](Memory.md)
* [이진법](https://ko.wikipedia.org/wiki/이진법) 및 [십육진법](https://ko.wikipedia.org/wiki/십육진법)
* [논리 연산](https://ko.wikipedia.org/wiki/논리_연산)

필수 요소는 아니지만, 아래에 대한 충분한 이해가 있으면 본문의 내용을 이해하는 데 수월하다:

* C 프로그래밍 언어
* 비주얼 스튜디오
* [WinDbg](WinDbg.md)

### 어셈블러
[어셈블러](https://ko.wikipedia.org/wiki/어셈블리어#어셈블러)(assembler)는 어셈블리 언어를 기계어로 변환하는 프로그램이다. 얼핏 [컴파일러](Compiler.md)와 유사하지만, 어셈블러는 프로그래밍 언어를 변환하는 게 아니라 단순히 어셈블리 코드를 대응하는 기계어로 변환한다. 대표적인 어셈블러로 인텔의 [NASM](https://ko.wikipedia.org/wiki/넷와이드_어셈블러)<sub>([공식 홈페이지](https://www.nasm.us/))</sub>, 마이크로소프트의 [MASM](https://learn.microsoft.com/en-us/cpp/assembler/masm/microsoft-macro-assembler-reference)<sub>([공식 홈페이지](http://www.masm32.com/))</sub> 등이 존재한다.

어셈블러를 통해 어셈블리 언어에서 기계어로 변환되면 부산물로 [오브젝트 파일](https://ko.wikipedia.org/wiki/목적_파일)(object file)이 생성된다. 오브젝트 파일은 파편적인 코드이므로, 다른 코드와 연동시키기 위해서는 [링커](https://ko.wikipedia.org/wiki/링커_(컴퓨팅))(linker)의 작업을 필요하다. 이를 통해 정적 및 동적 [라이브러리](C.md#라이브러리)의 함수나 구조체를 가져올 수 있게 된다.

# 레지스터
[레지스터](https://ko.wikipedia.org/wiki/프로세서_레지스터)(register)는 [프로세서](Processor.md)가 연산을 위해 필요한 데이터, 또는 연산을 마치고 반환될 데이터를 임시로 저장할 수 있는 [워드](https://ko.wikipedia.org/wiki/워드_(컴퓨팅))([x86](https://ko.wikipedia.org/wiki/X86) 및 [x64](https://ko.wikipedia.org/wiki/X86-64)는 각각 32비트 그리고 64비트) 크기의 메모리이다. 여기서 워드(word)란, 시스템 아키텍처가 처리하는 데 가장 자연스러운 데이터 크기를 가리킨다. 시스템 아키텍처의 워드를 정의하는 요소 중 하나가 바로 프로세서의 레지스터 크기이다.

> 레지스터는 프로세서에만 종속되지 않고 [물리 디스크](https://ko.wikipedia.org/wiki/하드_디스크_드라이브) 또는 [그래픽 카드](https://ko.wikipedia.org/wiki/그래픽_카드) 등 다양한 하드웨어에서도 활용되지만, 본문은 프로세서를 위주로 설명한다.

비록 RAM도 매우 빠른 속도의 데이터 접근성을 자랑하지만, 레지스터는 아예 CPU 내부에 탑재되어 있어 접근 속도가 순식간이다. 그러한 만큼 어셈블리 언어는 레지스터를 활발히 사용하기 때문에 각 레지스터의 역할이 무엇인지 인지해야 한다.

![x86-64 아키텍처의 프로세서 레지스터](https://upload.wikimedia.org/wikipedia/commons/1/15/Table_of_x86_Registers_svg.svg)

위의 그림은 x86-64(일명 x64) 아키텍처의 다양한 레지스터를 보여주며, 그 중에는 레지스터(예. 64비트 `RAX`) 안에 또 다른 레지스터(예. 32비트 `EAX`)가 들어있는 구조를 찾아볼 수 있다. 이는 사실상 하드웨어적으로는 하나의 메모리이지만, `EAX`는 메모리의 전체 64비트 중에서 하위 32비트만을 활용하는 레지스터이다.

## 범용 레지스터
[범용 레지스터](https://en.wikipedia.org/wiki/Processor_register#GPR)(General-Purpose Register; GPR)는 본래 의도된 목적이 존재하지만 상황에 따라 다양한 용도로 사용될 수 있는 레지스터이다. 이론적으로 어느 작업이라도 제약없이 유연하게 활용될 수 있으나, [호출 규약](#호출-규약)(calling convention)에 의해 레지스터마다 어떻게 사용되어야 할 지 규칙이 정해져 있다. 다음은 x64 아키텍처에서 제공하는 범용 레지스터를 소개한다.

* **기본 범용 레지스터(Primary General-Purpose Register)**

    GPR 중에서 가장 기본적이면서 활발히 사용되는 `A`, `C`, `D`, 그리고 `B` 레지스터이다. 16비트 아키텍처 당시에도 이들은 8비트 단위로 처리되어야 하는 경우가 흔하여, 상위(High) 및 하위(Low) 8비트 명칭을 접미사 `H`와 `L`로 분류하였다. 그리고 이 둘을 종합한 레지스터는 16비트로 "확장되었다(e**X**tended)"고 하여 접미사 `X`가 붙는다. 32비트 및 64비트 프로세서의 등장으로 각각 접두사 `E`(**E**xtended의 앞글자)와 `R`(**R**egister의 앞글자)"를 붙여 명칭한다.

    * `A` (Accumulator): 산술 및 논리 연산에 활용되는 레지스터이다.
    * `C` (Counter): 반복문의 카운터로 활용되며, `loop` 등의 일부 명령어는 카운터의 영값 여부에 따라 반복 실행한다.
    * `D` (Data): 산술 및 논리 연산을 보조하는 데이터 레지스터이며, 특히 정수의 곱셈이나 나눗셈 등에 흔히 활용된다.
    * `B` (Base): 배열, 문자열, 구조체 등의 데이터 기반 혹은 오프셋 메모리 주소를 저장하는 포인터 레지스터이다.

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">x86-64 프로세서의 기본 GPR 명칭</caption><colgroup><col style="width: 50%;"/><col style="width: 25%;"/><col style="width: 12.5%;"/><col style="width: 12.5%;"/></colgroup><thead><tr><th style="text-align: center;">64</th><th style="text-align: center;">32</th><th style="text-align: center;">16</th><th style="text-align: center;">8</th></tr></thead><tbody style="text-align: center;"><tr><td colspan="4"><code>R?X</code></td></tr><tr><td>-</td><td colspan="3"><code>E?X</code></td></tr><tr><td colspan="2">-</td><td colspan="2"><code>?X</code></td></tr><tr><td colspan="2">-</td><td><code>?H</code></td><td><code>?L</code></td></tr></tbody></table>

* **[포인터](C.md#포인터) 레지스터(Pointer Register)**

    [스택](https://ko.wikipedia.org/wiki/스택) 연산을 위한 메모리 주소를 다루며, 명칭 뒤에 포인터(pointer)를 의미하는 `P`가 있는 게 특징이다:

    * `SP` (Stack Pointer): 스택의 최상위 주소를 가리키는 레지스터이며, `push` 또는 `pop` 연산 시 자동으로 값이 변동된다.
    * `BP` (Base Pointer): [함수](C.md#함수)의 [스택 프레임](https://en.wikipedia.org/wiki/Call_stack#Structure)(stack frame), 일명 호출 스택(call stacK)의 기반 메모리 주소를 가리키는 레지스터이다.

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">x86-64 프로세서의 포인터 레지스터 명칭</caption><colgroup><col style="width: 50%;"/><col style="width: 25%;"/><col style="width: 12.5%;"/><col style="width: 12.5%;"/></colgroup><thead><tr><th style="text-align: center;">64</th><th style="text-align: center;">32</th><th style="text-align: center;">16</th><th style="text-align: center;">8</th></tr></thead><tbody style="text-align: center;"><tr><td colspan="4"><code>R?P</code></td></tr><tr><td>-</td><td colspan="3"><code>E?P</code></td></tr><tr><td colspan="2">-</td><td colspan="2"><code>?P</code></td></tr><tr><td colspan="3">(64비트 모드에서만 지원)</td><td><code>?PL</code></td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><sup>† 범용 레지스터처럼 16비트 레지스터를 상위 및 하위 8비트로 나누려는 데 의미를 두지 않았다.</sup></caption></table>

* **[인덱스](C.md#배열) 레지스터(Index Register)**

    [배열](C.md#배열) 혹은 [문자열](C.md#문자열)의 메모리 주소를 다루며, 명칭 뒤에 인덱스(index)를 의미하는 `I`가 있는 게 특징이다:

    * `SI` (Source Index): 배열 혹은 문자열의 원천 주소를 담는 레지스터이다.
    * `DI` (Destination Index): 배욜 혹은 문자열의 목적 주소를 담는 레지스터이다.

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">x86-64 프로세서의 인덱스 레지스터 명칭</caption><colgroup><col style="width: 50%;"/><col style="width: 25%;"/><col style="width: 12.5%;"/><col style="width: 12.5%;"/></colgroup><thead><tr><th style="text-align: center;">64</th><th style="text-align: center;">32</th><th style="text-align: center;">16</th><th style="text-align: center;">8</th></tr></thead><tbody style="text-align: center;"><tr><td colspan="4"><code>R?I</code></td></tr><tr><td>-</td><td colspan="3"><code>E?I</code></td></tr><tr><td colspan="2">-</td><td colspan="2"><code>?I</code></td></tr><tr><td colspan="3">(64비트 모드에서만 지원)</td><td><code>?IL</code></td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><sup>† 범용 레지스터처럼 16비트 레지스터를 상위 및 하위 8비트로 나누려는 데 의미를 두지 않았다.</sup></caption></table>

* **추가 레지스터(Additional Register)**

    x86-64 아키텍처의 64비트 모드에서만 사용할 수 있는 GPR `R8` ~ `R15`는 본래 64비트를 위해 설계되었으므로, 하위 비트의 레지스터를 부르는 명칭을 달리 택하였다: 접미사에 `DWORD` (32비트), `WORD` (16비트), 그리고 `BYTE` (8비트) 자료형의 앞글자를 따서 분별한다.

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">x86-64 프로세서의 64비트 전용 GPR 명칭</caption><colgroup><col style="width: 50%;"/><col style="width: 25%;"/><col style="width: 12.5%;"/><col style="width: 12.5%;"/></colgroup><thead><tr><th style="text-align: center;">64</th><th style="text-align: center;">32</th><th style="text-align: center;">16</th><th style="text-align: center;">8</th></tr></thead><tbody style="text-align: center;"><tr><td colspan="4"><code>?</code></td></tr><tr><td>-</td><td colspan="3"><code>?D</code></td></tr><tr><td colspan="2">-</td><td colspan="2"><code>?W</code></td></tr><tr><td colspan="3">-</td><td><code>?B</code></td></tr></tbody></table>

## 특수 레지스터
특수 레지스터(Special-Purpose Register; SPR)는 [범용 레지스터](#범용-레지스터)와 달리, 특수한 목적을 위해서만 사용되는 레지스터를 가리킨다:

* **명령어 포인터 레지스터(Instruction Pointer Register)**

    일명 [IP](https://ko.wikipedia.org/wiki/프로그램_카운터) 레지스터는 다음으로 실행할 명령어가 위치하는 메모리 주소를 가리킨다.

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">x86-64 프로세서의 IP 레지스터 명칭</caption><colgroup><col style="width: 50%;"/><col style="width: 25%;"/><col style="width: 12.5%;"/><col style="width: 12.5%;"/></colgroup><thead><tr><th style="text-align: center;">64</th><th style="text-align: center;">32</th><th style="text-align: center;">16</th><th style="text-align: center;">8</th></tr></thead><tbody style="text-align: center;"><tr><td colspan="4"><code>RIP</code></td></tr><tr><td>-</td><td colspan="3"><code>EIP</code></td></tr><tr><td colspan="2">-</td><td colspan="2"><code>IP</code></td></tr></tbody></table>

* **[플래그 레지스터](https://en.wikipedia.org/wiki/FLAGS_register)(FLAGS Register)**

    일종의 [상태 레지스터](https://ko.wikipedia.org/wiki/상태_레지스터), 즉 현 프로세서 상태를 담고 있는 레지스터이다. 흔히 산술 연산의 결과나 당시 CPU 작업에 걸린 [제약](Processor.md#프로세서-모드)을 반영한다.

## 호출 규약
[호출 규약](https://ko.wikipedia.org/wiki/호출_규약)(calling convention)은 함수가 [매개변수](C.md#매개변수-및-전달인자)를 통해 인자를 전달받고 [`return`](C.md#return-반환문) 문으로 결과를 반환하는 방식을 규정하며, 일반적으로 [ABI](https://ko.wikipedia.org/wiki/응용_프로그램_이진_인터페이스)의 일부로 간주된다. 호출자(caller)와 피호출자(callee) 간 데이터 전달은 일반적으로 레지스터나 스택 프레임의 도움으로 처리된다. 올바른 호출 규약의 사용은 신뢰할 수 있는 프로그램 실행을 보장하기 때문에 매 함수 호출 시 준수되어야 한다.

본 부문에 대한 설명을 진행하기 전, 우선 휘발성(volitile) 및 비휘발성(non-volitile) 레지스터가 무엇인지 소개한다.

<table style="width: 80%; margin: auto;"><caption style="caption-side: top;">휘발성 및 비휘발성 레지스터 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">휘발성(volitile)</th><th style="text-align: center;">비휘발성(non-volitile)</th></tr></thead><tbody><tr><td style="text-align: center;">호출자에 의해 저장(caller-saved)</td><td style="text-align: center;">피호출자에 의해 저장(callee-saved)</td></tr><tr><td>저장된 정보는 다른 함수로 인해 쉽게 덮어씌어질 수 있으며, 만일 복원하려면 호출자가 당시 값을 저장해야 한다.</td><td>피호출자가 반환된 이후에도, 해당 함수를 호출한 당시 호출자의 레지스터 값들은 피호출자에 의해 복원되어야 한다.</td></tr></tbody></table>

### x86 아키텍처
> *참고: [x86 Architecture - Windows driver | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/x86-architecture)*

다양한 종류의 [x86 호출 규약](https://ko.wikipedia.org/wiki/X86_호출_규약)이 존재하며 함수에 선언하여 결정할 수 있으나, 본 내용에서는 중요하다고 판단한 두 가지만 소개한다:

1. **[`__cdecl`](https://learn.microsoft.com/en-us/cpp/cpp/cdecl)**

    "C Declaration"의 줄임말로 C 언어의 기본 호출 규약이면서 가장 널리 사용되고 있다. 일반적으로 선언 당시에 별도로 호출 규약을 명시하지 않아도 되지만, 만일 해야 할 경우가 있으면 아래와 같이 작성한다.

    ```c
    void __cdecl function(int argc, char** argv) { return; }
    ```

    해당 호출 규약의 특징으로 (1) 전달인자를 스택을 통해 전달하며, 오른쪽에서부터 왼쪽 순서대로 스택에 푸쉬되고 (2) 정수나 메모리 주소는 `EAX` 레지스터, 부동소수점은 `ST0 x87` 레지스터를 통해 반환된다.

1. **[`__stdcall`](https://learn.microsoft.com/en-us/cpp/cpp/stdcall)**

    마이크로소프트의 [WinAPI](WinAPI.md) 함수에 적용되어, 아래와 같이 함수에서 선언되어야 한다.

    ```c
    void __stdcall function(int argc, char** argv) { return; }
    ```

호출 규약 `__cdecl`와 `__stdcall` 사이에는 호출된 함수를 정리(clean-up)하는 주체가 누구인지 달라진다: 전자는 호출자에게, 그리고 후자는 피호출자에게 책임을 묻는다. 여기서 스택 정리란, 위에서 언급한 휘발성 및 비휘발성 레지스터와 전혀 다른 개념이다.

<table style="width: 80%; margin: auto;"><caption style="caption-side: top;"><code>__cdecl</code> 및 <code>__stdcall</code> 호출 규약의 휘발성 및 비휘발성 레지스터</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">휘발성(volitile)</th><th style="text-align: center;">비휘발성(non-volitile)</th></tr></thead><tbody><tr><td style="text-align: center;"><code>EAX</code>, <code>ECX</code>, <code>EDX</code></td><td style="text-align: center;">나머지 레지스터</td></tr></tbody></table>

### x64 아키텍처
> *참고: [x64 Architecture - Windows driver | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/x64-architecture)*

[x64 호출 규약](https://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_calling_conventions)은 크게 두 종류가 있으며, [윈도우 OS](Windows.md) 혹은 [유닉스 계열](https://ko.wikipedia.org/wiki/유닉스_계열) 운영체제에 따라 각각 Microsoft x64 호출 규약 및 System V AMD64 ABI가 사용된다. x86 아키텍처와 달리, 레지스터 개수가 늘어나면서 레지스터 활용도가 상당히 늘어났으며, 본 부분은 Microsft x64 호출 규약을 위주로 설명한다.

1. 첫 네 개의 인자는 레지스터를 통해 피호출자로 전달된다(정수, 구조체, 또는 포인터: `RCX`, `RDX`, `R8`, `R9`; 부동소수점: `XMM0`, `XMM1`, `XMM2`, `XMM3`). 이때, 호출자는 반환 메모리 주소 위에 *무조건* 32바이트의 그림자 공간(shadow space)을 할당해야 하는 데, 이는 방금 언급한 네 개의 레지스터를 반영한 공간이다.

1. 레지스터에서 수용할 수 없는 나머지 인자(즉, 다섯 개 이상의 경우에만 해당)들은 오른쪽에서부터 왼쪽 순서대로 그림자 공간 위에 푸쉬한다.

1. 정수나 메모리 주소는 `RAX` 레지스터, 부동소수점은 `XMM0` 레지스터를 통해 반환된다. 그러나 반환되는 데이터가 64비트 미만일 경우, 상위 비트는 영으로 정리되지 않은 채 하위 레지스터(즉, `EAX`, `AX` 등)를 활용한다.

<table style="width: 80%; margin: auto;"><caption style="caption-side: top;">Microsoft x64 호출 규약의 휘발성 및 비휘발성 레지스터</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">휘발성(volitile)</th><th style="text-align: center;">비휘발성(non-volitile)</th></tr></thead><tbody><tr><td style="text-align: center;"><code>RAX</code>, <code>RCX</code>, <code>RDX</code>, <code>R8</code>, <code>R9</code>, <code>R10</code>, <code>R11</code></td><td style="text-align: center;"><code>RBX</code>, <code>RBP</code>, <code>RDI</code>, <code>RSI</code>, <code>RSP</code>, <code>R12</code>, <code>R13</code>, <code>R14</code>, <code>R15</code></td></tr></tbody></table>

# 구문
[구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)은 프로그래밍 언어에서 문자 및 기호들의 조합이 올바른 문장 또는 표현식을 구성하였는지 정의하는 규칙이다. 어셈블리 코드를 작성하지 않더라도, 구문을 알고 있으면 디버깅 등의 트러블슈팅 목적으로도 유용하게 활용될 수 있다. 허나, 운영체제 및 [어셈블러](#어셈블러)에 따라 구문에 차이가 있음을 명시하도록 한다.

> 본문은 [윈도우](Windows.md) 운영체제에서 [NASM](https://ko.wikipedia.org/wiki/넷와이드_어셈블러) 어셈블러를 위주로 설명한다.

어셈블리의 소스 코드는 [메모리 세그먼트](https://ko.wikipedia.org/wiki/X86_메모리_분할)<sub>([참고](https://en.wikipedia.org/wiki/File:Program_memory_layout.pdf))</sub> 중 다음 세 가지를 지원하며 (즉, [스택](https://ko.wikipedia.org/wiki/스택) 및 [힙](https://ko.wikipedia.org/wiki/동적_메모리_할당) 영역 제외), 이들은 `section` 키워드와 함께 명시하여 구분된다:

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">어셈블리 언어의 프로그래밍 영역</caption>
<colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup>
<thead><tr><th style="text-align: center;">세그먼트</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Code_segment"><code>.text</code></a></td><td>프로세서가 실행할 코드가 기입된다; MASM의 경우 <a href="https://en.wikipedia.org/wiki/Code_segment"><code>.code</code></a> 사용을 권장한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Data_segment"><code>.data</code></a></td><td>초기화된 변수 및 상수를 선언하며, 런타임에 변경이 불가하다.</td></tr><tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/.bss"><code>.bss</code></a></td><td>초기화되지 않거나 영값을 가진 변수를 선언하며, 흔히 버퍼로 사용된다.</td></tr></tbody>
</table>

```nasm
global _main     ; 전역으로 선언된 _main 함수
extern _printf   ; extern으로 선언된 _printf 함수

; .text (혹은 .code) 영역
section .text
_main:
    push message
    call _printf
    add  esp, 4
    ret

; .data 영역
section .data
    message db "Hello World", 10, 0    ; ASCII 10 (new line '\n') and 0 (null terminator '\0')
```

위의 예시 코드에서 세미콜론 `;`은 주석(comment)을 나타내며 프로그램의 소스 코드로 취급하지 않아 실행되지 않는다.

### 진입점
[진입점](https://ko.wikipedia.org/wiki/엔트리_포인트)(entry point)은 프로그램이 시작되는 지점이다. C 런타임 라이브러리는 `_main` 함수를 기본 진입점으로 지정하였지만, [링커](https://ko.wikipedia.org/wiki/링커_(컴퓨팅))에게 진입점을 명시한다면 어떠한 이름을 지정해도 상관없다. 그리고 프로그램을 실행할 수 있도록 진입점은 외부에서도 접근이 가능한 [전역 함수](C.md#함수)(global function)로 선언되어야 한다.

> `_main`에서의 밑줄은 사용자가 정의한 심볼과 네이밍 충돌을 방지하기 위한 관습으로, GCC 컴파일러는 C 언어 심볼에 해당 관습을 기본으로 적용한다.

## 문장
[문장](https://ko.wikipedia.org/wiki/문_(프로그래밍))(statement)은 실질적으로 무언가를 실행하는 구문적 존재를 가리킨다. 어셈블리 언어는 스크립트 줄마다 한 개의 문장만 기입될 수 있다.

```nasm
MNEMONIC    OPERAND     ; 명령어 집합을 표현하는 기초적인 문장 구성
```
위의 문장은 하나의 [명령어 집합](#명령어)을 나타내며, 이를 구성하는 요소들은 다음 역할을 지닌다:

* [니모닉](https://ko.wikipedia.org/wiki/기억술#치환법(변환법))(mnemonic): 기계어 연산자(일명 opcode)를 프로그래머가 쉽게 알아볼 수 있도록 영문으로 명시된 심볼적 명칭이다(`ADD`, `MOV`, 등).
* [피연산자](https://ko.wikipedia.org/wiki/피연산자#컴퓨터_과학)(operand): 니모닉으로부터 연산될 데이터들이며, `RET`와 같이 일부 경우에는 피연산자가 요구되지 않는다.

### 레이블
[레이블](https://ko.wikipedia.org/wiki/레이블_(컴퓨터_과학))(label)은 [메모리 주소](C.md#포인터)를 명칭으로 호출할 수 있도록 하며, [C](C.md)/[C++](Cpp.md) 언어의 `goto` 이동문에 사용되는 [레이블](C.md#goto-이동문)과 동일하다. 다시 말해, 레이블은 `.data` 혹은 `.bss` 영역에서 다루어지는 [변수](C.md#변수)가 절대 아니다. 본 장의 예시에서 `_main`은 진입점을 가리키는 레이블에 해당한다.

# 명령어
> *출처: [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)*

[프로세서](Processor.md)가 처리하게 될 [명령어](https://en.wikipedia.org/wiki/Instruction_set_architecture#Instructions)는 일반적으로 두 가지 필드로 구성되어 있다.

![MIPS32 Add Immediate 명령의 구조](https://upload.wikimedia.org/wikipedia/commons/2/2a/Mips32_addi.svg)

1. [명령 코드](https://ko.wikipedia.org/wiki/명령_코드)(일명 opcode)는 덧셈, 복사, 이동 등의 프로세서가 수행할 연산 작업을 명시한다.
1. [피연산자](https://ko.wikipedia.org/wiki/피연산자#컴퓨터_과학)는 opcode에 따라 아예 없거나 한 개 이상이 [레지스터](#레지스터), [리터럴](C.md#구문) 및 [상수](C.md#변수), 또는 [메모리 주소](C.md#포인터)로서 활용된다.

다양한 명령어를 전부 다룰 수 없는 관계로 본 장에서는 부연 설명이 필요한 일부 명령어에 대해서만 소개하며, 만일 찾을 수 없다면 위의 링크로부터 다운로드한 소프트웨어 개발자 매뉴얼 문서에서 *Volume 2: Instruction Set Reference, A-Z*를 참고한다.
