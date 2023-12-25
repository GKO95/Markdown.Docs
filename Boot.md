# 부팅
> *참고: [Booting process of Windows NT - Wikipedia](https://en.wikipedia.org/wiki/Booting_process_of_Windows_NT)*

[부팅](https://ko.wikipedia.org/wiki/부팅)(booting)은 하드웨어 (예. 전원 버튼) 또는 소프트웨어 명령으로 컴퓨터를 켜는 절차이다. 부팅 초기에 전력을 공급받은 컴퓨터의 [메모리](Memory.md)가 [휘발성](https://ko.wikipedia.org/wiki/휘발성_메모리)이기 때문에 아무런 소프트웨어가 로드되지 않은 상태이다. 그러므로 시스템 하드웨어의 [펌웨어](https://en.wikipedia.org/wiki/Firmware) 또는 타 [프로세서](Processor.md)의 도움으로 컴퓨터를 구동하기 위한 프로그램 이미지를 메모리에 주입시키는 작업이 필요하다.

아래는 시스템이 부팅되는 과정을 두 규격, [UEFI](#uefi)와 [BIOS](#bios)에 대하여 순차적으로 보여주는 도표이다.

![윈도우 NT 운영체제의 부팅 절차 트러블슈팅 (예시. 윈도우 10)](https://i0.wp.com/www.msnoob.com/wp-content/uploads/2019/01/boot-sequence.png?fit=1167%2C1107&ssl=1)

## 시동 자체 시험
**[시동 자체 시험](https://ko.wikipedia.org/wiki/시동_자체_시험)**(Power-on self-test), 일명 POST는 컴퓨터나 타 디지털 전자 장치가 전원을 공급받는 즉시 하드웨어 초기화 및 상태를 진단하는 [펌웨어](https://en.wikipedia.org/wiki/Firmware) 혹은 소프트웨어 루틴이다. POST는 부팅 초기에 [BIOS](#bios)와 [UEFI](#uefi) 모드에 공통적으로 진행되며, 검사를 통과하면 [부트로더](#부트로더)에 의해 운영체제가 로드 및 실행된다. 흔히 검은색 바탕에 [메인보드](https://ko.wikipedia.org/wiki/메인보드) 펌웨어 제조사의 로고가 표시되는 화면에 해당한다.

POST 진단 결과는 디스플레이 화면에 출력되거나 별도의 진단 도구로부터 확인할 수 있도록 저장된다. 만일 화면 출력 기능에 문제가 있을 경우를 대비하여 LED 또는 경고음을 통해 오류 코드를 알릴 수 있는 장치가 마련되어 있다.

## 부트로더
**[부트로더](https://en.wikipedia.org/wiki/Bootloader)**(bootloader)는 컴퓨터 부팅 과정 중에서 설치된 운영체제의 [커널](Kernel.md)을 불러와 실행하는 프로그램이다. 다른 명칭으로 **[부트스트랩](#부트스트랩) 로더**(bootstrap loader) 또는 **부트 관리자**(boot manager)라고도 불린다.

* [윈도우 부트 관리자](https://en.wikipedia.org/wiki/Windows_Boot_Manager): 일명 `BOOTMGR`은 [윈도우 NT](Windows.md)를 위한 부트로더이다.
* [GNU GRUB](https://en.wikipedia.org/wiki/GNU_GRUB): GNU 프로젝트의 일환으로 UNIX 기반의 운영체제를 위한 부트로더이다.

### 부트스트랩
**[부트스트랩](https://ko.wikipedia.org/wiki/부트스트랩_(컴퓨팅))**(bootstrap)은 아무런 외부 도움을 받지 않고 자체적으로 실행하는 절차를 가리킨다. 부트스트랩의 어원은 다음과 같다:

> 미국 관용구 "[pull yourself up by your bootstraps](https://en.wiktionary.org/wiki/pull_oneself_up_by_one%27s_bootstraps)," 즉 *스스로를 부트스트랩을 당겨 들어올린다*라는 표현이 있는 데 사실상 물리적으로 불가능하다. 하지만 현재는 "아무런 도움없이 어려움을 극복하고 성취하다"라는 의미로 변질되면서, 부트스트랩이란 용어가 이러한 의미를 함축하게 되었다.

컴퓨터에 전력이 공급되는 시점부터 운영체제가 로드될 때까지 자력으로 해내는 부트스트랩 과정을 일명 [부팅](#부팅)(booting)이라 부르게 된 것이다.

# BIOS
**[BIOS](https://en.wikipedia.org/wiki/BIOS)**(Basic Input/Output System)는 [부팅](#부팅) 과정에 하드웨어를 초기화 및 진단하고, [디스크](Disk.md)에 저장된 [부트로더](#부트로더)를 [메모리](Memory.md)에 로드하여 운영체제의 [커널](Kernel.md)을 실행시키는 [메인보드](https://en.wikipedia.org/wiki/Motherboard) [펌웨어](https://en.wikipedia.org/wiki/Firmware)이다. [IBM](https://en.wikipedia.org/wiki/IBM)에서 개발한 전매 소프트웨어였으나, [역설계](https://ko.wikipedia.org/wiki/역공학)를 성공한 이후 호환 PC 기종이 대거 생산되며 [사실상 표준](https://en.wikipedia.org/wiki/De_facto_standard)이 되었다. 새로운 [UEFI](#uefi)의 등장으로, 이를 구분하기 위해 "레거시" BIOS라고 흔히 언급된다.

BIOS가 부트 장치를 탐색하는 과정은 다음과 같다.

![BIOS 부팅 과정](https://upload.wikimedia.org/wikipedia/commons/2/20/Legacy_BIOS_boot_process_fixed.png)

콜드 부트(cold boot), 다시 말해 물리적 전원 버튼 또는 전력 공급으로 부팅이 시작된 경우 메인보드의 [ROM](https://ko.wikipedia.org/wiki/고정_기억_장치) 혹은 [플래시 메모리](https://ko.wikipedia.org/wiki/플래시_메모리)에 저장된 BIOS가 실행된다. BIOS에 의한 [POST](#시동-자체-시험) 절차가 마무리되면 [INT](Processor.md#인터럽트) [19h](https://en.wikipedia.org/wiki/BIOS_interrupt_call)를 호출하여 부트로더를 탐색, 로드, 그리고 실행한다. 부트로더가 위치한 [저장소](Disk.md)를 "부트 장치(boot device)"라고 부른다.

BIOS가 부트로더를 탐색하는 과정은 다음과 같다:

1. BIOS 설정이 저장된 [비휘발성 메모리](https://en.wikipedia.org/wiki/Nonvolatile_BIOS_memory)(대표적으로 [CMOS](https://en.wikipedia.org/wiki/CMOS))로부터 부트 장치 목록을 지정한 순서대로 살펴본다.
1. 부트 장치의 [부트 섹터](#부트-섹터)(즉, [MBR](#마스터-부트-레코드))를 메모리로 불러오고, 만일 해당 섹터를 읽을 수 없다면 다음 부트 장치로 넘어간다.
1. 부트 섹터를 읽을 수 있는 경우, 일부 BIOS는 마지막 두 시그니처 바이트 `0x55`, `0xAA`까지 검증하고 부팅 장치로 인식한다.

이후 BIOS는 메모리로 불러온 부트 섹터에게 PC 제어권을 양도한다. 다만, BIOS는 시그니처 바이트를 확인하는 것 외에 부트 섹터의 내용물을 판독하지 않는다.

## 부트 섹터
**[부트 섹터](https://en.wikipedia.org/wiki/Boot_sector)**(boot sector)는 시스템을 부팅하기 위해 필요한 코드, 즉 [부트로더](#부트로더)가 담겨있는 [저장소](Disk.md)의 [섹터](Disk.md#섹터)이다. 일반적으로 디스크의 가장 첫 섹터를 가리키며, 부트 섹터를 포함한 [디스크](Disk.md)를 "부트 장치(boot device)"라고 부른다.

다음은 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)에 사용되는 부트 섹터의 유형을 소개한다:

* [마스터 부트 레코드](#마스터-부트-레코드)(MBR)
* [볼륨 부트 레코드](#볼륨-부트-레코드)(VBR)

### 마스터 부트 레코드
**[마스터 부트 레코드](https://en.wikipedia.org/wiki/Master_boot_record)**(master boot record; MBR)는 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)을 위한 부트 섹터의 한 유형이며, [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive) 또는 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive) 등의 [파티션](Disk.md#파티션)을 나눌 수 있는 [대용량 저장 매체](Disk.md)([휴대용](https://en.wikipedia.org/wiki/Disk_enclosure) 포함)가 대상이다. 부트로더 뿐만 아니라, 해당 디스크의 [파티션 정보](https://en.wikipedia.org/wiki/Master_boot_record#PT)도 MBR에 저장되어 있다.

### 볼륨 부트 레코드
**[볼륨 부트 레코드](https://en.wikipedia.org/wiki/Volume_boot_record)**(volume boot record; VBR)는 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)을 위한 부트 섹터의 한 유형이며, 다음 두 경우가 대상이다.

* 파티션을 나눌 수 없는 [플로피 디스크](https://en.wikipedia.org/wiki/Floppy_disk), [CD](https://en.wikipedia.org/wiki/Compact_disc) 및 [DVD](https://en.wikipedia.org/wiki/DVD) 등의 데이터 저장 매체의 부트 섹터가 해당한다.
* 파티션을 나눌 수 있는 대용량 저장 매체에서 각 파티션의 첫 번째 섹터가 해당한다. 디스크 전반의 첫 번째 섹터는 여전히 MBR로써 파티션 정보를 저장하고 있다.

# UEFI
> *참고: [Boot and UEFI - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/bringup/boot-and-uefi)*

**[UEFI](https://en.wikipedia.org/wiki/UEFI)**(Unified Extensible Firmware Interface)는 [BIOS](#bios)의 기술적 한계를 극복하기 위해 설계된 [펌웨어](https://en.wikipedia.org/wiki/Firmware) 구조를 정의하는 규격이다. [인텔](https://www.intel.com/)에서 최초로 *EFI* 규격을 개발하였으나, 차후 [UEFI 포럼](https://en.wikipedia.org/wiki/UEFI_Forum)이란 산업 [컨소시엄](https://en.wikipedia.org/wiki/Consortium)에 합류하며 2006년에 *UEFI* [개방형 표준](https://en.wikipedia.org/wiki/Open_standard)을 발표하였다.

UEFI가 부트 장치를 탐색하는 과정은 다음과 같다.

![UEFI 부팅 과정](https://upload.wikimedia.org/wikipedia/commons/1/17/UEFI_boot_process.png)

시스템에 전원이 들어오면 부트 관리자는 [NVRAM](https://ko.wikipedia.org/wiki/비휘발성_메모리)에 저장된 설정을 확인하고, 이를 기반으로 특정 운영체제 부트로더 혹은 커널을 실행한다. UEFI는 컴퓨터 아키텍처마다 표준화된 파일 경로에 의존하여 부트로더를 스스로 찾아낼 수 있는데, USB 플래시 드라이브와 같은 장치로도 간편한 부팅을 가능케 한다.

텍스트 기반의 사용자 인터페이스를 갖춘 부트 매니저가 널러 배포되면서 사용자가 원하는 운영체제를 부트 옵션에서 선택할 수 있다.

## EFI 시스템 파티션
[EFI 시스템 파티션](https://en.wikipedia.org/wiki/EFI_system_partition), 일명 ESP는 UEFI 시스템에 탑재된 [보조 기억 장치](https://en.wikipedia.org/wiki/Computer_data_storage#Secondary_storage)의 파티션으로써, 부팅 당시 UEFI 펌웨어는 설치된 운영체제 및 다양한 유틸리티를 실행하기 위해 ESP에 위치한 파일을 불러온다. ESP 안에는 다른 파티션에 설치된 모든 운영체제의 부트로더 또는 커널 이미지, 컴퓨터에 인식된 하드웨어 장치 중 펌웨어가 부팅 단계에서 필요로 하는 장치 드라이버 파일, 운영체제가 부팅되기 전에 먼저 실행되어야 하는 시스템 유틸리티 프로그램, 그리고 오류 로그와 같은 데이터 파일이 저장된다.
