---
category: 운영체제
title: 부팅
---
# 부팅
[부팅](https://ko.wikipedia.org/wiki/부팅)(booting)은 버튼과 같은 하드웨어 혹은 소프트웨어 명령으로 컴퓨터를 켜는 절차이다. 부팅 초기에 전력을 공급받은 컴퓨터의 [메모리](ko.Memory.md)는 [휘발성](https://ko.wikipedia.org/wiki/휘발성_메모리)에 의해 아무런 소프트웨어도 로드되지 않은 상태로 [CPU](ko.Processor.md)는 어떠한 작업도 수행할 수 없는 데, 시스템 하드웨어나 CPU 펌웨어 혹은 또 다른 프로세서의 도움으로 CPU가 실행할 작업을 메모리에 주입시키는 작업이 필요하다.

![윈도우 NT 운영체제의 부팅 절차 트러블슈팅 (예시. 윈도우 10)](https://i0.wp.com/www.msnoob.com/wp-content/uploads/2019/01/boot-sequence.png?fit=1167%2C1107&ssl=1)

### 부트스트랩
[부트스트랩](https://ko.wikipedia.org/wiki/부트스트랩_(컴퓨팅))(bootstrap)은 외부로부터 아무런 도움을 받지 않고 자체적으로 실행하는 절차를 가리킨다. 부트스트랩 어원의 유래는 다음과 같다:

> 미국 관용구 "[pull yourself up by your bootstraps](https://en.wiktionary.org/wiki/pull_oneself_up_by_one%27s_bootstraps)," 즉 *스스로를 부트스트랩을 당겨 들어올린다*라는 표현이 있는 데 사실상 물리적으로 불가능하다. 하지만 현재는 "아무런 도움없이 어려움을 극복하고 성취하다"라는 의미로 변질되면서, 부트스트랩이란 용어가 이러한 의미를 함축하게 되었다.

컴퓨터에 전력이 공급되는 시점부터 운영체제가 로드될 때까지 자력으로 해내는 부트스트랩 과정을 일명 [부팅](#부팅)(booting)이라 부르게 된 것이다.

## 시동 자체 시험
[시동 자체 시험](https://ko.wikipedia.org/wiki/시동_자체_시험)(Power-on self-test), 일명 POST는 컴퓨터나 타 디지털 전자 장치가 전원을 공급받는 즉시 하드웨어 초기화 및 상태를 진단하는 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 혹은 소프트웨어 루틴이다. [BIOS](#bios) 또는 [UEFI](#uefi) 규격에 따르는 메인보드 펌웨어에 의해 POST가 진행되고, 검사를 통과하면 [부트로더](https://en.wikipedia.org/wiki/Bootloader)로부터 운영체제를 불러와 실행한다.

흔히 컴퓨터 전원을 켤 때 나타나는 검은색 바탕에 [메인보드](https://ko.wikipedia.org/wiki/메인보드) 펌웨어 제조사의 로고가 표시되는 화면이 POST가 이루어지는 단계이며, 결과는 장치의 디스플레이 화면에 출력되거나 차후 진단 도구로부터 확인할 수 있도록 저장된다. 만일 화면 출력 기능에 문제가 발견될 경우를 대비하여 LED 또는 경고음을 통해 오류 코드를 알릴 수 있는 장치가 마련되어 있다.

# BIOS
[BIOS](https://ko.wikipedia.org/wiki/바이오스)(Basic Input/Output System)는 [운영체제](https://ko.wikipedia.org/wiki/운영체제)를 시스템에 로드하기 전에 하드웨어 초기화를 시행하는 [메인보드](https://ko.wikipedia.org/wiki/메인보드)의 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어)이다. [BIOS 인터럽트 호출](https://ko.wikipedia.org/wiki/%바이오스_인터럽트_호출)를 제공하므로써 운영체제가 요청한 하드웨어 제어 및 입출력 함수 실행, 그리고 시스템 정보의 반환이 이루어진다.

> BIOS는 원래 IBM 소유의 기술로써 IBM 기종의 PC에서만 탑재되었으나, 호환된 시스템을 만들고자 한 일부 기업에서 [역설계](https://ko.wikipedia.org/wiki/역공학)에 성공하여 BIOS 대중화에 기여하였다.

시스템에 전력이 공급되거나 재시작 버튼이 눌리는, 일명 콜드 부트(cold boot)가 된 즉시 메인보드의 [ROM](https://ko.wikipedia.org/wiki/고정_기억_장치) 혹은 [플래시 메모리](https://ko.wikipedia.org/wiki/플래시_메모리)에 저장된 BIOS가 실행되어 하드웨어 초기화 및 [자가 진단](#시동-자체-시험)을 진행한다. 이후 BIOS는 인터럽트 (간단히 INT) `19h`를 호출하여 운영제체를 실행시키는 [부트로더](https://en.wikipedia.org/wiki/Bootloader)(bootloader)가 위치할 [HDD](https://ko.wikipedia.org/wiki/하드_디스크_드라이브) 및 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브), [CD](https://ko.wikipedia.org/wiki/콤팩트_디스크) 또는 [DVD](https://ko.wikipedia.org/wiki/DVD) 등의 [보조 기억 장치](https://en.wikipedia.org/wiki/Computer_data_storage#Secondary_storage), 즉 부트 장치(boot device)를 탐색한다.

BIOS가 부트 장치를 탐색하는 과정은 다음과 같다:

![BIOS 부팅 과정](https://upload.wikimedia.org/wikipedia/commons/2/20/Legacy_BIOS_boot_process_fixed.png)

1. [비휘발성 BIOS 메모리](https://en.wikipedia.org/wiki/Nonvolatile_BIOS_memory)([CMOS](https://ko.wikipedia.org/wiki/CMOS) 혹은 [NVRAM](https://ko.wikipedia.org/wiki/비휘발성_메모리) 사용)에 저장된 부트 장치들의 집합을 열거한다.
1. 열거된 부트 장치의 [부트 섹터](https://ko.wikipedia.org/wiki/부트_섹터)(boot sector)를 우선순위에 따라 불러오는 데, 만일 인식이 불가하면 다음 부트 장치로 넘어간다.
1. 부트 섹터를 성공적으로 인식하였을 시, 일부 BIOS는 해당 부트 섹터의 마지막 두 바이트(일명 시그니처)가 `0x55`, `0xAA`인지 확인한 다음 PC 제어권을 양도한다.

부트 섹터는 흔히 [MBR](https://ko.wikipedia.org/wiki/마스터_부트_레코드)(master boot record)이란 명칭으로 잘 알려져 있으며, 파일 시스템을 지닌 파티션이 해당 디스크의 섹터로부터 어떻게 구성되어 나뉘어졌는지 정보가 들어있다. 또한 설치된 운영체제를 불러오기 위한 실행 코드, 즉 부트로더가 위치하여 PC 제어권을 취득한 이후 다음 부팅 단계로 진입한다.

# UEFI
[UEFI](https://ko.wikipedia.org/wiki/통일_확장_펌웨어_인터페이스)(Unified Extensible Firmware Interface)는 기존 [BIOS](#bios)가 가지던 기술적 한계를 극복하기 위한 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어)와 [운영체제](https://ko.wikipedia.org/wiki/운영체제) 간의 인터페이스 규격이다. 특히 BIOS와 달리 부트 섹터에 의존하지 않는 대신 부트 관리자(boot manager)를 도입하였다.

> [윈도우 NT](ko.WindowsNT.md)의 경우에는 비스타부터 소개된 "[윈도우 부트 관리자](https://ko.wikipedia.org/wiki/윈도우_비스타_시작_프로세스)(Windows Boot Manager)"가 존재한다.

![UEFI 부팅 과정](https://upload.wikimedia.org/wikipedia/commons/1/17/UEFI_boot_process.png)

시스템에 전원이 들어오면 부트 관리자는 [NVRAM](https://ko.wikipedia.org/wiki/비휘발성_메모리)에 저장된 설정을 확인하고, 이를 기반으로 특정 운영체제 부트로더 혹은 커널을 실행한다. UEFI는 컴퓨터 아키텍처마다 표준화된 파일 경로에 의존하여 부트로더를 스스로 찾아낼 수 있는데, USB 플래시 드라이브와 같은 장치로도 간편한 부팅을 가능케 한다.

텍스트 기반의 사용자 인터페이스를 갖춘 부트 매니저가 널러 배포되면서 사용자가 원하는 운영체제를 부트 옵션에서 선택할 수 있다.

## EFI 시스템 파티션
[EFI 시스템 파티션](https://en.wikipedia.org/wiki/EFI_system_partition), 일명 ESP는 UEFI 시스템에 탑재된 [보조 기억 장치](https://en.wikipedia.org/wiki/Computer_data_storage#Secondary_storage)의 파티션으로써, 부팅 당시 UEFI 펌웨어는 설치된 운영체제 및 다양한 유틸리티를 실행하기 위해 ESP에 위치한 파일을 불러온다. ESP 안에는 다른 파티션에 설치된 모든 운영체제의 부트로더 또는 커널 이미지, 컴퓨터에 인식된 하드웨어 장치 중 펌웨어가 부팅 단계에서 필요로 하는 장치 드라이버 파일, 운영체제가 부팅되기 전에 먼저 실행되어야 하는 시스템 유틸리티 프로그램, 그리고 오류 로그와 같은 데이터 파일이 저장된다.
