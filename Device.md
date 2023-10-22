# 디바이스
> 본 문서는 마이크로소프트 내용과 통일시키기 위해 기술용어에 한하여 다음과 같이 표기합니다: [장치](https://ko.wikipedia.org/wiki/장치)(한글) → [디바이스](https://en.wiktionary.org/wiki/디바이스)(영문)

[윈도우](Windows.md) [운영체제](https://ko.wikipedia.org/wiki/운영체제)는 시스템에 연결된 [장치](https://ko.wikipedia.org/wiki/컴퓨터_하드웨어)들을 [PnP](https://ko.wikipedia.org/wiki/플러그_앤_플레이) 장치 트리, 일명 "디바이스 트리"라는 [트리 구조](https://ko.wikipedia.org/wiki/트리_구조)의 노드로 표현하여 체계화하였다. [디바이스 노드](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/device-nodes-and-device-stacks)(device node; 장치 노드)는 일반적으로 장치 또는 장치의 개별 기능, 아니면 간혹 물리적인 장치와 연관이 없는 소프트웨어 구성을 나타내기도 한다. 트리 구조의 특성상 PnP 환경의 장치 간 부모와 자식 관계를 파악할 수 있다.

![PnP 장치들의 관계를 트리 구조로 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/devicetree01.png)

일부 디바이스 노드는 [메인보드](https://ko.wikipedia.org/wiki/메인보드)에 탑재된 물리적 [PCI 버스](https://ko.wikipedia.org/wiki/PCI_버스)를 표현한 *PCI Bus* 노드처럼 하위 장치들이 연결될 수 있는 [버스](https://ko.wikipedia.org/wiki/버스_(컴퓨팅))를 나타낸다.

* [PnP 관리자](Kernel.md#PnP-관리자)는 버스에 연결된 장치들의 목록을 해당 버스의 [드라이버](Driver.md)에게 열거하도록 요청하며, 열거된 장치들은 자식 노드로 표현된다. 위의 예시에는 *[USB](https://ko.wikipedia.org/wiki/USB) Host Controller*, *Audio Controller*, 그리고 *[PCI Express](https://ko.wikipedia.org/wiki/PCI_익스프레스) Port*를 포함한 PCI 버스에 연결된 장치들이 PCI 버스 노드의 자식 노드에 해당한다.

* 버스는 또 다른 버스를 자식 노드로 가질 수 있다. 이러한 경우, PnP 관리자는 각 자식 버스에 대해서도 연결된 장치들을 열거할 것을 요청한다. 위의 예시에서 *Audio Controller*는 오디오 장치, *PCI Express Port*는 디스플레이 어댑터, 그리고 *Display Adpater*는 하나의 모니터가 연결된 버스이다.

디바이스 노드가 장치와 버스 중 무엇인지 판단하는 건 관점에 따라 다르다: 예를 들어, *Display Adapter*를 (1) 화면에 나타날 프레임을 마련하는 핵심 역할의 장치, 혹은 (2) 연결된 모니터를 감지하고 열거할 수 있는 버스라고 생각할 수 있다.

## 디바이스 객체
> *출처: [Introduction to Device Objects - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-device-objects)*

디바이스 객체(device object; 장치 객체)는 장치에 관여하는 [드라이버](#드라이버) 개수만큼 (디바이스 객체, 드라이버) 쌍이 노드에 구성된다. 이러한 구조가 가진 의의를 설명하기 위해 아래 PCI 버스에 연결된 Proseware Gizmo 장치를 나타낸 디바이스 트리를 예시로 보여준다. 

![장치에 할애된 드라이버와 연계된 객체로 구성된 스택을 나타낸 예시 다이어그램](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/images/prosewaredevicenode02.png)

Proseware Gizmo 장치에는 총 세 개의 드라이버가 사용되고 있다: AfterThought.sys, Proseware.sys, 그리고 Pci.sys이다. 각 드라이버마다 하나의 디바이스 객체(DO 접미부)와 쌍을 이루며, 이들은 총 세 가지 유형으로 나뉘어진다.

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">디바이스 객체 유형별 소개</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/types-of-wdm-device-objects">디바이스 객체 유형</a></th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">물리 디바이스 객체<br/>(physical device object; PDO)</td><td>버스 드라이버와 쌍을 이루는 디바이스 객체이다. <a href="Kernel.md#PnP-관리자">PnP 관리자</a>의 요청으로 버스가 연결된 장치를 열거할 때, 인식된 장치(즉 자식 노드)마다 PDO를 생성하여 디바이스 스택을 마련한다.</td></tr><tr><td style="text-align: center;">기능 디바이스 객체<br/>(functional device object; FDO)</td><td>기능 드라이버와 쌍을 이루는 디바이스 객체이다.</td></tr><tr><td style="text-align: center;">필터 디바이스 객체<br/>(filter device object; Filter DO)</td><td>필터 드라이버와 쌍을 이루는 디바이스 객체이다.</td></tr></tbody></table>

여기서 *PDO-FDO-Filter DO* 순서로 나열된 Proseware Gizmo의 디바이스 스택(device stack)과 달리, 다른 PCI 장치의 스택은 *PDO-Filter DO-FDO* 순서로 객체가 나열된 걸 확인할 수 있다. 이는 장치와 운영체제가 서로 통신을 하는데 필요하는 드라이버 및 순서가 다르기 때문이다. 결론적으로 장치에 관여하는 드라이버의 입출력 정보를 관리하는 게 디바이스 객체의 의의이다.

[`DEVICE_OBJECT`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_device_object) [구조체](C.md#구조체)를 살펴보면 `AttachedDevice` 필드를 찾아볼 수 있는데, 이게 바로 다음 상위 드라이버(와 쌍을 이루는 디바이스 객체) 포인터를 가리킨다. 결국 Proseware Gizmo 노드에서 Pci.sys 이후에 Proseware.sys 드라이버가 있음을 디바이스 객체로부터 알 수 있게 된다.

* [WinDbg](WinDbg.md)에서 [`!devobj`](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/-devobj) 명령을 입력하면 `AttachedTo`라는 항목이 있어 이전 드라이버가 무엇인지 알려주지만, 이는 `DEVICE_OBJECT`의 필드가 아니다.
