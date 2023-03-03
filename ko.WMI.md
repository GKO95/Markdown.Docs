---
category: 윈도우
title: 윈도우 관리 도구
---
# WMI
[윈도우 관리 도구](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page)(Windows Management Instrumentation; WMI)는 [윈도우](ko.Windows.md) 기반의 운영체제에서 동작하는 [마이크로소프트](https://www.microsoft.com) 버전의 [웹 기반 기업 관리](#웹-기반-기업-관리) 도구이다.

## 웹 기반 기업 관리
[웹 기반 기업 관리](https://en.wikipedia.org/wiki/Web-Based_Enterprise_Management)(Web-Based Enterprise Management), 일명 WBEM은 컴퓨터가 다른 네트워크에 위치하지만 하나의 시스템을 구성하는 [분산 컴퓨팅](https://ko.wikipedia.org/wiki/분산_컴퓨팅) 환경에서의 관리를 단일화하기 위한 [시스템 관리](https://en.wikipedia.org/wiki/Systems_management) 표준 기술로써 관리 정보를 접근한는 데 활용된다.

> 비록 "웹 기반"이라고 명시되어 있으나, WBEM은 [사용자 인터페이스](https://ko.wikipedia.org/wiki/사용자_인터페이스) 표준을 규정하지 않으므로 [인터넷 브라우저](https://en.wikipedia.org/wiki/Browser_user_interface)가 아니더라도 [GUI](https://ko.wikipedia.org/wiki/그래픽_사용자_인터페이스) 혹은 [CLI](https://ko.wikipedia.org/wiki/명령_줄_인터페이스) 등도 사용할 수 있다.

WBEM은 아래의 표준들로부터 기반한다:

### 일반 정보 모델
[일반 정보 모델](https://ko.wikipedia.org/wiki/일반_정보_모델_(컴퓨팅))(Common Information Model), 일명 CIM은 [데스크탑](https://ko.wikipedia.org/wiki/워크스테이션), [네트워크 라우터](https://ko.wikipedia.org/wiki/라우터), [어플리케이션](https://ko.wikipedia.org/wiki/응용_소프트웨어) 등 기업의 IT 환경을 구성하여 운영되는 요소들을 일반적인 객체 지향 데이터로 반영한 모델이다. 단순히 운영정보 교환에 그치지 않으며, 이들을 적극적으로 제어하고 관리할 수 있는 기능을 함께 제공한다. CIM의 공통 모델이 적용된 요소들은 하나의 IT 환경 관리 소프트웨어로도 복잡하거나 무거운 변환 작업 또는 정보의 손실 없이도 다양한 구성들과 상호작용이 가능하다.

CIM 표준은 다음 내용들을 소개한다:

* **CIM 기반 구조 사양 (CIM Infrastructure Specification)**

    CIM 구조 및 개념을 정의한다: 다른 정보 모델(예를 들어 [SNMP](https://ko.wikipedia.org/wiki/간이_망_관리_프로토콜))하고 매핑되는 방법과 CIM 스키마가 정의된 언어가 무엇인지 등을 내포한다. CIM 구조는 객체 지향으로써 IT 환경의 구성요소와 이들의 관계성을 각각 CIM [클래스](ko.Csharp.md#클래스) 및 [연관](https://en.wikipedia.org/wiki/Association_(object-oriented_programming))으로 나타내고, [상속](ko.Csharp.md#상속)을 활용하여 파생 CIM 클래스 정의도 가능케 한다.

* **[CIM 스키마](https://en.wikipedia.org/wiki/CIM_Schema) (CIM Schema)**

    IT 환경 내에서 운영되는 요소들의 공통 기반을 나타낸 특정 객체들의 집합과 이들의 관계를 정의하는 개념적 [스키마](https://ko.wikipedia.org/wiki/XML_스키마_(W3C))이다. 오늘날 IT 환경에서 사용되는 컴퓨터 시스템([`CIM_ComputerSystem`](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-computersystem)), 운영 체계([`CIM_OperatingSystem`](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-operatingsystem)), 프로세스([`CIM_Process`](https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/cim-process)), 네트워크, 미들웨어, 보조 기억 장치 등 대부분의 요소를 다룬다. 제품 및 제조사에 특화된 기능도 함께 반영될 수 있도록 확장성을 지원한다.

### 웹 서비스 관리
[웹 서비스 관리](https://learn.microsoft.com/en-us/windows/win32/winrm/ws-management-protocol)(Web Service-Management), 일명 WS-Management 혹은 WS-MAN은 서버, 장치, 어플리케이션 그리고 다양한 [웹 서비스](https://ko.wikipedia.org/wiki/웹_서비스)를 관리하기 위한 [SOAP](https://ko.wikipedia.org/wiki/SOAP)(Simple Object Access Protocol; 단순 객체 접근 프로토콜) 기반의 프로토콜이다. WS-MAN은 시스템이 IT 인프라를 거쳐 운영 정보를 접근 및 교환할 수 있도록 하는 일반적인 방법을 제시한다.
