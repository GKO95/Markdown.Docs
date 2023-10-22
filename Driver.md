# 드라이버
> *출처: [What is a driver? - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-)*

[드라이버](https://ko.wikipedia.org/wiki/장치_드라이버)(driver)는 시스템에 장착된 특정 [하드웨어](https://ko.wikipedia.org/wiki/컴퓨터_하드웨어) [장치](#장치)를 제어하거나 [커널](Kernel.md#커널) 서비스에 접근할 수 있는 [로드 가능한 모듈](https://ko.wikipedia.org/wiki/적재_가능_커널_모듈)(loadable kernel moduel; LKM)이다. 즉, 드라이버는 필요에 따라 언제든지 [메모리](Memory.md)에 로드 및 언로드 될 수 있다. 흔히 시스템 하드웨어와 상호작용을 하기 위해 사용되기 때문에 일반적으로 *장치 드라이버*를 지칭하는 경우가 대다수이지만, 단지 운영체제 커널에서만 접근이 가능한 데이터를 다루기 위한 [*소프트웨어 드라이버*](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-#software-drivers)도 존재한다.

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">장치 드라이버 유형별 소개</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">장치 드라이버</th><th style="text-align: center;">개요</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/function-drivers">기능 드라이버</a><br/>(function driver)</td><td>장치를 제어하기 위해 필요한 인터페이스를 제공하는 유일한 핵심 드라이버이다. 해당 하드웨어에 대하여 가장 잘 알고 있으며, 장치에 내장된 <a href="https://ko.wikipedia.org/wiki/하드웨어_레지스터">레지스터</a> 접근이 가능하다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/bus-drivers">버스 드라이버</a><br/>(bus driver)</td><td>장치가 (<a href="https://ko.wikipedia.org/wiki/버스_(컴퓨팅)">버스</a>가 아닌) <a href="https://ko.wikipedia.org/wiki/운영체제">운영체제</a>와 통신할 수 있도록 하는 드라이버이다. 그럼에도 불구하고 "버스"라는 명칭이 붙게 된 건 시스템에 연결된 장치를 <a href="Kernel.md#PnP-관리자">PnP 관리자</a>가 버스를 통해 열거하고 인식하기 때문이다. 장치의 버스 드라이버는 해당 장치가 연결된 "버스의 기능 드라이버"이기도 한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/filter-drivers">필터 드라이버</a><br/>(filter driver)</td><td>장치나 기존 드라이버에 기능을 추가, 혹은 입출력 요청이나 응답을 수정하는 부가적인 드라이버이다. 개수에 제한이 없고 선택사항이며, 개입되는 시점에 따라 아래와 같이 분류된다.<ul><li>버스 필터 드라이버: 버스 드라이버 이후</li><li>하위 필터 드라이버: 기능 드라이버 이전</li><li>상위 필터 드라이버: 기능 드라이버 이후</li></ul>하드웨어 제조사는 흔히 필터 드라이버를 활용해 장치로부터 발견된 이슈를 해결하곤 한다.</td></tr></tbody></table>

위의 도표로부터 단일 하드웨어 장치에 한 개 이상의 드라이버가 정해진 순서에 따라 관여할 수 있음을 시사한다. 드라이버에 대한 추가 내용은 [디바이스 객체](#디바이스-객체) 및 [스택](#디바이스-스택)에서 다시 한번 설명할 예정이다.

## 윈도우 드라이버 모델
> *참고: [Introduction to WDM - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)*

[윈도우 드라이버 모델](https://ko.wikipedia.org/wiki/윈도우_드라이버_모델)(Windows Driver Model; WDM)은 [윈도우 98](https://ko.wikipedia.org/wiki/윈도우_98) 및 [윈도우 2000](https://ko.wikipedia.org/wiki/윈도우_2000)과 함께 소개되어 [윈도우 NT](Windows.md)에서 동작할 장치 드라이버 설계를 규정 및 표준화하는 프레임워크이다. 윈도우 운영체제가 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 기반에서 NT 계열로 전환되면서 이전에 사용하던 [VxD](https://ko.wikipedia.org/wiki/VxD) 프레임워크를 대체한다. WDM은 [상위호환](https://ko.wikipedia.org/wiki/상위_호환성)을 지원하도록 설계되어, 빌드 대상보다 이전 버전의 윈도우 OS에서는 정상 동작을 보장하지 않지만 이후 버전에서는 문제없이 구동된다.

### 윈도우 드라이버 프레임워크
[윈도우 드라이버 프레임워크](https://ko.wikipedia.org/wiki/윈도우_드라이버_프레임웍스)(Windows Driver Frameworks; WDF), 혹은 윈도우 드라이버 파운데이션(Windows Driver Foundation)는 [WDM](#윈도우-드라이버-모델) 기반의 커널 모드 및 사용자 모드 드라이버를 훨씬 간편하고 효율적으로 작성할 수 있도록 하는 [오픈 소스](https://github.com/Microsoft/Windows-Driver-Frameworks) "개발" 프레임워크이다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">WDF에서 제공하는 프레임워크</caption>
<colgroup><col style="width: 33.4%;"/><col style="width: 33.3%;"/><col style="width: 33.3%;"/></colgroup>
<thead><tr><th rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Kernel-Mode_Driver_Framework">커널 모드 드라이버 프레임워크</a>(KMDF)</th><th colspan="2" style="text-align: center; border-bottom-style: none;"><a href="https://en.wikipedia.org/wiki/User-Mode_Driver_Framework">사용자 모드 드라이버 프레임워크</a>(UMDF)</th></tr><th style="text-align: center;">버전 1</th><th style="text-align: center;">버전 2</th><tr></tr></thead>
<tbody><tr><td>WDM을 직접 작성하는 것보다 간편하고 효율적인 개발을 가능케 하는 객체 기반이다.</td><td>어플리케이션 개발자에게 친숙한 <a href="Cpp.md">C++</a> 및 COM을 활용하지만 KMDF와 상당히 이질적이다.</td><td>KMDF와 동일한 객체 모델을 채택한다.</td></tr></tbody>
</table>

# 디바이스
> *출처: [Device nodes and device stacks - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/device-nodes-and-device-stacks)*

[윈도우](Windows.md) [운영체제](https://ko.wikipedia.org/wiki/운영체제)는 시스템에 연결된 [장치](https://ko.wikipedia.org/wiki/주변기기)들을 [PnP](https://ko.wikipedia.org/wiki/플러그_앤_플레이) 장치 트리, 일명 "디바이스 트리"라는 [트리 구조](https://ko.wikipedia.org/wiki/트리_구조)의 노드로 표현하여 체계화하였다. 장치 노드(device node; 디바이스 노드)는 일반적으로 장치 또는 장치의 개별 기능, 아니면 간혹 물리적인 장치와 연관이 없는 소프트웨어 구성을 나타내기도 한다. 트리 구조의 특성상 PnP 환경의 장치 간 부모와 자식 관계를 파악할 수 있다.

![PnP 장치들의 관계를 트리 구조로 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/devicetree01.png)

일부 장치 노드는 [메인보드](https://ko.wikipedia.org/wiki/메인보드)에 탑재된 물리적 [PCI 버스](https://ko.wikipedia.org/wiki/PCI_버스)를 표현한 *PCI Bus* 노드처럼 하위 장치들이 연결될 수 있는 [버스](https://ko.wikipedia.org/wiki/버스_(컴퓨팅))를 나타낸다.

* PnP 관리자는 버스에 연결된 장치들의 목록을 해당 버스의 [드라이버](#드라이버)에게 열거하도록 요청하며, 열거된 장치들은 자식 노드로 표현된다. 위의 예시에는 *[USB](https://ko.wikipedia.org/wiki/USB) Host Controller*, *Audio Controller*, 그리고 *[PCI Express](https://ko.wikipedia.org/wiki/PCI_익스프레스) Port*를 포함한 PCI 버스에 연결된 장치들이 PCI 버스 노드의 자식 노드에 해당한다.

* 버스는 또 다른 버스를 자식 노드로 가질 수 있다. 이러한 경우, PnP 관리자는 각 자식 버스에 대해서도 연결된 장치들을 열거할 것을 요청한다. 위의 예시에서 *Audio Controller*는 오디오 장치, *PCI Express Port*는 디스플레이 어댑터, 그리고 *Display Adpater*는 하나의 모니터가 연결된 버스이다.

노드가 장치와 버스 중 무엇인지 판단하는 건 관점에 따라 다르다: 예를 들어, *Display Adapter*를 (1) 화면에 나타날 프레임을 마련하는 핵심 역할의 장치, 혹은 (2) 연결된 모니터를 감지하고 열거할 수 있는 버스라고 생각할 수 있다.

## 디바이스 객체
> *출처: [Introduction to Device Objects - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-device-objects)*

장치 객체(device object; 디바이스 객체)는 [`DEVICE_OBJECT`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_device_object) [구조체](C.md#구조체)로부터 객체화된 데이터이며, 장치를 제어하기 위해 할애된 [드라이버](#드라이버)당 하나씩 연계되어 쌍을 이룬다. 아래 예시는 PCI 버스에 연결된 Proseware Gizmo 장치를 나타낸 디바이스 트리를 보여준다. 

![장치에 할애된 드라이버와 연계된 객체로 구성된 스택을 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/prosewaredevicenode02.png)

Proseware Gizmo 장치에는 총 세 개의 드라이버가 사용된다: AfterThought.sys, Proseware.sys, 그리고 Pci.sys이다. 각 드라이버마다 하나의 디바이스 객체를 가지며 쌍을 이룬다. 여기서 DO는 디바이스 객체를 가리키며 세 가지 유형으로 나뉘어진다.

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">디바이스 객체 유형별 소개</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/types-of-wdm-device-objects">디바이스 객체 유형</a></th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">물리 디바이스 객체<br/>(physical device object; PDO)</td><td>버스 드라이버와 쌍을 이루는 객체 드라이버이다. <a href="Kernel.md#PnP-관리자">PnP 관리자</a>의 요청으로 버스가 연결된 장치를 열거할 때, 인식된 장치(즉 자식 노드)마다 PDO를 생성하여 <a href="#디바이스-스택">디바이스 스택</a>의 기반이 된다.</td></tr><tr><td style="text-align: center;">기능 디바이스 객체<br/>(functional device object; FDO)</td><td>기능 드라이버와 쌍을 이루는 디바이스 객체이다.</td></tr><tr><td style="text-align: center;">필터 디바이스 객체<br/>(filter device object; Filter DO)</td><td>필터 드라이버와 쌍을 이루는 디바이스 객체이다.</td></tr></tbody></table>

### 디바이스 스택
[드라이버](#드라이버)와 [디바이스 객체](#디바이스-객체)는 항상 일대일로 쌍을 이룬다: 장치에 관여하는 디바이스 객체의 개수, 유형, 그리고 순서 또한 드라이버와 마찬가지로 대응하며, 이러한 순차적으로 나열된 일련의 디바이스 객체들을 디바이스 스택(device stack)이라고 부른다. 다시 말해, 장치 노드마다 자신만의 디바이스 스택을 가진다. 스택에 생성된 첫 디바이스 객체는 최하단에 위치하고, 그리고 마지막으로 생성되어 스택에 정착한 디바이스 객체는 최상단에 위치한다.
