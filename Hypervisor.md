# 하이퍼바이저
운영체제 커널을 지칭하는 [슈퍼바이저](Kernel.md)(supervisor)란 옛 용어에서 비롯되어, "슈퍼바이저의 슈퍼바이저"를 의미하는 [하이퍼바이저](https://ko.wikipedia.org/wiki/하이퍼바이저)(hypervisor)가 유래되었다.

# 가상 보안 모드
[가상 보안 모드](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/vsm)(virtual secure mode; VSM)는 [가상화 기반 보안](#가상화-기반-보안) 기술을 접목하여 운영체제 안에 새로운 보안 영역을 구축하는 [하이퍼바이저](#하이퍼바이저) 기능이다. VSM은 시스템 보안 자산을 보관하고 처리하기 위한 격리된 [메모리](Memory.md) 영역을 생성하며, 이는 오로지 하이퍼바이저를 통해서만 접근이 승인 및 제어된다. 하이퍼바이저가 운영체제보다 상위 권한을 가지고 있는 점에서 VSM의 격리된 영역은 ([슈퍼바이저 모드](Processor.md#프로세서-모드)의 [커널](Kernel.md#nt-커널) 및 [드라이버](Driver.md)를 포함한) 인가받지 않은 접근으로부터 보호된다.

> [컴퓨터 보안](https://ko.wikipedia.org/wiki/컴퓨터_보안)에서 [자산](https://en.wikipedia.org/wiki/Asset_(computer_security))(assets)이란, 정보 관련 활동에 관여하는 시스템 환경의 데이터, 장치, 또는 기타 구성요소들을 일컷는다.

![윈도우 10 및 서버 2016의 가상화 기반 보안 아키텍처](https://learn.microsoft.com/en-us/windows/win32/procthread/images/uim-architecture.png)

위의 그림은 가상화 기반 보안으로부터 구축된 아키텍처이며, [VTL](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/tlfs/vsm#virtual-trust-level-vtl)(Virtual Trust Level; 가상 신뢰 수준)에 의해 독립된 두 개의 VSM 영역을 묘사하였다. VTL의 숫자가 높을수록 부여되는 권한이 상승하는데, 일반 [윈도우 NT](Windows.md) 커널 및 드라이버는 최하위 권한의 VTL0에 속한다. 이들은 VTL1에 상주하는 리소스를 제어하거나 정의하는 게 절대로 허용되지 않아, 즉 격리된 보안 영역이 구현된다.

VTL1도 마찬가지로 [커널](Processor.md#프로세서-모드) 및 [사용자 공간](Processor.md#프로세서-모드)이 구분되어 있다:

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">윈도우 NT의 VTL1 프로세서 모드</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">커널 모드</th><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/procthread/isolated-user-mode--ium--processes">격리된 사용자 모드</a>(Isolated User Mode; IUM)</th></tr></thead>
<tbody><tr style="text-align: center;"><td>Ring 0</td><td>Ring 3</td></tr><tr style="text-align: center;"><td>보안 커널 (혹은 프록시 커널): securekernel.exe</td><td><a href="WinAPI.md#시스템-서비스">시스템 서비스</a> 및 <a href="Subsystem.md#환경-서브시스템">서브시스템 DLL</a>: Iumdll.dll 및 Iumbase.dll</td></tr><tr style="text-align: center;"><td>VTL0의 메모리 및 리소스 접근에 어떠한 제약을 받지 않는다.</td><td>VTL0의 커널 및 사용자 공간에 접근할 수 없는 완전히 격리된 상태이다.</td></tr><tr><td>보안 커널은 IUM에 특수한 보안 시스템 서비스를 제공하지만, VTL0로 연계되는 시스템 서비스 중 자칫 문제를 일으킬 수 있는 일부를 제한시킨다.</td><td>IUM에서 실행되는 프로세스를 <a href="https://learn.microsoft.com/en-us/windows/win32/procthread/isolated-user-mode--ium--processes#trustlets">Trustlet</a>이라 부르며, 보안 커널의 소스 코드에 등록이 되어야만 실행될 수 있어 마이크로소프트 관리 하에 있다.</td></tr></tbody>
</table>

### 가상화 기반 보안
[가상화 기반 보안](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-vbs)(Virtualization-based security; VBS)은 윈도우 OS의 신뢰를 기반하여 커널과 절충할 수 있는 고립된 환경을 [하드웨어 가상화](https://en.wikipedia.org/wiki/Hardware_virtualization)와 하이퍼바이저로부터 생성한다. 해당 환경에는 다수의 보안 솔루션이 구동되어 운영체제를 취약점으로부터 크게 보호하고, 보안 프로그램을 공격하려는 [멀웨어](https://ko.wikipedia.org/wiki/악성_소프트웨어)(malware; 악성 소프트웨어)로부터 노출을 방지한다. 이러한 제약으로 인해 VBS는 필수적인 시스템 및 리소스를 보호하는 데에도 활용된다.
