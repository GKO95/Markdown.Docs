# 드라이버
> 본 내용은 [디바이스](Device.md)에 이해도가 매우 밀접하게 요구되므로 문서를 병독하는 걸 적극 권장한다.

[드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-)(driver)는 시스템에 장착된 특정 [하드웨어](https://ko.wikipedia.org/wiki/컴퓨터_하드웨어) [장치](Device.md)를 제어하거나 [커널](Kernel.md#커널) 서비스에 접근할 수 있는 [로드 가능한 모듈](https://ko.wikipedia.org/wiki/적재_가능_커널_모듈)(loadable kernel moduel; LKM)이다. 즉, 드라이버는 필요에 따라 언제든지 [메모리](Memory.md)에 로드 및 해제될 수 있다. 흔히 하드웨어 상호작용에 활용되기 때문에 일반적으로 [*장치 드라이버*](https://ko.wikipedia.org/wiki/장치_드라이버)를 지칭하는 경우가 대다수이지만, 그렇지 않고 단지 운영체제 커널에서만 접근이 가능한 데이터를 다루기 위한 [*소프트웨어 드라이버*](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/what-is-a-driver-#software-drivers)도 존재한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">장치 드라이버 유형별 소개</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">장치 드라이버</th><th style="text-align: center;">개요</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/function-drivers">기능 드라이버</a><br/>(function driver)</td><td>장치를 제어하기 위해 필요한 인터페이스를 제공하는 유일한 핵심 드라이버이다. 해당 하드웨어에 대하여 가장 잘 알고 있으며, 장치에 내장된 <a href="https://ko.wikipedia.org/wiki/하드웨어_레지스터">레지스터</a> 접근이 가능하다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/bus-drivers">버스 드라이버</a><br/>(bus driver)</td><td>장치가 (<a href="https://ko.wikipedia.org/wiki/버스_(컴퓨팅)">버스</a>가 아닌) <a href="https://ko.wikipedia.org/wiki/운영체제">운영체제</a>와 통신할 수 있도록 하는 드라이버이다. 그럼에도 불구하고 "버스"라는 명칭이 붙게 된 건 시스템에 연결된 장치를 <a href="Kernel.md#pnp-관리자">PnP 관리자</a>가 버스를 통해 열거하고 인식하기 때문이다. 장치의 버스 드라이버는 해당 장치가 연결된 "버스의 기능 드라이버"이기도 한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/filter-drivers">필터 드라이버</a><br/>(filter driver)</td><td>장치나 기존 드라이버에 기능을 추가, 혹은 입출력 요청이나 응답을 수정하는 부가적인 드라이버이다. 개수에 제한이 없고 선택사항이며, 개입되는 시점에 따라 아래와 같이 분류된다.<ul><li>버스 필터 드라이버: 버스 드라이버 이후</li><li>하위 필터 드라이버: 기능 드라이버 이전</li><li>상위 필터 드라이버: 기능 드라이버 이후</li></ul>하드웨어 제조사는 흔히 필터 드라이버를 활용해 장치로부터 발견된 이슈를 해결하곤 한다.</td></tr></tbody></table>

하드웨어 장치에 대하여 한 개 이상의 장치 드라이버가 특정 순서에 따라 로드될 수 있으며, 이에 대한 내용은 [디바이스 객체](Device.md#디바이스-객체)에서 다시 한번 설명한다.

## 입출력 요청 패킷
**[입출력 요청 패킷](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/i-o-request-packets)**(I/O request packet; IRP)

# 윈도우 드라이버 모델
> *참고: [Introduction to WDM - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)*

**[윈도우 드라이버 모델](https://ko.wikipedia.org/wiki/윈도우_드라이버_모델)**(Windows Driver Model; WDM)은 [윈도우 98](https://ko.wikipedia.org/wiki/윈도우_98) 및 [윈도우 2000](https://ko.wikipedia.org/wiki/윈도우_2000)과 함께 소개되어 [윈도우 NT](Windows.md)에서 동작할 장치 드라이버 설계를 규정 및 표준화하는 프레임워크이다. 윈도우 운영체제가 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 기반에서 NT 계열로 전환되면서 이전에 사용하던 [VxD](https://ko.wikipedia.org/wiki/VxD) 프레임워크를 대체한다. WDM은 [상위호환](https://ko.wikipedia.org/wiki/상위_호환성)을 지원하도록 설계되어, 빌드 대상보다 이전 버전의 윈도우 OS에서는 정상 동작을 보장하지 않지만 이후 버전에서는 문제없이 구동된다.

### 윈도우 드라이버 프레임워크
**[윈도우 드라이버 프레임워크](https://ko.wikipedia.org/wiki/윈도우_드라이버_프레임웍스)**(Windows Driver Frameworks; WDF), 혹은 윈도우 드라이버 파운데이션(Windows Driver Foundation)는 [WDM](#윈도우-드라이버-모델) 기반의 커널 모드 및 사용자 모드 드라이버를 훨씬 간편하고 효율적으로 작성할 수 있도록 하는 [오픈 소스](https://github.com/Microsoft/Windows-Driver-Frameworks) "개발" 프레임워크이다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">WDF에서 제공하는 프레임워크</caption><colgroup><col style="width: 33.4%;"/><col style="width: 33.3%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Kernel-Mode_Driver_Framework">커널 모드 드라이버 프레임워크</a>(KMDF)</th><th colspan="2" style="text-align: center; border-bottom-style: none;"><a href="https://en.wikipedia.org/wiki/User-Mode_Driver_Framework">사용자 모드 드라이버 프레임워크</a>(UMDF)</th></tr><th style="text-align: center;">버전 1</th><th style="text-align: center;">버전 2</th><tr></tr></thead><tbody><tr><td>WDM을 직접 작성하는 것보다 간편하고 효율적인 개발을 가능케 하는 객체 기반이다.</td><td>어플리케이션 개발자에게 친숙한 <a href="Cpp.md">C++</a> 및 COM을 활용하지만 KMDF와 상당히 이질적이다.</td><td>KMDF와 동일한 객체 모델을 채택한다.</td></tr></tbody></table>

드라이버를 개발할 때 WDM이 아닌 WDF 모델을 사용할 것을 마이크로스프트는 강력하고 권장하며, 해당 내용은 [여기](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)에서 확인할 수 있다.
