# WMI
[윈도우 관리 도구](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page)(Windows Management Instrumentation; WMI)는 [윈도우](Windows.md) 운영체제에서 동작하는 [마이크로소프트](https://www.microsoft.com) 버전의 [웹 기반 기업 관리](#웹-기반-기업-관리) 도구이다. 로컬 시스템에서 WMI는 [winmgmt](https://learn.microsoft.com/en-us/windows/win32/wmisdk/winmgmt) [서비스](Service.md)에 의해 svchost.exe에 호스트되어 실행되며, 터미널로부터 WMI의 리포지터리 또는 svchost.exe 호스팅 방식 등의 설정이 가능하다. WMI에 대한 정보는 윈도우의 `%WinDir%\System32\Wbem` 디렉토리에서 찾아볼 수 있다.

## 웹 기반 기업 관리
[웹 기반 기업 관리](https://en.wikipedia.org/wiki/Web-Based_Enterprise_Management)(Web-Based Enterprise Management), 일명 WBEM은 컴퓨터가 다른 네트워크에 위치하지만 하나의 시스템을 구성하는 [분산 컴퓨팅](https://ko.wikipedia.org/wiki/분산_컴퓨팅) 환경에서의 관리를 단일화하기 위한 [시스템 관리](https://en.wikipedia.org/wiki/Systems_management) 표준 기술로써 관리 정보를 접근한는 데 활용된다.

> 비록 "웹 기반"이라고 명시되어 있으나, WBEM은 [사용자 인터페이스](https://ko.wikipedia.org/wiki/사용자_인터페이스) 표준을 규정하지 않으므로 [인터넷 브라우저](https://en.wikipedia.org/wiki/Browser_user_interface)가 아니더라도 [GUI](https://ko.wikipedia.org/wiki/그래픽_사용자_인터페이스) 혹은 [CLI](https://ko.wikipedia.org/wiki/명령_줄_인터페이스) 등도 사용할 수 있다.

WBEM은 아래에서 소개되는 [CIM](#일반-정보-모델) 및 [WS-MAN](#웹-서비스-관리) 표준들로부터 비롯되었다.

### 일반 정보 모델
[일반 정보 모델](https://learn.microsoft.com/en-us/windows/win32/wmisdk/common-information-model)(Common Information Model), 일명 CIM은 [데스크탑](https://ko.wikipedia.org/wiki/워크스테이션), [네트워크 라우터](https://ko.wikipedia.org/wiki/라우터), [어플리케이션](https://ko.wikipedia.org/wiki/응용_소프트웨어) 등 기업의 IT 환경을 구성하여 운영되는 요소들을 일반적인 [객체 지향 데이터](https://ko.wikipedia.org/wiki/객체_(컴퓨터_과학))로 반영한 모델이다. 단순히 운영정보 교환에 그치지 않으며, 이들을 적극적으로 제어하고 관리할 수 있는 기능을 함께 제공한다. CIM의 공통 모델이 적용된 요소들은 하나의 IT 환경 관리 소프트웨어로도 복잡하거나 무거운 변환 작업 또는 정보의 손실 없이도 다양한 구성들과 상호작용이 가능하다.

CIM 표준은 다음 내용들을 소개한다:

* **[CIM 스키마](https://en.wikipedia.org/wiki/CIM_Schema)(CIM Schema)**

    IT 환경 내에서 운영되는 장비, 하드웨어, 소프트웨어 등의 요소들을 정의한 [CIM 클래스](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-wmi-provider)들의 집합체이다. CIM 클래스는 하드웨어 종류나 소프트웨어 유형마다 이들을 잘 반영하는 대표되는 데이터 모형을 제공하는, 즉 프로그래밍 언어의 [클래스](Csharp.md#클래스)와 동일한 개념이다. [운영체제](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-operatingsystem), [프로세스](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-process), [네트워크 어댑터](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-networkadapter), [프린터](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-printer), 심지어 [쿨링팬](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-fan) 등도 CIM 클래스로 정의되며, 실제 장비나 프로그램들은 CIM 클래스로부터 객체화된 CIM 인스턴스(CIM instance)라고 부른다.

    * *[Win32 스키마](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/win32-provider)*

        CIM 스키마에서 파생되어 Win32 환경에 최적화된 클래스들을 제공한다. CIM과 Win32 스키마는 각각 `CIM_` 그리고 `Win32_` 접두사로 구분된다. 예를 들어 [Win32 운영체제 클래스](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/win32-operatingsystem)는 CIM 운영체제 클래스에서 파생되었으나, 빌드 번호 등의 윈도우 운영체제를 반영하는 속성들이 추가되었다.

* **CIM 기반 구조 사양(CIM Infrastructure Specification)**

    CIM 구조 및 개념을 정의한다: 다른 정보 모델(예를 들어 [SNMP](https://ko.wikipedia.org/wiki/간이_망_관리_프로토콜))하고 매핑되는 방법과 CIM 스키마가 정의된 언어가 무엇인지 등을 내포한다. CIM 구조는 객체 지향으로써 IT 환경의 구성요소와 이들의 관계성을 각각 CIM 클래스 및 [연관](https://en.wikipedia.org/wiki/Association_(object-oriented_programming))으로 나타내고, [상속](Csharp.md#상속)을 활용하여 파생 CIM 클래스 정의도 가능케 한다.

### 웹 서비스 관리
[웹 서비스 관리](https://learn.microsoft.com/en-us/windows/win32/winrm/ws-management-protocol)(Web Service-Management), 일명 WS-Management 혹은 WS-MAN은 서버, 장치, 어플리케이션 그리고 다양한 [웹 서비스](https://ko.wikipedia.org/wiki/웹_서비스)를 관리하기 위한 [SOAP](https://ko.wikipedia.org/wiki/SOAP)(Simple Object Access Protocol; 단순 객체 접근 프로토콜) 기반의 프로토콜이다. WS-MAN은 시스템이 IT 인프라를 거쳐 운영 정보를 접근 및 교환할 수 있도록 하는 일반적인 방법을 제시한다.

# WMI 상호작용
다음은 [WMI](#wmi) 인터페이스를 제공하여 로컬 혹은 원격 시스템의 관리 정보 등을 불러올 수 있도록 하는 도구들을 목록이다.

* [WMI 명령줄 유틸리티](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmic)(WMI command-line utility; WMIC); [윈도우 10](https://ko.wikipedia.org/wiki/윈도우_10), [버전 21H2](https://ko.wikipedia.org/wiki/윈도우_10_버전_역사#21H2) 및 반기 채널 업데이트의 윈도우 서버부터 더이상 장려되지 않는다.
* [파워셸](PowerShell.md)
    * WMI cmdlet: [Microsoft.PowerShell.Management](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management) 모듈의 일부로 WMI에 대한 명령을 제공한다.
    * CIM cmdlet: [CimCmdlets](https://learn.microsoft.com/en-us/powershell/module/cimcmdlets) 모듈에서 (WMI를 포함한) [CIM](#일반-정보-모델)을 지원하는 WMI cmdlet의 상위호환이다.

아래 예시는 운영체제 빌드 번호를 확인하기 위해 CIM 서버, 즉 [winmgmt](#wmi) [서비스](Service.md)로부터 [Win32_OperatingSystem](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/win32-operatingsystem) [클래스](https://learn.microsoft.com/en-us/windows/win32/wmisdk/retrieving-a-class) 인스턴스를 반환받는 방법을 WMIC와 파워셸의 CimCmdlets 모듈을 사용하여 비교한다.

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">WMI 명령 유티릴티 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">WMIC</th><th style="text-align: center;">파워셸 CimCmdlets</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```terminal
WMIC /OUTPUT:clipboard PATH Win32_OperatingSystem GET BuildNumber /VALUE
```
</td><td>

```powershell
Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object BuildNumber
```
</td></tr><tr style="vertical-align: top;"><td>

```terminal
BuildNumber=22621
```
</td><td>

```terminal
BuildNumber
-----------
22621
```
</td></tr></tbody></table>

본 문서는 파워셸의 CimCmdlets 모듈을 활용한 명령을 위주로 WMI 상호작용 예시를 보여준다.
