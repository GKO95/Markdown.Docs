---
category: 디버깅
title: 윈도우 디버거
---
# WinDbg
[WinDbg](https://ko.wikipedia.org/wiki/WinDbg), 일명 윈도우 디버거(Windows Debugger)는 윈도우 운영체제에서 구동되는 [사용자 모드](ko.Windows.md#보호-링)의 어플리케이션이나 [커널 모드](ko.Windows.md#보호-링)의 덤프 등을 [디버깅](https://ko.wikipedia.org/wiki/디버그)하여 트러블슈팅에 활용되는 프로그램이다.

![WinDbg의 간단한 활용 예시: Surface Pro X에서 생성된 <a href="ko.Dump.md#커널-모드-덤프">커널 덤프</a> 분석](./images/windbg_bugcheck_sample.png)

WinDbg는 어플리케이션 충돌이나 [블루스크린](ko.BSOD.md)으로 생성된 `.DMP` 확장자의 [덤프](ko.Dump.md) 파일을 분석하는데 매우 유용하게 사용되며, 그 외에도 실시간 디버깅 및 TTD (시간여행 디버깅) 등이 가능하다. 단, WinDbg는 트러블슈팅 도구 중 하나에 불과하며 윈도우에서 발생한 모든 문제를 해결할 수 없다.

## 설치
윈도우 10 및 11을 사용하고 있다면 [마이크로소프트 스토어](https://apps.microsoft.com/store/detail/windbg-preview/9PGJGD53TN86)에서 WinDbg Preview 버전을 설치할 수 있다.

> 만일 윈도우 7 혹은 8.1 운영체제를 사용하거나, 혹은 Preview가 아닌 버전을 설치하려면 윈도우 [SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)를 통해 설치를 진행한다.

## 설정
원활한 디버깅 작업을 위해서는 아래의 설정을 하는 것을 권장한다.

### 심볼 불러오기
[심볼](ko.Symbol.md)(symbol)은 시스템 환경 변수 `_NT_SYMBOL_PATH`에 심볼 서버 및 캐시 경로를 지정한다. 아래의 예시는 공개 심볼(public symbol)을 `D:\Symbols` 경로에 캐싱한다.

![환경 변수<code>_NT_SYMBOL_PATH</code>의 예시](./images/windbg_environment_symbol.png)

### MEX 확장도구
MEX 확장도구는 WinDbg 디버깅 작업을 더 수월하게 사용할 수 있도록 도와준다. [다운로드](https://www.microsoft.com/en-us/download/details.aspx?id=53304)를 하면 라이브러리가 x86 및 x64 아키텍처로 나뉘어져 있는데, 이는 각각 32비트와 64비트 버전의 WinDbg를 위한 확장도구이다. 시스템 환경 변수 `_NT_DEBUGGER_EXTENSION_PATH`에 MEX 확장도구 라이브러리가 위치한 경로를 기입한다. 아래의 예시는 64비트 `mex.dll` 라이브러리가 `%localappdata%\DBG\EngineExtensions\` 경로에 존재한다.

![환경 변수<code>_NT_DEBUGGER_EXTENSION_PATH</code>의 예시](./images/windbg_environment_extension.png)

# 호출 스택
[호출 스택](https://ko.wikipedia.org/wiki/콜_스택)(call stack)은 현재 실행되고 있는 [함수](https://ko.wikipedia.org/wiki/함수_(컴퓨터_과학))(일명 루틴 혹은 서브루틴)들의 정보를 담고있는 [스택](https://ko.wikipedia.org/wiki/스택) 데이터 구조이다.

> [하노이의 탑](https://ko.wikipedia.org/wiki/하노이의_탑)과 동일한 구조를 지닌 스택은 함수 정보를 최하단부터 순서대로 쌓아가며 (일명 푸쉬; Push), 오로지 최상위에 위치한 함수 정보부터 제거되어야 한다 (일명 팝; Pop). 단, 스택 메모리에서 데이터는 주소가 높은데서 낮은 곳으로 감소하는 방향으로 쌓인다는 점을 유의한다.

여기서 언급한 함수 정보는 [기계어](https://ko.wikipedia.org/wiki/기계어)로 컴파일된 함수 코드에 의해 생성된 데이터이기 때문에, 기계어와 일대일 대응하는 [어셈블리](https://ko.wikipedia.org/wiki/어셈블리어) 언어를 알고 있으면 호출 스택을 이해하는데 큰 도움이 된다. 스택 구조상 하나의 함수로부터 생성된 데이터들은 모두 인접해 있는데, 이들을 통틀어 스택 프레임(stack frame)이라고 칭한다. 즉, 함수들은 스택에서 프레임으로 통용될 수 있으며, 다음 도표와 같은 정보를 담고 있다.

<table style="width: 90%; margin: auto;">
<caption></caption>
<colgroup><col style="width: 10%;"/><col style="width: 45%;"/><col style="width: 45%;"/></colgroup>
<thead><tr><th style="text-align: center">함수 정보</th><th style="text-align: center">설명</th><th style="text-align: center">비고</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><code>Args</code></td><td>호출한 스택 상단의 함수 매개변수로 전달된 <a href="ko.C.md#매개변수-및-전달인자">인자</a></td><td>전달할 인자가 존재하지 않으면 스택에 쌓이지 않는다.</td></tr>
<tr><td style="text-align: center;"><code>RetAddr</code></td><td>종료되면 스택 하단의 함수로 돌아가 재개될 코드 위치 <a href="ko.C.md#매개변수-및-전달인자">주소</a></td><td>스택의 기반이 되는 최하단 함수는 <code>NULL</code>을 갖는다.</td></tr>
<tr><td style="text-align: center;"><code>Locals</code></td><td>함수 <a href="ko.C.md#지역-변수-및-전역-변수">지역 변수</a></td><td>지역 변수 존재하지 않으면 스택에 쌓이지 않는다.</td></tr>
</tbody>
</table>

본 장에서는 호출 스택의 이해를 돕기 위해 [비주얼 스튜디오](https://ko.wikipedia.org/wiki/마이크로소프트_비주얼_스튜디오)(Visual Studio)를 활용하여 직접 소스 코드를 작성하고 x86 및 x64 아키텍처 어셈블리를 확인하여 설명한다.

```c
#include <stdio.h>

long inner_function() {
    
    return 'b';    // 62h
}

void outer_function(short param1, char param2) {
    
    char  variable4 = param1;
    long  variable5 = inner_function();
    short variable6 = param2;
}

int main(int argc, char** argv) {
    
    char  variable1 = 'W';    // 57h
    short variable2 = 'i';    // 69h
    char  variable3 = 'n';    // 6Eh

    outer_function('D' /* 44h */, 'g' /* 67h */);

    return 0;
}
```

## x86 아키텍처
다음은 [x86 아키텍처](https://ko.wikipedia.org/wiki/X86)로 (최적화 없이) 빌드한 어플리케이션을 어셈블리 언어로 나타낸 것이며, 여기서 메모리 주소는 4바이트이다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">x86 아키텍처의 어셈블리</caption>
<colgroup><col/><col/><col/></colgroup>
<thead><tr><th style="text-align: center;"><code>Sample!main</code></th><th style="text-align: center;"><code>Sample!outer_function</code></th><th style="text-align: center;"><code>Sample!inner_function</code></th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```nasm
push	ebp
mov		ebp, esp
sub		esp, 8
mov		byte ptr [ebp-1], 57h
mov		eax, 69h
mov		word ptr [ebp-8], ax
mov		byte ptr [ebp-2], 6Eh
push	67h
push	44h
call	Sample!outer_function
add		esp, 8
xor		eax, eax
mov		esp, ebp
pop		ebp
ret
```
</td>
<td>

```nasm
push	ebp
mov		ebp, esp
sub		esp, 0Ch
mov		al, byte ptr [ebp+8]
mov		byte ptr [ebp-1], al
call	Sample!inner_function
mov		dword ptr [ebp-0Ch], eax
movsx	cx, byte ptr [ebp+0Ch]
mov		word ptr [ebp-8], cx
mov		esp, ebp
pop		ebp
ret
```
</td>
<td>

```nasm
push	ebp
mov		ebp, esp
mov		eax, 62h
pop		ebp
ret
```
</td></tr></tbody>
</table>

우선적으로 다음 레지스터 및 저장하는 데이터를 주목한다. 여기서 레지스터 접두사 `E`는 16비트 레지스터에서 확장되었음을 의미하는 "Extended"를 가리킨다.

* **`ESP`**: 스택 포인터(stack pointer; SP)

    > 스택상 최상위 메모리 주소이다. 해당 주소 위에 데이터를 쌓는 푸쉬, 그리고 해당 주소까지 쌓인 데이터를 위에서부터 순서대로 제거하는 팝 행위가 이루어지면 SP가 감소 및 증가한다.

* **`EBP`**: 베이스 포인터(base pointer; BP)

    > 스택 프레임이 위에 쌓여 기반이 될 스택상 메모리 주소이며, 이는 또한 밑에 깔려있는 스택 프레임의 최상위 주소이기도 한다.

* **`EIP`**: 명령 포인터(instruction pointer; IP)

    > 스택에서 다음으로 실행할 명령어가 위치한 모듈상 메모리 주소이다.

* **`EAX`**, **`EBX`**, **`ECX`**, **`EDX`**: 범목적 레지스터

x86 아키텍처의 스택은 함수가 호출되어 실행될 때 스택상 프레임을 구축하고 종료될 때 해당 프레임을 철거하여 이전 함수의 프레임으로 돌아가는 형식적인 절차를 공통적으로 확인할 수 있다.

```nasm
push	ebp			; 이전 프레임의 BP를 스택에 저장한다.
mov		ebp, esp	; 현 스택의 SP를 현재 프레임의 BP로 설정한다.
.		.
.		.
.		.
mov		esp, ebp	; 현재 프레임의 BP를 현 스택 최상위 주소로 설정한다.
pop		ebp			; 이전 프레임의 BP까지 차례대로 스택을 제거하여 EBP 레지스터로 전달한다.
ret					; 현 스택의 SP가 가리키고 있는 `RetAddr` 주소를 EIP 레지스터로 팝 및 이동한다.
```

`ESP` 레지스터에 값을 빼는 명령이 있는데, 이는 지역 변수를 할당받을 공간을 SP를 스택이 쌓이는 방향으로 이동시켜 확보하는 행위이다. 지역 변수를 갖는 `main` 및 `outer_function` 함수는 각각 스택에서의 데이터 정렬을 고려하여 8바이트와 12바이트 공간을 확보하였다 (이와 유사한 개념으로 [구조체의 데이터 정렬](ko.C.md#데이터-구조-정렬)이 있다). 그리고 `EBP` 레지스터에 저장된 BP가 프레임의 "기반"으로써 지역 변수를 공간에 할당하는데 기준 메모리 주소로 활용된다.

BP의 활용은 그 외에도 인자를 매개변수로 전달할 때에도 사용된다. 함수가 `call` 명령으로 호출되기 전에 전달인자가 스택에 푸쉬되는데, 이들 또한 BP를 기준으로 확보한 인자를 지역 변수로 전달된다. 전달인자는 자료형이 특정되지 않은 관계로 아키텍처 기본 크기인 [워드](https://ko.wikipedia.org/wiki/워드_(컴퓨팅))(즉, 4바이트)만큼 스택에 푸쉬된다. 호출한 함수가 종료되면 푸쉬된 크기만큼 `ESP` 레지스터에 값을 더하여 전달인자가 푸쉬되기 이전 SP로 되돌린다.

함수가 종료되기 전에 `EAX` 레지스터가 할당받은 데이터는 함수로부터 반환될 값이다. 일부 경우에는 `xor` 연산이 수행되는데, 반환값 0을 만들기 위해 필요한 바이트 수가 가장 적으면서 빠른 방법이다.

## x64 아키텍처
다음은 [x64 아키텍처](https://ko.wikipedia.org/wiki/X86-64)로 (최적화 없이) 빌드한 어플리케이션을 어셈블리 언어로 나타낸 것이며, 여기서 메모리 주소는 8바이트이다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">x64 아키텍처의 어셈블리</caption>
<thead><tr><th style="text-align: center;"><code>Sample!main</code></th><th style="text-align: center;"><code>Sample!outer_function</code></th><th style="text-align: center;"><code>Sample!inner_function</code></th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```nasm
mov		qword ptr [rsp+10h], rdx
mov		dword ptr [rsp+8], exc
sub		rsp, 38h
mov		byte ptr [rsp+20h], 57h
mov		eax, 69h
mov		word ptr [rsp+24h], ax
mov		byte ptr [rsp+21h], 6Eh
mov		dl, 67h
mov		cx, 44h
call	Sample!outer_function
xor		eax, eax
add		rsp, 38h
ret
```
</td>
<td>

```nasm
mov		byte ptr [rsp+10h], dl
mov		word ptr [rsp+8], cx
sub		rsp, 38h
movzx	eax, byte ptr [rsp+40h]
mov		byte ptr [rsp+20h], al
call	Sample!inner_function
mov		dword ptr [rsp+28h], eax
movsx	ax, byte ptr [rsp+48h]
mov		word ptr [rsp+24h], ax
add		rsp, 38h
ret
```
</td>
<td>

```nasm
mov		eax, 62h
ret
```
</td></tr></tbody>
</table>

가장 눈에 띄는 차이점으로 레지스터가 "Register"를 의미하는 `R` 접두사로 변경되었다. 총 8바이트 크기 중에 하위 4바이트가 x86 아키텍처의 `E` 접두사 레지스터와 동일하게 동작하는 게 x86 아키텍처와의 호환성에 기여를 한다. 범용 레지스터 `R8` ~ `R15` 총 여덟 개가 추가되었으며 그 외에 x64 아키텍처에서 변경된 사항은 다음과 같다:

* 함수 매개변수로의 첫 네 개 인자들은 스택에 푸쉬되지 않고 각각 `RCX`, `RDX`, `R8`, 그리고 `R9`에 전달된다.

    > 그 이상의 전달인자는 x86 아키텍처처럼 곧바로 스택에 푸쉬된다.

* 인자들은 워드(즉, 8바이트)만큼 0을 패딩하지 않으며, 순전히 매개변수 자료형만큼 크기로 레지스터에 전달된다.
    
    > 예시 코드에서 알파벳 D(`44h`)와 g(`67h`)는 각각 매개변수 자료형 `short` 및 `char`에 대응하는 2바이트 `cx` 및 1바이트 `dl` 레지스터로 전달한다.

* BP를 스택상에서 더 이상 찾아볼 수 없다.

    > SP가 프레임 범위를 결정하는 역할을 대신한다; 종료된 함수의 프레임을 정리하는 게 호출된 함수(callee)가 아닌 호출한 함수(caller)에서 처리하는 일환이다.

* 스택은 16 (`10h`) 바이트 정렬이다.

    > SP로 프레임 공간이 확보되었을 때 `RetAddr`와 최소 8바이트 혹은 그 이상의 빈 공간이 생기는데, 이는 스택 최상위 주소를 `0x10` 배수에 맞추는 과정에서 발생한 잔여 공간이다.

## 스택 추적
[스택 추적](https://ko.wikipedia.org/wiki/스택_추적)은 WinDbg의 [`k`](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/k--kb--kc--kd--kp--kp--kv--display-stack-backtrace-) 명령으로 확인할 수 있으나, 스택 구조상 아키텍처에 따라 표시되는 정보가 다소 상이하다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">아키텍처간 스택 비교</caption>
<colgroup><col/><col/></colgroup>
<thead><tr><th style="text-align: center;">x86 아키텍처 프로그램</th><th style="text-align: center;">x64 아키텍처 프로그램</th></tr></thead>
<tbody><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```windbg
0:000> kL
 # ChildEBP	RetAddr
00 00d8fe6c	00e41021	Experiment!inner_function+0x3
01 00d8fe80	00e41060	Experiment!outer_function+0x11
02 00d8fe98	00e41236	Experiment!main+0x20
03 (Inline)	--------	Experiment!invoke_main+0x1c
04 00d8fee0	75356739	Experiment!__scrt_common_main_seh+0xfa
05 00d8fef0	776290af	KERNEL32!BaseThreadInitThunk+0x19
06 00d8ff48	7762907d	ntdll!__RtlUserThreadStart+0x2b
07 00d8ff58	00000000	ntdll!_RtlUserThreadStart+0x1b
```
</td>
<td>

```windbg
0:000> kL
 # Child-SP				RetAddr				Call Site
00 00000031`56f3f9c8	00007ff7`a6b8102b	Experiment!inner_function
01 00000031`56f3f9d0	00007ff7`a6b8106c	Experiment!outer_function+0x1b
02 00000031`56f3fa10	00007ff7`a6b812a0	Experiment!main+0x2c
03 (Inline Function)	--------`--------	Experiment!invoke_main+0x22
04 00000031`56f3fa50	00007ffc`7ac354e0	Experiment!__scrt_common_main_seh+0x10c
05 00000031`56f3fa90	00007ffc`7c40485b	KERNEL32!BaseThreadInitThunk+0x10
06 00000031`56f3fac0	00000000`00000000	ntdll!RtlUserThreadStart+0x2b
```
</td>
</tr>
<tr><td><ul><li><code>ChildEBP</code>는 각 프레임의 기반이 되는 메모리 주소인 BP를 가리킨다.</li><li>32비트 어플리케이션 및 윈도우 운영체제</li></ul></td>
<td><ul><li><code>Child-SP</code>는 각 프레임의 최상위 메모리 주소인 SP를 가리킨다.</li><li>64비트 어플리케이션 및 윈도우 운영체제</li></ul></td></tr></tbody>
</table>

> 명령어 옆에 `L` 매개변수를 기입하면 소스 코드에 대한 정보를 숨긴 채로 스택 내용을 보여준다.

`Call Site`는 해당 프레임이 어느 함수의 호출로 생성되었는지, 그리고 오프셋이 있다면 이는 함수가 재개될 모듈상 메모리 주소를 가리킨다. 즉, 상위 프레임의 `RetAddr`와 동일한 값을 갖는다.

```windbg
0:000> ? 00007ff7`a6b8102b == Experiment!outer_function+0x1b
Evaluate expression: 1 = 00000000`00000001
```

# 같이 보기
* [Debugger Commands - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-commands)
* [x64 Architecture - Windows drivers &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/x64-architecture)
* [The history of calling conventions, part 5: amd64 - The Old New Thing](https://devblogs.microsoft.com/oldnewthing/20040114-00/?p=41053)
