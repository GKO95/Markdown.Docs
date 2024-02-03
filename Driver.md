# 드라이버
**[드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-)**(driver)는 시스템에 장착된 [하드웨어 장치](https://en.wikipedia.org/wiki/Computer_hardware)를 제어하거나 [커널](Kernel.md#커널) 서비스에 접근할 수 있는 [로드 가능한 모듈](https://ko.wikipedia.org/wiki/적재_가능_커널_모듈)(loadable kernel moduel; LKM)이다. 즉, 드라이버는 필요에 따라 언제든지 [메모리](Memory.md)에 로드 및 해제될 수 있다. 흔히 하드웨어 상호작용에 활용되기 때문에 일반적으로 [*장치 드라이버*](#장치-드라이버)를 지칭하는 경우가 대다수이지만, 그렇지 않고 단지 [운영체제](https://ko.wikipedia.org/wiki/운영체제) 커널에서만 접근이 가능한 데이터를 다루기 위한 [*소프트웨어 드라이버*](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-#software-drivers)도 존재한다.

### 드라이버 객체
> *출처: [Introduction to Driver Objects - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-driver-objects)*

**드라이버 객체**(driver object)는 시스템에 설치되어 메모리에 로드된 각 드라이버를 나타내는 [DRIVER_OBJECT](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_driver_object) [구조체](C.md#구조체)의 객체이다. 커널의 [입출력 관리자](Kernel.md#입출력-관리자)가 관리하며, 드라이버의 [DriverEntry](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_initialize) 루틴 호출 시 해당 드라이버 객체의 주소를 제공한다. 드라이버 객체 안에는 드라이버의 표준 [루틴](C.md#함수)들의 [진입점](C.md#진입점)들이 저장되어 있다.

## 장치 드라이버
**[장치 드라이버](https://en.wikipedia.org/wiki/Device_driver)**(device driver)는 운영체제 커널과 하드웨어 장치 간 상호작용에 개입되는 드라이버를 가리킨다. [윈도우 NT](Windows.md)의 [PnP 관리자](Kernel.md#pnp-관리자)는 "[디바이스 트리](https://en.wikipedia.org/wiki/Tree_(data_structure))"로부터 시스템에 연결된 각 장치들을 [디바이스 노드](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/device-nodes-and-device-stacks)(device node; 장치 [노드](https://en.wikipedia.org/wiki/Node_(computer_science)))로 표현하여 관계를 체계화한다. 아래 그림은 디바이스 트리를 나타낸 예시를 보여준다.

![PnP 장치들의 관계를 트리 구조로 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/devicetree01.png)

디바이스 노드는 *[USB Device](https://en.wikipedia.org/wiki/USB)*, *[Audio Device](https://en.wikipedia.org/wiki/Computer_speakers)*, *[Monitor](https://en.wikipedia.org/wiki/Computer_monitor)* 등의 장치 외에도 시스템과 통신할 수 있도록 장치를 연결하는 [버스](https://en.wikipedia.org/wiki/Bus_(computing))도 나타낸다: *[USB Host Controller](https://en.wikipedia.org/wiki/Host_adapter)* *[Display Adapater](https://en.wikipedia.org/wiki/Graphics_card)*, *[PCI Express Port](https://en.wikipedia.org/wiki/PCI_Express)* 등이 해당한다. 다만, 디바이스 노드가 장치인지 버스인지 판별하는 건 관점에 따라 다르다.

1. *Display Adapter*는 화면에 나타날 프레임을 마련하는 핵심 역할의 "장치"
1. *Display Adpater*에 연결된 모니터를 감지, 열거, 그리고 영상 신호로 변환하여 송출하는 역할의 "버스"

PnP 관리자의 디바이스 트리를 구성하며 장치 (및 버스)와 시스템 간 상호작용을 가능케 하는 장치 드라이버는 다음 세 유형으로 나뉘어진다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">장치 드라이버 유형별 소개</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">장치 드라이버</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/function-drivers">기능 드라이버</a><br/>(function driver)</td><td>장치를 제어하기 위해 필요한 인터페이스를 제공하는 유일한 핵심 드라이버이다. 해당 하드웨어에 대하여 가장 잘 알고 있으며, 장치에 내장된 <a href="https://ko.wikipedia.org/wiki/하드웨어_레지스터">레지스터</a> 접근이 가능하다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/bus-drivers">버스 드라이버</a><br/>(bus driver)</td><td>PnP 관리자의 요청에 의해 버스에 연결된 장치를 열거하고, 인식된 장치는 자식 노드를 생성하여 관리하는 드라이버이다. 사실상 부모 노드로부터 상속받은 "기능 드라이버"이다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/filter-drivers">필터 드라이버</a><br/>(filter driver)</td><td>장치나 기존 드라이버에 기능을 추가, 혹은 입출력 요청이나 응답을 수정하는 부가적인 드라이버이다. 개수에 제한이 없고 선택사항이며, 개입되는 시점에 따라 아래와 같이 분류된다.<ul><li><i>버스 필터 드라이버</i>&nbsp;<sub>(버스 드라이버 이후)</sub></li><li><i>하위 필터 드라이버</i>&nbsp;<sub>(기능 드라이버 이전)</sub></li><li><i>상위 필터 드라이버</i>&nbsp;<sub>(기능 드라이버 이후)</sub></li></ul>하드웨어 제조사는 흔히 필터 드라이버를 활용해 장치로부터 발견된 이슈를 해결하곤 한다.</td></tr></tbody></table>

위에서 소개한 장치 드라이버 유형별 역할은 [디바이스 객체](#디바이스-객체)에서 다시 한번 언급된다.

* 버스는 또 다른 버스를 자식 노드로 가질 수 있다. 이러한 경우, PnP 관리자는 각 자식 버스에 대해서도 연결된 장치들을 열거할 것을 요청한다. 위의 예시에서 *PCI Express Port*는 *Display Adpater*가 연결된, 그리고 *Display Adpater*는 하나의 *Monitor*가 연결된 버스이다.

### 디바이스 객체
> *출처: [Introduction to Device Objects - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-device-objects)*

**디바이스 객체**(device object; 장치 객체)는 장치에 관여하는 [드라이버](#드라이버) 개수만큼 {디바이스 객체, [드라이버 객체](#드라이버-객체)} 쌍이 노드에 구성된다. 이러한 구조가 가진 의의를 설명하기 위해 아래 *PCI Bus*에 연결된 *Proseware Gizmo* 장치를 나타낸 디바이스 트리를 예시로 보여준다. 

![장치에 할애된 드라이버와 연계된 객체로 구성된 스택을 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/prosewaredevicenode02.png)

*Proseware Gizmo* 장치에는 총 세 개의 드라이버가 사용되고 있다: AfterThought.sys, Proseware.sys, 그리고 Pci.sys이다. 각 드라이버마다 하나의 디바이스 객체(DO 접미부)와 쌍을 이루며, 이들은 총 세 가지 유형으로 나뉘어진다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">디바이스 객체 유형별 소개</caption><colgroup><col style="width: 20%;"/><col style="width: 10%;"><col style="width: 70%;"/></colgroup><thead><tr><th colspan="2" style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/types-of-wdm-device-objects">디바이스 객체</a></th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">물리 디바이스 객체<br/>(physical device object)</td><td>PDO</td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/bus-drivers">버스 드라이버</a>와 쌍을 이루는 디바이스 객체이다. <a href="Kernel.md#pnp-관리자">PnP 관리자</a>의 요청으로 버스가 연결된 장치를 열거할 때 인식된 장치, 즉 자식 노드마다 PDO를 생성하여 디바이스 스택을 마련한다.</td></tr><tr><td style="text-align: center;">기능 디바이스 객체<br/>(functional device object)</td><td>FDO</td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/function-drivers">기능 드라이버</a>와 쌍을 이루는 디바이스 객체이다.</td></tr><tr><td style="text-align: center;">필터 디바이스 객체<br/>(filter device object)</td><td>Filter DO</td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/filter-drivers">필터 드라이버</a>와 쌍을 이루는 디바이스 객체이다.</td></tr></tbody></table>

디바이스 객체는 PnP의 장치 관리에 있어 다음과 같은 의의를 가진다:

1. [디바이스 스택](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/device-nodes-and-device-stacks#device-objects-and-device-stacks)(device stack)을 구성하여, 해당 디바이스 노드가 시스템과 상호작용을 하기 위해 개입되어야 할 드라이버 순서를 정의한다. "Filter DO-FDO-PDO" 순서의 스택을 가진 *Proseware Gizmo* 장치와 달리, 다른 PCI 장치의 스택은 "FDO-Filter DO-PDO" 순서로 객체가 나열되어 있다.

1. 비록 디바이스 객체는 드라이버 객체와 쌍을 이루지만, 스택을 구성하는 건 (*Proseware Gizmo* 장치의 경우) "AfterThought.sys-Proseware.sys-Pci.sys" 순서의 드라이버 객체가 아닌 "Filter DO-FDO-PDO" 순서의 디바이스 객체이다. 장치의 드라이버를 교체하여도 시스템에 미치는 영향을 최소화할 수 있다.

[DEVICE_OBJECT](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_device_object) [구조체](C.md#구조체)를 살펴보면 `AttachedDevice` 필드를 찾아볼 수 있는데, 이게 바로 스택상 다음 상위 디바이스 객체의 포인터를 가리킨다. 결국 *Proseware Gizmo* 노드에서 Pci.sys 이후에 Proseware.sys 드라이버가 있음을 디바이스 객체로부터 알 수 있게 된다.

* [WinDbg](WinDbg.md)에서 [`!devobj`](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/-devobj) 명령을 입력하면 `AttachedTo` 항목이 있어 이전 드라이버가 무엇인지 알려주지만, 이는 DEVICE_OBJECT의 필드가 아니다.

## 입출력 요청 패킷
**[입출력 요청 패킷](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/i-o-request-packets)**(I/O request packet; IRP)

# 윈도우 드라이버 모델
> *참고: [Introduction to WDM - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)*

**[윈도우 드라이버 모델](https://ko.wikipedia.org/wiki/윈도우_드라이버_모델)**(Windows Driver Model; WDM)은 [윈도우 98](https://ko.wikipedia.org/wiki/윈도우_98) 및 [윈도우 2000](https://ko.wikipedia.org/wiki/윈도우_2000)과 함께 소개되어 [윈도우 NT](Windows.md)에서 동작할 장치 드라이버 설계를 규정 및 표준화하는 프레임워크이다. 윈도우 운영체제가 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 기반에서 NT 계열로 전환되면서 이전에 사용하던 [VxD](https://ko.wikipedia.org/wiki/VxD) 프레임워크를 대체한다. WDM은 [상위호환](https://ko.wikipedia.org/wiki/상위_호환성)을 지원하도록 설계되어, 빌드 대상보다 이전 버전의 윈도우 OS에서는 정상 동작을 보장하지 않지만 이후 버전에서는 문제없이 구동된다.

### 윈도우 드라이버 프레임워크
**[윈도우 드라이버 프레임워크](https://ko.wikipedia.org/wiki/윈도우_드라이버_프레임웍스)**(Windows Driver Frameworks; WDF), 혹은 윈도우 드라이버 파운데이션(Windows Driver Foundation)는 [WDM](#윈도우-드라이버-모델) 기반의 커널 모드 및 사용자 모드 드라이버를 훨씬 간편하고 효율적으로 작성할 수 있도록 하는 [오픈 소스](https://github.com/Microsoft/Windows-Driver-Frameworks) "개발" 프레임워크이다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">WDF에서 제공하는 프레임워크</caption><colgroup><col style="width: 33.4%;"/><col style="width: 33.3%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Kernel-Mode_Driver_Framework">커널 모드 드라이버 프레임워크</a>(KMDF)</th><th colspan="2" style="text-align: center; border-bottom-style: none;"><a href="https://en.wikipedia.org/wiki/User-Mode_Driver_Framework">사용자 모드 드라이버 프레임워크</a>(UMDF)</th></tr><th style="text-align: center;">버전 1</th><th style="text-align: center;">버전 2</th><tr></tr></thead><tbody><tr><td>WDM을 직접 작성하는 것보다 간편하고 효율적인 개발을 가능케 하는 객체 기반이다.</td><td>어플리케이션 개발자에게 친숙한 <a href="Cpp.md">C++</a> 및 COM을 활용하지만 KMDF와 상당히 이질적이다.</td><td>KMDF와 동일한 객체 모델을 채택한다.</td></tr></tbody></table>

드라이버를 개발할 때 WDM이 아닌 WDF 모델을 사용할 것을 마이크로스프트는 강력하고 권장하며, 해당 내용은 [여기](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)에서 확인할 수 있다.
