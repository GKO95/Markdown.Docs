# DTrace
> *출처: [DTrace on Windows - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/dtrace)*

[**DTrace**](https://en.wikipedia.org/wiki/DTrace)는 운영 중인 어플리케이션 및 [커널](Kernel.md)을 실시간으로 트러블슈팅할 수 있도록 하는 [C 언어](C.md)로 제작된 [트레이싱](https://en.wikipedia.org/wiki/Tracing_(software)) 프레임워크이다. 처음에는 [Sun Microsystems](https://en.wikipedia.org/wiki/Sun_Microsystems)에서 자신들의 UNIX 기반 운영체제인 [Solaris](https://en.wikipedia.org/wiki/Oracle_Solaris)를 위해 개발하였으나, 추후 오픈 소스로 공개되며 [Linux](https://en.wikipedia.org/wiki/Linux) 및 [Windows](Windows.md) 버전으로도 포팅되었다. DTrace는 [Windows Server 2025](https://en.wikipedia.org/wiki/Windows_Server_2025)부터 기본적으로 시스템에 탑재되었지만,  Windows 19040 이상인 그 이전 빌드의 경우에서 DTrace를 별도로 [설치](https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#installing-dtrace-under-windows)해야 한다.

DTrace가 지원되는 Windows에서 이를 활성화하려면 BCD를 설정해야 하지만 아래 사항에 대해 유의하도록 한다.

```
bcdedit /set dtrace on
```

* Windows 업그레이드로 빌드가 바뀌면 BCD의 DTrace 설정도 원래대로 초기화된다.
* [BitLocker](BitLocker.md)를 사용하고 있다면 임시 중단(suspend)시켜 BCD 변경으로 인해 복구 키를 요구하는 걸 방지할 수 있다.

### 가상 머신의 DTrace 요건
가상 머신에서 DTrace를 실행하려면 [네스티드 가상화](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/enable-nested-virtualization)를 활성화해야 한다. 아래는 관리자 권한의 [Windows PowerShell](PowerShell.md)에서 [Set-VMProcessor](https://learn.microsoft.com/en-us/powershell/module/hyper-v/set-vmprocessor) cmdlet으로 [Hyper-V](Hypervisor.md) VM의 가상 [프로세서](Processor.md)를 설정하는 명령이며, 여기서 `<VMName>`에는 DTrace를 실행할 가상 머신의 이름을 기입한다.

```powershell
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```

## 아키텍처
DTrace 엔진은 크게 두 프로그램으로 구성된다.

![DTrace 아키텍처](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/images/dtrace-architecture.png)

1. DTrace.exe: 사용자의 상호작용으로 입력받은 [D 스크립트](#d-프로그래밍-언어)를 중간 언어인 DIF로 컴파일하여 커널 영역으로 전달한다.
1. DTrace.sys: 사용자 모드에서 전달받은 DIF를 실행하는 DIF Virtual Machine을 제공한다.

한편, Traceext.sys는 Windows 커널의 확장 드라이버로 DTrace가 트레이싱을 하는 데 필요한 Windows 기능을 노출시킨다. [스택 추적](https://en.wikipedia.org/wiki/Stack_trace)이나 메모리 접근이 이루어질 때, 커널은 DTrace가 필요한 기능이나 정보를 트레이싱 확장 드라이버에 구현된 [콜아웃](https://learn.microsoft.com/windows-hardware/drivers/network/callout)을 통해 제공한다.

### DTrace 제공자
트레이싱 탐사(probe) 대상에 따라 Windows DTrace는 아래 제공자들을 구현한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">DTrace on Windows에 구현된 제공자 목록</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">제공자</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#syscall--ntos-system-calls"><b>SYSCALL</b></a></td><td>사용자와 커널 모드 간 전환이 이루어지는 <a href="Kernel.md#nt-커널">NT 커널</a>의 <a href="WinAPI.md#시스템-서비스">시스템 호출</a>을 탐사한다.

```
dtrace -ln syscall:::
```
<ul><li>진입 탐사: <i>시스템 호출이 진입되기 직전에 트리거된다.</i></li><li>반환 탐사: <i>시스템 호출을 완료하여 사용자 모드로 되돌아가기 직전에 트리거된다.</i></li></ul></td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#etw"><b>ETW</b></a></td><td><i><a href="ETW.md">Event Tracing for Windows</a></i>를 탐사하여 DTrace가 운영체제의 기존 트레이싱 체계를 활용할 수 있도록 한다.

```
dtrace -ln etw:::
```
</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#function-boundary-tracing-fbt"><b>FBT</b></a></td><td><i>Function Boundary Tracing</i>의 약자이며, <a href="Hypervisor.md#가상-보안-모드">VSM</a>가 활성화되어 있다면 NT 커널의 루틴의 진입과 반환을 탐사할 수 있다. 모듈의 함수를 탐사하기 때문에 <a href="Symbol.md">심볼</a> 설정이 필요할 수 있다.

```
dtrace -ln fbt:nt::
```
<ul><li>위의 예시 코드는 nt 모듈에서 탐사할 수 있는 모든 커널 루틴을 나열한다.</li></ul></td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#pid"><b>PID</b></a></td><td>PID를 제시하여 <a href="Processor.md#사용자-모드">사용자 모드</a>의 <a href="Process.md">프로세스</a> 내부 코드를 트레이싱하며, 함수의 특정 오프셋을 지정할 수도 있다. DTrace 스크립트에 의해 실행되거나 이미 실행 중인 프로세스만 탐사될 수 있다.

```
dtrace -ln pid$target:ntdll:RtlAllocateHeap:entry -c notepad.exe
```
<ul><li><code>$target</code>은 <code>-p</code> 혹은 <code>-c</code>로부터 전달받은 PID를 확장하는 매크로이며, 탐사하려는 프로세스를 지정한다.</li><li>위의 예시 코드는 notepad.exe를 새로 실행하여 ntdll 모듈의 RtlAllocateHeap 함수의 진입 탐사를 살펴본다.</li></ul></td></tr></tbody></table>

<sup>_† 도표 안에 위치한 명령어는 각 제공자마다 탐사할 수 있는 시스템 호출, 함수, 혹은 ETW 목록을 나열한다._</sup>

# D 프로그래밍 언어
**D 프로그래밍 언어**는 DTrace의 트레이싱을 설계하는데 사용되는 [스크립트 언어](https://en.wikipedia.org/wiki/Scripting_language)이며, 스크립트 파일은 .d 확장자로 구분된다. 단, 동명의 "[D 프로그래밍 언어](https://en.wikipedia.org/wiki/D_(programming_language))"와 전혀 다른 존재이며 아무런 연관성이 없다. 다음은 D 프로그래밍 언어의 대표적인 특징들을 몇 가지 소개한다.

* [대소문자를 구분한다](https://en.wikipedia.org/wiki/Case_sensitivity).
* 조건문 및 반복문을 사용하지 않는다.
* [C 언어](C.md)의 구문 + [AWK](https://en.wikipedia.org/wiki/AWK)의 [데이터 지향](https://en.wikipedia.org/wiki/Data-driven_programming) 프로그래밍

다음은 DTrace의 가장 기본적인 형태이다.

```
provider:module:function:name /predicate/ {
    action;
}
```
