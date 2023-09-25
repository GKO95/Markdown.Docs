# 장치 드라이버
[장치 드라이버](https://ko.wikipedia.org/wiki/장치_드라이버)(device driver)는 시스템에 연결된 특정 [하드웨어](https://ko.wikipedia.org/wiki/컴퓨터_하드웨어) [장치](https://ko.wikipedia.org/wiki/주변기기)를 동작하거나 제어하는 [로드 가능한 모듈](https://ko.wikipedia.org/wiki/적재_가능_커널_모듈)(loadable kernel moduel; LKM)이다. 다시 말해, 장치 드라이버는 필요에 따라 언제든지 [커널](Kernel.md#커널) [메모리](Memory.md)에 로드 및 언로드 될 수 있다. 커널의 [입출력 관리자](Kernel.md#입출력-관리자)와 실질적인 장치 제어를 담당하는 [HAL](Kernel.md#하드웨어-추상-계층) 간에 [인터페이스](https://ko.wikipedia.org/wiki/인터페이스_(컴퓨팅))를 제공하는데, 이를 통해 장치에 대한 상세한 정보를 알지 않아도 상호작용이 가능해진다.

## 윈도우 드라이버 모델
> *참고: [Introduction to WDM - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/introduction-to-wdm)*

[윈도우 드라이버 모델](https://ko.wikipedia.org/wiki/윈도우_드라이버_모델)(Windows Driver Model; WDM)은 [윈도우 98](https://ko.wikipedia.org/wiki/윈도우_98) 및 [윈도우 2000](https://ko.wikipedia.org/wiki/윈도우_2000)과 함께 소개되어 [윈도우 NT](Windows.md)에서 동작할 장치 드라이버 설계를 규정 및 표준화하는 프레임워크이다. 윈도우 운영체제가 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 기반에서 NT 계열로 전환되면서 이전에 사용하던 [VxD](https://ko.wikipedia.org/wiki/VxD) 프레임워크를 대체한다. WDM은 [상위호환](https://ko.wikipedia.org/wiki/상위_호환성)을 지원하도록 설계되어, 빌드 대상보다 이전 버전의 윈도우 OS에서는 정상 동작을 보장하지 않지만 이후 버전에서는 문제없이 구동된다.

아래는 WDM의 [드라이버 스택](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/driver-stacks)과 이를 구성하는 세 가지 유형의 WDM 드라이버를 소개한다.

![드라이버 스택으로 보는 WDM 드라이버 유형 간의 관계](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/images/drvlyr.png)

* **[버스 드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/bus-drivers)(Bus drivers)**

    [버스](https://ko.wikipedia.org/wiki/버스_(컴퓨팅)), 즉 컴퓨터 내 구성요소 간 혹은 컴퓨터 간에 데이터를 전송하는 통신 시스템(예를 들어 [PCI](https://ko.wikipedia.org/wiki/PCI_버스), [SATA](https://ko.wikipedia.org/wiki/직렬_ATA), [USB](https://ko.wikipedia.org/wiki/USB) 등)을 위한 드라이버이다. 시스템에서 절대적으로 필요한 드라이버로써 일반적인 경우 [마이크로소프트](https://www.microsoft.com/)에서 윈도우 운영체제와 함께 제공한다. 인식된 장치는 [PnP 관리자](Kernel.md#PnP-관리자)로 보고된다.

* **[기능 드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/function-drivers)(Function drivers)**

    하드웨어 장치를 제어하기 위해 필요한 인터페이스를 제공하는 장치 드라이버의 핵심이다. 기능 드라이버야 말로 해당 하드웨어에 대하여 가장 잘 알고 있으며, 장치에 내장된 [레지스터](https://ko.wikipedia.org/wiki/하드웨어_레지스터)를 접근할 수 있는 유일한 드라이버이다.

* **[필터 드라이버](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/filter-drivers)(Filter drivers)**

    장치나 기존 드라이버에 기능을 추가하거나, 혹은 입출력 요청이나 응답을 수정하는 부가적인 드라이버이다. 개수에 제한이 없고 선택사항인 관계로, 흔히 장치에서 발견된 문제를 고치기 위해 하드웨어 제조사나 벤더사에서 제공하는 경우가 많다.

    * 버스 필터 드라이버(Bus filter driver)
    * 하위 필터 드라이버(Lower-level filter driver)
    * 상위 필터 드라이버(Upper-level filter driver)

WDM 환경에서는 어떤 드라이버도 위에 언급한 세 가지 측면을 모두 포용할 수 없다.

### 윈도우 드라이버 프레임워크
> *참고: [Microsoft/Windows-Driver-Frameworks | GitHub](https://github.com/Microsoft/Windows-Driver-Frameworks)*

[윈도우 드라이버 프레임워크](https://ko.wikipedia.org/wiki/윈도우_드라이버_프레임웍스)(Windows Driver Frameworks; WDF), 혹은 윈도우 드라이버 파운데이션(Windows Driver Foundation)는 WDM 기반의 커널 모드 및 사용자 모드 드라이버를 훨씬 간편하고 효율적으로 작성할 수 있도록 하는 "개발" 프레임워크이다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">WDF에서 제공하는 프레임워크</caption>
<colgroup><col style="width: 33.4%;"/><col style="width: 33.3%;"/><col style="width: 33.3%;"/></colgroup>
<thead><tr><th rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Kernel-Mode_Driver_Framework">커널 모드 드라이버 프레임워크</a>(KMDF)</th><th colspan="2" style="text-align: center; border-bottom-style: none;"><a href="https://en.wikipedia.org/wiki/User-Mode_Driver_Framework">사용자 모드 드라이버 프레임워크</a>(UMDF)</th></tr><th style="text-align: center;">버전 1</th><th style="text-align: center;">버전 2</th><tr></tr></thead>
<tbody><tr><td>WDM을 직접 작성하는 것보다 간편하고 효율적인 개발을 가능케 하는 객체 기반이다.</td><td>어플리케이션 개발자에게 친숙한 <a href="Cpp.md">C++</a> 및 COM을 활용하지만 KMDF와 상당히 이질적이다.</td><td>KMDF와 동일한 객체 모델을 채택한다.</td></tr></tbody>
</table>
