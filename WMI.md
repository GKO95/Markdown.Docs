# WMI
**[윈도우 관리 도구](https://learn.microsoft.com/windows/win32/wmisdk/wmi-start-page)**(Windows Management Instrumentation; WMI)는 [Windows](Windows.md) 운영체제에서 [WINMGMT](https://learn.microsoft.com/en-us/windows/win32/wmisdk/winmgmt) [서비스](Service.md)에 의해 동작하는 [마이크로소프트](https://www.microsoft.com) 버전의 [웹 기반 기업 관리](https://en.wikipedia.org/wiki/Web-Based_Enterprise_Management), 일명 WBEM 도구이다. 여기서 WBEM이란, 컴퓨터들이 서로 다른 다양한 네트워크에 연결된 기업의 [분산 컴퓨팅](https://en.wikipedia.org/wiki/Distributed_computing) 환경에서 단일화된 [시스템 관리](https://en.wikipedia.org/wiki/Systems_management)를 제공하기 위한 표준 기술이다. WBEM은 [CIM](#일반-정보-모델) 및 [WS-MAN](#웹-서비스-관리) 표준들로부터 비롯되었다.

비록 WBEM의 명칭 안에는 "웹 기반"이라고 명시되었으나 [사용자 인터페이스](https://en.wikipedia.org/wiki/User_interface) 표준을 규정하지 않으므로 [인터넷 브라우저](https://en.wikipedia.org/wiki/Browser_user_interface)가 아닌 프로그램을 사용할 수 있다. 다음은 Windows에서 WMI를 활용할 수 있는 도구들을 소개하고, 운영체제 빌드 번호를 가져오는 [WQL](https://learn.microsoft.com/windows/win32/wmisdk/wql-sql-for-wmi) 결과를 공유한다.

* <s>[WMIC](https://learn.microsoft.com/windows/win32/wmisdk/wmic)(WMI command-line utility)</s>&nbsp;<sub>(deprecated)</sub>
* [PowerShell](PowerShell.md)
    * [Microsoft.PowerShell.Management](https://learn.microsoft.com/powershell/module/microsoft.powershell.management) 모듈에서 제공하는 WMI cmdlets
    * [CimCmdlets](https://learn.microsoft.com/powershell/module/cimcmdlets) 모듈에서 제공하지만 Windows OS에서만 지원하는 상위호환의 CIM cmdlets

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">WMI 명령 유티릴티 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">WMIC</th><th style="text-align: center;">PowerShell - CimCmdlets</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```terminal
WMIC /OUTPUT:clipboard PATH Win32_OperatingSystem GET BuildNumber /VALUE
```
</td><td>

```powershell
Get-CimInstance -ClassName Win32_OperatingSystem -Property BuildNumber
```
</td></tr><tr style="vertical-align: top;"><td>

```terminal
BuildNumber=26200
```
</td><td>

```terminal
BuildNumber : 26200
```
</td></tr></tbody></table>

<sup>_† 참고: [Retrieving a WMI Class - Win32 apps | Microsoft Learn](https://learn.microsoft.com/windows/win32/wmisdk/retrieving-a-class)_</sup>

Windows는 WMI와 관련된 데이터를 `%WinDir%\System32\Wbem` 디렉토리에서 취급한다.

### 일반 정보 모델
**[일반 정보 모델](https://learn.microsoft.com/windows/win32/wmisdk/common-information-model)**(Common Information Model), 일명 CIM은 [데스크탑](https://en.wikipedia.org/wiki/Workstation), [네트워크 어댑터](https://en.wikipedia.org/wiki/Network_interface_controller), [프로세스](Process.md) 등 기업의 IT 환경을 구성하여 운영되는 요소들을 일반적인 [객체 지향 데이터](https://en.wikipedia.org/wiki/Object_(computer_science))로 반영한 크로스 플랫폼 표준 모델이다. CIM은 [C++](Cpp.md) 언어와 무관하지만 IT 환경을 구성하는 요소들을 "[클래스](Cpp.md#클래스)"로 정의하여 각 실체마다 해당하는 클래스로부터 객체화하여 표현되고, 자식 클래스를 [파생](Cpp.md#상속)할 수 있는 점은 매우 유사하다: *[CIM_UnitaryComputerSystem](https://learn.microsoft.com/windows/win32/cimwin32prov/cim-unitarycomputersystem)*, *[CIM_NetworkAdapter](https://learn.microsoft.com/windows/win32/cimwin32prov/cim-networkadapter)*, *[CIM_Process](https://learn.microsoft.com/windows/win32/cimwin32prov/cim-process)* 등.

CIM은 크게 세 가지 유형의 클래스로 나뉘어진다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">CIM의 세 가지 클래스 유형</caption><colgroup><col style="width: 33.3%;"/><col style="width: 33.4%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th style="text-align: center;">핵심 (Core)</th><th style="text-align: center;">공통 (Common)</th><th style="text-align: center;">확장 (Extended)</th></tr></thead><tbody><tr><td>모든 분야의 개체에 적용되어 관리되는 시스템을 분석 및 설명하는데 필요한 기초 용어들을 제공하는 클래스이다.</td><td>특정 분야의 개체에 적용되지만, 어느 한 구현이나 기술로부터 종속되지 않아 공통적으로 사용될 수 있는 클래스이다.</td><td>특정 기술에 종속된 공통 클래스를 가리키며, 대체로 UNIX 혹은 마이크로소프트 Win32 환경과 같은 플랫폼에만 적용될 수 있다.
</td></tr><tr style="text-align: center;"><td><a href="https://learn.microsoft.com/windows/win32/wmisdk/--parameters">__PARAMETERS</a>, <a href="https://learn.microsoft.com/windows/win32/wmisdk/--systemsecurity">__SystemSecurity</a> 등</td><td><a href="https://learn.microsoft.com/windows/win32/cimwin32prov/cim-unitarycomputersystem">CIM_UnitaryComputerSystem</a> 등</td><td><a href="https://learn.microsoft.com/windows/desktop/CIMWin32Prov/win32-computersystem">Win32_ComputerSystem</a> 등</td></tr></tbody></table>

위의 도표에서 WMI 클래스인 Win32_ComputerSystem는 CIM 공통 클래스인 CIM_UnitaryComputerSystem으로부터 파생되었다. 즉, WMI는 마이크로소프트의 Windows 운영체제를 설명하는 속성을 확장하여 CIM에 포함시킨 것이다. CIM 클래스들을 [CIM 스키마](https://en.wikipedia.org/wiki/CIM_Schema)에 미리 정의되어 있으며, 이를 부모로 둔 WMI은 [CIM 2.x 버전 스키마](https://dmtf.org/standards/cim/schemas)만을 지원한다.

### 웹 서비스 관리
**[웹 서비스 관리](https://learn.microsoft.com/windows/win32/winrm/ws-management-protocol)**(Web Service-Management), 일명 WS-Management 혹은 WS-MAN은 서버, 장치, 어플리케이션 그리고 다양한 [웹 서비스](https://en.wikipedia.org/wiki/Web_service)를 관리하기 위한 [SOAP](https://en.wikipedia.org/wiki/SOAP)(Simple Object Access Protocol; 단순 객체 접근 프로토콜) 기반의 프로토콜이다. WS-MAN은 시스템이 IT 인프라를 거쳐 운영 정보를 접근 및 교환할 수 있도록 하는 일반적인 방법을 제시한다.

## 성능 카운터
WMI는 [성능 카운터](Perfmon.md#성능-카운터)로부터 성능 데이터 수치를 가져올 수 있으며, [Win32_Perf](https://learn.microsoft.com/windows/win32/cimwin32prov/performance-counter-classes)로부터 기반한 두 개의 클래스를 소개한다.

* [Win32_PerfRawData](https://learn.microsoft.com/windows/win32/cimwin32prov/win32-perfrawdata): 순수 비가공된 성능 데이터를 [성능 카운터 제공자](https://learn.microsoft.com/windows/win32/wmisdk/performance-counter-provider)로부터 제공받는다.
* [Win32_PerfFormattedData](https://learn.microsoft.com/windows/win32/cimwin32prov/win32-perfformatteddata): 가공된 성능 데이터를 [서식화 성능 카운터 제공자](https://learn.microsoft.com/windows/win32/wmisdk/formatted-performance-data-provider)로부터 제공받는다.

여기서 "가공된 성능 데이터"란, [프로세서](Processor.md) 점유율(%)과 같이 사전에 계산된 성능 데이터를 가리킨다. 그리고 Win32_PerfFormattedData 클래스가 [성능 모니터](Perfmon.md)와 같은 시스템 모니터링 도구에 활용되는 성능 카운터이다.

```powershell
Get-CimInstance -ClassName Win32_PerfFormattedData_PerfOS_Processor -Filter "Name='_Total'" | Select-Object Name, PercentIdleTime, PercentUserTime, PercentPrivilegedTime 
```
```terminal
Name   PercentIdleTime PercentUserTime PercentPrivilegedTime
----   --------------- --------------- ---------------------
_Total              83              10                     2
```

# WMI 아키텍처
> *출처: [WMI Architecture - Win32 apps | Microsoft Learn](https://learn.microsoft.com/windows/win32/wmisdk/wmi-architecture)*

![WMI 아키텍처 다이어그램](https://learn.microsoft.com/en-us/windows/win32/wmisdk/images/wmi-architecture.png)

* [WMI consumers](#wmi-소비자)
* [WMI infrastructure](#wmi-인프라구조)
* [WMI providers](#wmi-공급자)

## WMI 소비자
**WMI 소비자**(WMI consumers)은 WMI를 활용하여 정보를 쿼리 또는 메소드 실행을 요청하는 어플리케이션으로, 일명 "관리 프로그램"이라고도 칭한다. 소비자는 [C](C.md)/[C++](Cpp.md) 또는 [.NET](Csharp.md#net) 프로그램이나 [스크립트](https://learn.microsoft.com/windows/win32/wmisdk/scripting-api-for-wmi)로 제작될 수 있으나, 최종적으로 [WMI COM API](https://learn.microsoft.com/en-us/windows/win32/wmisdk/com-api-for-wmi)를 통해 [WINMGMT](#wmi-인프라구조) 서비스와 접촉하게 된다.

Windows에 기본적으로 내장된 WMI 영구 소비자는 다음 클래스로 구분된다:

* [ActiveScriptEventConsumer](https://learn.microsoft.com/windows/win32/wmisdk/activescripteventconsumer)
* CommandLineEventConsumer
* LogFileEventConsumer
* NTEventLogEventConsumer
* SMTPEventConsumer

## WMI 인프라구조
[**WMI 인프라구조**](https://learn.microsoft.com/windows/win32/wmisdk/wmi-infrastructure)는 다음 두 핵심요소로 구성된다.

1. WINMGMT 서비스
1. WMI 리포지터리

## WMI 공급자
