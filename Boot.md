# 부팅
> *참고: [Booting process of Windows NT - Wikipedia](https://en.wikipedia.org/wiki/Booting_process_of_Windows_NT)*

**[부팅](https://en.wikipedia.org/wiki/Booting)**(booting)은 하드웨어 또는 소프트웨어 상호작용으로 컴퓨터를 켜는 절차이다. 부팅 초기에 전력을 공급받은 컴퓨터의 [메모리](Memory.md)가 [휘발성](https://en.wikipedia.org/wiki/Volatile_memory)이기 때문에 아무런 소프트웨어가 로드되지 않은 상태이다. 그러므로 시스템 하드웨어의 [펌웨어](https://en.wikipedia.org/wiki/Firmware) 또는 타 [프로세서](Processor.md)의 도움으로 컴퓨터를 구동하기 위한 프로그램 이미지를 메모리에 주입시키는 작업이 필요하다.

아래는 시스템이 부팅되는 과정을 두 규격, [UEFI](#uefi)와 [BIOS](#bios)에 대하여 순차적으로 보여주는 도표이다.

![윈도우 NT 운영체제의 부팅 절차 트러블슈팅 (예시. 윈도우 10)](https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/media/windows-boot-issues-troubleshooting/boot-sequence-thumb-expanded.png)

전원이 켜진 컴퓨터의 CPU는 [리셋 벡터](https://en.wikipedia.org/wiki/Reset_vector)(reset vector)에 위치한 [명령어](Processor.md#명령어)를 가장 먼저 실행하도록 하드웨어적으로 설계되었다. [x86](https://en.wikipedia.org/wiki/X86) 아키텍처 경우, 해당 위치는 [리얼 모드](Processor.md#리얼-모드)에서 실제 메모리 주소 `FFFFFFF0h`(즉, 4 GB의 16 바이트 아래)로 고정되었다. 리셋 벡터는 [ROM](https://en.wikipedia.org/wiki/Read-only_memory)에 저장된 UEFI 혹은 BIOS 펌웨어의 [진입점](C.md#진입점)을 가리킨다.

> UEFI 혹은 BIOS 펌웨어는 [RAM](Memory.md)에 로드되는 게 아니라, 아예 ROM을 시스템의 물리 메모리 일부로 인식하여 곧장 실행된다. 이러한 설계가 고안된 배경은 다음과 같으며, [하위호환](https://en.wikipedia.org/wiki/Backward_compatibility)을 위해 현재까지 이어지고 있다.<sup>[<a href="https://superuser.com/questions/988473/why-is-the-first-bios-instruction-located-at-0xfffffff0-top-of-ram
">참고</a>]</sup>
> 
> 1. 1980년대에 [32비트](https://en.wikipedia.org/wiki/32-bit_computing)를 지원하는 [i386](https://en.wikipedia.org/wiki/I386) 등의 프로세서가 등장하였으며, 4 GB 표현 범위에 불가하였으나 당시에는 엄청난 기술이었다.
> 1. RAM의 하위 1024바이트는 이미 [인터럽트 벡터](https://en.wikipedia.org/wiki/Interrupt_vector_table)로 지정되어, 오히려 메모리의 최상위 주소에 ROM을 대입하는 방안이 검토되었다.

ROM에 저장된 UEFI 혹은 BIOS 펌웨어가 실행되면 가장 먼저 [POST](#시동-자체-시험)를 진행하고, 검사를 통과하면 [부트로더](#부트로더)가 [운영체제](https://en.wikipedia.org/wiki/Operating_system)를 로드 및 실행한다.

### 시동 자체 시험
**[시동 자체 시험](https://en.wikipedia.org/wiki/Power-on_self-test)**(Power-on self-test; POST)은 컴퓨터나 타 디지털 전자 장치가 전원을 공급받는 즉시 실행된 ([UEFI](#uefi) 혹은 [BIOS](#bios)) 펌웨어에서 하드웨어 초기화 및 상태를 진단하는 절차이다. 흔히 [메인보드](https://en.wikipedia.org/wiki/Motherboard) 제조사 또는 OEM 로고가 표시되는 화면에 해당한다. POST 진단 결과는 디스플레이 화면에 출력되거나 별도의 진단 도구로부터 확인할 수 있도록 저장된다. 만일 화면 출력 기능에 문제가 있을 경우를 대비하여 LED 또는 경고음을 통해 오류 코드를 알릴 수 있는 장치가 마련되어 있다.

## 부트로더
**[부트로더](https://en.wikipedia.org/wiki/Bootloader)**(bootloader), 일명 **[부트스트랩](#부트스트랩) 로더**(bootstrap loader)는 컴퓨터 부팅 과정 중에서 설치된 운영체제의 [커널](Kernel.md)을 불러와 실행하는 프로그램이다. 만일 여럿 부팅 선택지 메뉴를 제공한다면 흔히 **부팅 관리자**(boot manager)라고 부르며, 선택된 별개의 OS 부트로더를 실행하는 [연쇄 로딩](https://en.wikipedia.org/wiki/Chain_loading)을 구현한다.

* [윈도우 부트 관리자](#윈도우-부팅-관리자): 일명 [`BOOTMGR`](#bios)(혹은 [`bootmgfw.efi`](#uefi))는 비스타 이상의 [윈도우 NT](Windows.md)를 위한 부팅 관리자이다.
    * *[Winload.exe](#윈도우-운영체제-로더): 커널 및 드라이버를 로드하는 윈도우 OS 부트로더이다; UEFI에서는 Winload.efi가 해당한다.*
* [GNU GRUB](https://en.wikipedia.org/wiki/GNU_GRUB): GNU 프로젝트의 일환으로 UNIX 기반의 운영체제를 위한 부트로더이다.

### 부트스트랩
**[부트스트랩](https://en.wikipedia.org/wiki/Bootstrapping)**(bootstrap)은 아무런 외부 도움을 받지 않고 자체적으로 실행하는 절차를 가리킨다. 부트스트랩의 어원은 다음과 같다:

> 미국 관용구 "[pull yourself up by your bootstraps](https://en.wiktionary.org/wiki/pull_oneself_up_by_one%27s_bootstraps)," 즉 *스스로를 부트스트랩을 당겨 들어올린다*라는 표현이 있는데 사실상 물리적으로 불가능하다. 하지만 현재는 "아무런 도움없이 어려움을 극복하고 성취하다"라는 의미로 변질되면서, 부트스트랩이란 용어가 이러한 의미를 함축하게 되었다.

컴퓨터에 전력이 공급되는 시점부터 운영체제가 로드될 때까지 자력으로 해내는 부트스트랩 과정을 일명 [부팅](#부팅)(booting)이라 부르게 된 것이다.

# BIOS
**[BIOS](https://en.wikipedia.org/wiki/BIOS)**(Basic Input/Output System)는 [부팅](#부팅) 과정에 하드웨어를 초기화 및 진단하고, [디스크](Storage.md)에 저장된 [부트로더](#부트로더)를 [메모리](Memory.md)에 로드하여 [운영체제](https://en.wikipedia.org/wiki/Operating_system)의 [커널](Kernel.md)을 실행시키는 [메인보드](https://en.wikipedia.org/wiki/Motherboard) [펌웨어](https://en.wikipedia.org/wiki/Firmware)이다. [IBM](https://en.wikipedia.org/wiki/IBM)에서 개발한 전매 소프트웨어였으나, [역설계](https://en.wikipedia.org/wiki/Reverse_engineering)를 성공한 이후 호환 PC 기종이 대거 생산되며 [사실상 표준](https://en.wikipedia.org/wiki/De_facto_standard)이 되었다. 새로운 [UEFI](#uefi)의 등장으로, 이를 구분하기 위해 "레거시" BIOS라고 흔히 언급된다.

BIOS가 부트 장치를 탐색하는 과정은 다음과 같다.

![BIOS 부팅 순서도 (개략)](https://upload.wikimedia.org/wikipedia/commons/2/20/Legacy_BIOS_boot_process_fixed.png)

[리셋 벡터](#부팅)가 가리킨 [ROM](https://en.wikipedia.org/wiki/Read-only_memory)에 저장된 BIOS 펌웨어가 실행되면 먼저 [POST](#시동-자체-시험)를 진행한다. 하드웨어 초기화 및 진단을 통과하면 [INT](Processor.md#인터럽트) [19h](https://en.wikipedia.org/wiki/BIOS_interrupt_call)를 호출하여 부트로더를 탐색, 로드, 그리고 실행하도록 한다. 부트로더가 위치한 [저장 매체](Storage.md)를 "부트 장치(boot device)"라고 부르며, BIOS가 부트로더를 탐색하는 과정은 다음과 같다:

1. BIOS 펌웨어는 [비휘발성 메모리](https://en.wikipedia.org/wiki/Nonvolatile_BIOS_memory)(대표적으로 [CMOS](https://en.wikipedia.org/wiki/CMOS))에 저장된 BIOS 설정으로부터 부트 장치 목록을 지정한 순서대로 살펴본다.
1. 부트 장치의 [부트 섹터](#부트-섹터)(즉, [MBR](#마스터-부트-레코드))를 메모리로 불러오고, 만일 해당 섹터를 읽을 수 없다면 다음 부트 장치로 넘어간다.
1. 부트 섹터를 읽을 수 있는 경우, 일부 BIOS는 마지막 두 시그니처 바이트 `0x55`, `0xAA`까지 검증하고 부팅 장치로 인식한다.

이후 BIOS는 메모리로 불러온 부트 섹터에게 PC 제어권을 양도한다. 다만, BIOS는 시그니처 바이트를 확인하는 것 외에 부트 섹터의 내용물을 판독하지 않는다.

## 부트 섹터
> *참고: [BIOS/MBR-based hard drive partitions | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/configure-biosmbr-based-hard-drive-partitions)*

**[부트 섹터](https://en.wikipedia.org/wiki/Boot_sector)**(boot sector)는 시스템을 부팅하기 위해 필요한 코드, 즉 [부트로더](#부트로더)가 담겨있는 [저장소](Storage.md)의 [섹터](Storage.md#섹터)이다. 일반적으로 [파티션](Storage.md#파티션)에 포함되지 않는 디스크의 가장 첫 섹터를 가리키며, 부트 섹터를 포함한 [디스크](Storage.md)를 "부트 장치(boot device)"라고 부른다.

다음은 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)에 사용되는 부트 섹터의 유형을 소개한다:

* [마스터 부트 레코드](#마스터-부트-레코드)
* [볼륨 부트 레코드](#볼륨-부트-레코드)

### 마스터 부트 레코드
**[마스터 부트 레코드](https://en.wikipedia.org/wiki/Master_boot_record)**(master boot record; MBR)는 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)을 위한 부트 섹터의 한 유형이며, [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive) 또는 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive) 등의 파티션을 나눌 수 있는 [대용량 저장 매체](Storage.md)([휴대용](https://en.wikipedia.org/wiki/Disk_enclosure) 포함)가 대상이다. 부트로더 뿐만 아니라, 해당 디스크의 [파티션 정보](https://en.wikipedia.org/wiki/Master_boot_record#PT)도 MBR에 저장되어 있다 (최대 네 개의 주 파티션까지 지원). 하지만 MBR 파티션 크기는 512 바이트로 제한되어, 부팅 과정에 [VBR](#볼륨-부트-레코드)이 함께 동원되기도 한다.

[윈도우 NT](Windows.md)의 경우, MBR의 부트로더는 [부트 플래그](https://en.wikipedia.org/wiki/Boot_flag)가 설정된 부팅 대상의 시스템 파티션, 즉 [활성 파티션](https://learn.microsoft.com/en-us/troubleshoot/windows-server/performance/computer-not-start-active-partition)(active partition)을 탐색하는 용도로 사용된다.

### 볼륨 부트 레코드
**[볼륨 부트 레코드](https://en.wikipedia.org/wiki/Volume_boot_record)**(volume boot record; VBR)는 [IBM PC 호환기종](https://en.wikipedia.org/wiki/IBM_PC_compatible)을 위한 부트 섹터의 한 유형이며, 간혹 **파티션 부트 레코드**(partition boot record; PBR)라고 언급되기도 한다. VBR은 다음 두 경우의 볼륨에서 찾아볼 수 있다.

1. 파티션을 나눌 수 없는 [플로피 디스크](https://en.wikipedia.org/wiki/Floppy_disk), [CD](https://en.wikipedia.org/wiki/Compact_disc) 및 [DVD](https://en.wikipedia.org/wiki/DVD) 등의 데이터 저장 매체의 부트 섹터가 해당한다.
1. 파티션을 나눌 수 있는 대용량 저장 매체에서 각 파티션의 첫 번째 섹터가 해당한다. 디스크 전반의 첫 번째 섹터는 여전히 MBR로써 파티션 정보를 저장하고 있다.

[윈도우 NT](Windows.md)의 [MBR](#마스터-부트-레코드)이 활성된 시스템 파티션을 발견하였을 시, 해당 파티션의 VBR 부트로드는 시스템 파티션에 저장된 [`BOOTMGR`](#윈도우-부트-관리자) 파일을 탐색 및 실행한다.

# UEFI
> *참고: [Boot and UEFI - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/bringup/boot-and-uefi)*

**[UEFI](https://en.wikipedia.org/wiki/UEFI)**(Unified Extensible Firmware Interface)는 [BIOS](#bios)의 기술적 한계를 극복하기 위해 설계된 [펌웨어](https://en.wikipedia.org/wiki/Firmware) 구조를 정의하는 규격이다. [인텔](https://www.intel.com/)에서 최초로 *EFI* 규격을 개발하였으나, 차후 [UEFI 포럼](https://en.wikipedia.org/wiki/UEFI_Forum)이란 산업 [컨소시엄](https://en.wikipedia.org/wiki/Consortium)에 합류하며 2006년에 *UEFI* [개방형 표준](https://en.wikipedia.org/wiki/Open_standard)을 발표하였다. 대표적으로 [TianoCore EDK II](https://en.wikipedia.org/wiki/TianoCore_EDK_II), [Pheonix SecureCore](https://www.phoenix.com/phoenix-securecore/), [InsydeH2O](https://www.insyde.com/products) 등이 구현되었으며, 이들을 통상 "UEFI 펌웨어"라고 부른다.

UEFI가 부트 장치를 탐색하는 과정은 다음과 같다.

![UEFI 부팅 순서도 (개략)](https://upload.wikimedia.org/wikipedia/commons/1/17/UEFI_boot_process.png)

[리셋 벡터](#부팅)가 가리킨 [ROM](https://en.wikipedia.org/wiki/Read-only_memory)에 저장된 UEFI 펌웨어가 실행되면 먼저 [POST](#시동-자체-시험)를 진행한다. 하드웨어 초기화 및 진단을 통과하면 [GPT](#guid-파티션-테이블)로부터 [EFI 시스템 파티션](#efi-시스템-파티션)을 찾아 [부트 관리자](#부트로더)를 실행한다. 즉, BIOS와 달리 [부트 섹터](#부트-섹터)에 전혀 의존하지 않는다. 부트 관리자의 기본 경로는 `/BOOT/BOOT<MACHINE_TYPE_SHORT_NAME>.EFI`이며 [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)에 따라 괄호에는 `IA32`, `X64`, `IA64`, `ARM` 또는 `AA64`이 대입된다.<sup>[<a href="https://unix.stackexchange.com/questions/565615/efi-boot-bootx64-efi-vs-efi-ubuntu-grubx64-efi-vs-boot-grub-x86-64-efi-gru">출처</a>]</sup> [윈도우 NT](Windows.md)를 포함한 일부 운영체제는 설치 시, 메인보드에 탑재되어 UEFI 펌웨어 설정을 저장하는 [NVRAM](https://en.wikipedia.org/wiki/Non-volatile_random-access_memory)에 부트 경로를 자체적으로 제작한 부트로더로 변경하기도 한다. 

## GUID 파티션 테이블
> *참고: [UEFI/GPT-based hard drive partitions | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/configure-uefigpt-based-hard-drive-partitions)*

**[GUID 파티션 테이블](https://en.wikipedia.org/wiki/GUID_Partition_Table)**(GUID Partition Table; GPT)은 [파티션](Storage.md#파티션) 유형을 [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)로 식별하는 UEFI에서 규정한 [파티션 테이블](https://en.wikipedia.org/wiki/Disk_partitioning#Partition_table)의 레이아웃이다.

![GUID 파티션 테이블 구도](https://upload.wikimedia.org/wikipedia/commons/0/07/GUID_Partition_Table_Scheme.svg)

데이터 블록을 [CHS](Storage.md#실린더-헤드-섹터)가 아닌 [LBA](Storage.md#논리-블록-주소-지정) 방식을 택한 GPT는 (LBA 0을 제외한) LBA 1을 [파티션 테이블 헤더](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_table_header_(LBA_1))로 가지며, LBA 2부터 최소 0x4000 바이트를 [파티션 진입 배열](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_entries_(LBA_2%E2%80%9333))로 사용한다. 만일 한 개의 LBA가 512 바이트의 섹터와 대응한다면 실질적으로 사용 가능한 블록은 LBA 34 이상이 해당된다. GPT는 LBA 0를 사용하지 않기 때문에 [MBR](#마스터-부트-레코드)이 함께 공존할 수 있다; UEFI 펌웨어는 [CSM](#호환성-지원-모듈)으로부터 레거시 BIOS 부팅을 충분히 지원할 수 있다.

### EFI 시스템 파티션
**[EFI 시스템 파티션](https://en.wikipedia.org/wiki/EFI_system_partition)**(EFI system partition; ESP)은 부팅될 때 UEFI 펌웨어가 불러올 파일들이 위치한 [데이터 저장 매체](Storage.md)의 파티션이다. GUID `C12A7328-F81F-11D2-BA4B-00A0C93EC93B`로 식별되며 [FAT](https://en.wikipedia.org/wiki/File_Allocation_Table) [파일 시스템](https://en.wikipedia.org/wiki/File_system)에 기반할 것을 UEFI는 규정한다. ESP 안에는 다음과 같은 데이터 및 파일이 저장되어 있다.

* [UEFI 어플리케이션](https://en.wikipedia.org/wiki/UEFI#Applications)
    * [부트로더](#부트로더): *윈도우 NT의 경우 [bootmgfw.efi](#윈도우-부트-관리자), winload.efi, winresume.efi 등이 해당*
    * [UEFI 셸](https://en.wikipedia.org/wiki/UEFI#UEFI_shell): *[x86-64](https://en.wikipedia.org/wiki/X86-64) 아키텍처는 SHELLX64.efi*
* [장치 드라이버](Driver.md): *부팅 시 UEFI 펌웨어가 사용할 [컴퓨터 하드웨어](https://en.wikipedia.org/wiki/Computer_hardware) 대상*
* 데이터 파일
    * [Boot Configuration Data](#부팅-구성-데이터)(일명 BCD)
    * 오류 로그

### 호환성 지원 모듈
**[호환성 지원 모듈](https://en.wikipedia.org/wiki/UEFI#CSM_booting)**(Compatibility Support Module; CSM)은 UEFI 펌웨어가 MBR 파티션의 디스크로부터 레거시 BIOS 모드로 부팅하는 걸 지원하는 하위호환이다. GPT가 LBA 0를 활용하지 않는 점을 이용하여 레거시 BIOS 기반의 시스템 부팅이 가능하였으며, 이를 *BIOS-GPT*라고 불렀다. 하지만 2020년부터 인텔은 더 이상 CSM을 지원하지 않는다고 발표하였다.

# 윈도우 부팅 관리자
**[윈도우 부팅 관리자](https://en.wikipedia.org/wiki/Windows_Boot_Manager)**(Windows Boot Manager), 또는 간략히 **부트 관리자**는 [마이크로소프트](https://aka.ms/microsoft)가 제공하는 [윈도우 NT](Windows.md)의 [부트로더](#부트로더) 중 하나이다.

![윈도우 부팅 관리자](https://upload.wikimedia.org/wikipedia/commons/a/a3/Windows_11_RE_Boot_menu.png)

본 부트로더의 핵심은 [BCD](#부팅-구성-데이터)에 저장된 부팅 설정 정보에 따라 다음으로 [연쇄 로딩](https://en.wikipedia.org/wiki/Chain_loading)할 [OS 부트로더](#윈도우-운영체제-로더)를 호출한다. 즉, 아직 [Windows OS](Windows.md)를 로드하는 과정이 아니지만 어느 [운영체제](https://en.wikipedia.org/wiki/Operating_system)를 부팅할 건지 결정하는 징검다리 역할을 한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">구격별 부팅 관리자 및 OS 부트로더</caption><colgroup><col style="width: 10%;"/><col style="width: 30%;"/><col style="width: 30%;"/><col style="width: 30%;"/></colgroup><thead><tr><th style="text-align: center;">규격</th><th style="text-align: center;">부트 관리자</th><th style="text-align: center;"><a href="#윈도우-운영체제-로더">OS 부트로더</a></th><th style="text-align: center;">OS 부트로더 (<a href="#">최대 절전</a>)</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="#bios"><b>BIOS</b></a></td><td style="text-align: center;">BOOTMGR</td><td style="text-align: center;">winload.exe</td><td style="text-align: center;">winresume.exe</td></tr><tr><td style="text-align: center;"><a href="#uefi"><b>UEFI</b></a></td><td style="text-align: center;">bootmgfw.efi</td><td style="text-align: center;">winload.efi</td><td style="text-align: center;">winresume.efi</td></tr></tbody></table>

<sup>_† Winresume.exe는 [하이버네이션](#하이버네이션)에 진입한 시스템을 다시 깨우는데, 이는 마치 컴퓨터를 다시 켜는 행위와 마찬가지이기 때문에 동원되는 특수한 OS 부트로더이다._</sup>

## 부팅 구성 데이터
> *참고: [Boot Options Identifiers - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-options-identifiers)*

**[부팅 구성 데이터](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-options-in-windows#boot-configuration-data)**(boot configuration data; BCD)

## 윈도우 운영체제 로더
**[윈도우 운영체제 로더](https://en.wikipedia.org/wiki/Windows_Boot_Manager#winload.exe)**(Windows operating system loader)는 [부팅 관리자](#윈도우-부팅-관리자)에 의해 연쇄적으로 실행되는 [OS 부트로더](#부트로더)이며, 윈도우 OS [커널](Kernel.md) 및 부팅 시 실행되어야 할 [드라이버](Driver.md)를 불러온다. 단, 필요한 모든 리소스를 불러올 때까지 커널 및 드라이버는 아직 초기화된 상태가 아님을 주의한다.

### 하이버네이션
**[하이버네이션](https://en.wikipedia.org/wiki/Hibernation_(computing))**(hibernation), 일명 [**최대 절전 모드**](https://support.microsoft.com/windows/2941d165-7d0a-a5e8-c5ad-8c972e8e6eff)
