# 페이지 테이블
**[페이지 테이블](https://en.wikipedia.org/wiki/Page_table)**

## x86 페이지 테이블
> *참고: [Physical Address Extension - Wikipedia § Page table structures](https://en.wikipedia.org/wiki/Physical_Address_Extension#Page_table_structures)*

CR0.PE (0번째 비트)

CR4.PAE (5번째 비트)

64비트 프로세서의 워드가 8바이트인 관계로 32비트 페이징에 불과하고 인덱스의 엔트리는 8바이트 간격으로 떨어져있다.

### 32비트 페이징, 4 KB 페이지, PAE 비활성
![No PAE, 4 KB pages](https://upload.wikimedia.org/wikipedia/commons/8/8e/X86_Paging_4K.svg)

### 32비트 페이징, 4 MB 페이지, PAE 비활성
![No PAE, 4 MB pages](https://upload.wikimedia.org/wikipedia/commons/d/d9/X86_Paging_4M.svg)

### 32비트 페이징, 4 KB 페이지, PAE 활성
![With PAE; 4 KB pages](https://upload.wikimedia.org/wikipedia/commons/0/0d/X86_Paging_PAE_4K.svg)

이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 그 전에 먼저 PAE가 활성화가 된 시스템이란 걸 아래와 같이 검증한다.

```
0: kd> .formats cr0
Evaluate expression:
  Hex:     80010033
  Decimal: -2147418061
  Decimal (unsigned) : 2147549235
  Octal:   20000200063
  Binary:  10000000 00000001 00000000 00110011
  Chars:   ...3
  Time:    ***** Invalid
  Float:   low -9.1907e-041 high -1.#QNAN
  Double:  -1.#QNAN
0: kd> .formats cr4
Evaluate expression:
  Hex:     001406e9
  Decimal: 1312489
  Decimal (unsigned) : 1312489
  Octal:   00005003351
  Binary:  00000000 00010100 00000110 11101001
  Chars:   ....
  Time:    Fri Jan 16 13:34:49 1970
  Float:   low 1.83919e-039 high 0
  Double:  6.48456e-318
```

다음은 System 프로세스와 nt!KeBugCheckEx 코드가 위치한 가상 메모리 주소와 이를 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name:  Not Found
Windows 10 Kernel Version 19041 MP (2 procs) Free x86 compatible
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.x86fre.vb_release.191206-1406
Kernel base = 0x81a70000 PsLoadedModuleList = 0x81d34d98

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 004f4454 00000000 00000000 00000000

Crashing Stack
============================================
 # ChildEBP RetAddr      
00 83452e08 81a34bde     nt!KeBugCheckEx
01 83452e38 81cfe89a     hal!HalBugCheckSystem+0x6d
02 83452ec4 81a3680d     nt!WheaReportHwError+0x418
03 83452ed8 81c0a6da     hal!HalHandleNMI+0xea
    <Intermediate frames may have been skipped due to lack of complete unwind>
04 83452ed8 81a1c76b (T) nt!KiTrap02+0x322
    <Intermediate frames may have been skipped due to lack of complete unwind>
05 83452fe0 8344699c (T) hal!HalProcessorIdle+0x7
WARNING: Frame IP not in any known module. Following frames may be wrong.
06 83452fe4 00000000     0x8344699c

0: kd> r cr3
cr3=001a8000
0: kd> !process 0 0 System
PROCESS 82354040  SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 001a8000  ObjectTable: 86002d80  HandleCount: 2327.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     81beef4c
  Decimal: -2118193332
  Decimal (unsigned) : 2176773964
  Octal:   20157567514
  Binary:  10000001 10111110 11101111 01001100
  Chars:   ...L
  Time:    ***** Invalid
  Float:   low -7.01384e-038 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 Page Directory를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x001a8000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (4 KB w/ PAE)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">3</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>10</code></td><td style="text-align: center;"><code>0x002</code></td><td><code>001ab001</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Directory</td><td style="text-align: center;"><code>000001 101</code></td><td style="text-align: center;"><code>0x00D</code></td><td><code>01b09063</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Table</td><td style="text-align: center;"><code>11110 1110</code></td><td style="text-align: center;"><code>0x1EE</code></td><td><code>02fde121</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>1111 01001100</code></td><td style="text-align: center;"><code>0xF4C</code></td><td><code>55</code> (BYTE)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                    VA 81beef4c
PDE at C0602068            PTE at C040DF70
contains 0000000001B09063  contains 0000000002DEC121
pfn 1b09      ---DA--KWEV  pfn 2dec      -G--A--KREV

0: kd> u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
81beef4c 55              push    ebp

0: kd> !kext.dd 00000000`001a8000+8*0x002 L1
#  1a8010 001ab001

0: kd> !kext.dd 00000000`001ab000+8*0x00D L1
#  1ab068 01b09063

0: kd> !kext.dd 00000000`01b09000+8*0x1EE L1
# 1b0af00 02fde121

0: kd> !kext.db 00000000`02dec000+1*0xF4C L1
# 2decf4c 55 U...............
```

### 32비트 페이징, 2 MB 페이지, PAE 활성
![With PAE; 2 MB pages](https://upload.wikimedia.org/wikipedia/commons/0/05/X86_Paging_PAE_2M.svg)

이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 그 전에 먼저 PAE가 활성화가 된 시스템이란 걸 아래와 같이 검증한다.

```
0: kd> .formats cr0
Evaluate expression:
  Hex:     80010033
  Decimal: -2147418061
  Decimal (unsigned) : 2147549235
  Octal:   20000200063
  Binary:  10000000 00000001 00000000 00110011
  Chars:   ...3
  Time:    ***** Invalid
  Float:   low -9.1907e-041 high -1.#QNAN
  Double:  -1.#QNAN
0: kd> .formats cr4
Evaluate expression:
  Hex:     001406e9
  Decimal: 1312489
  Decimal (unsigned) : 1312489
  Octal:   00005003351
  Binary:  00000000 00010100 00000110 11101001
  Chars:   ....
  Time:    Fri Jan 16 13:34:49 1970
  Float:   low 1.83919e-039 high 0
  Double:  6.48456e-318
```

다음은 System 프로세스와 nt!KeBugCheckEx 코드가 위치한 가상 메모리 주소와 이를 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name:  Not Found
Windows 10 Kernel Version 19041 MP (2 procs) Free x86 compatible
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.x86fre.vb_release.191206-1406
Kernel base = 0x82800000 PsLoadedModuleList = 0x82ac4d98

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 004f4454 00000000 00000000 00000000

Crashing Stack
============================================
 # ChildEBP RetAddr      
00 87c52e08 83032bde     nt!KeBugCheckEx
01 87c52e38 82a8e89a     hal!HalBugCheckSystem+0x6d
02 87c52ec4 8303480d     nt!WheaReportHwError+0x418
03 87c52ed8 8299a6da     hal!HalHandleNMI+0xea
    <Intermediate frames may have been skipped due to lack of complete unwind>
04 87c52ed8 82a59b77 (T) nt!KiTrap02+0x322
    <Intermediate frames may have been skipped due to lack of complete unwind>
05 87c52fe8 00000000 (T) nt!PpmIdleGuestExecute+0x1a

0: kd> r cr3
cr3=001a8000
0: kd> !process 0 0 System
PROCESS 8636c040  SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 001a8000  ObjectTable: 88202d80  HandleCount: 2307.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     8297ef4c
  Decimal: -2103972020
  Decimal (unsigned) : 2190995276
  Octal:   20245767514
  Binary:  10000010 10010111 11101111 01001100
  Chars:   ...L
  Time:    ***** Invalid
  Float:   low -2.23248e-037 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 Page Directory를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x001a8000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (2 MB w/ PAE)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">2</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>10</code></td><td style="text-align: center;"><code>0x002</code></td><td><code>001ab001</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Directory</td><td style="text-align: center;"><code>000010 100</code></td><td style="text-align: center;"><code>0x014</code></td><td><code>01b09063</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>10111 11101111 01001100</code></td><td style="text-align: center;"><code>0x17EF4C</code></td><td><code>55</code> (BYTE)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                    VA 8297ef4c
PDE at C06020A0            PTE at C0414BF0
contains 0000000002C009E3  contains 0000000000000000
pfn 2c00      -GLDA--KWEV    LARGE PAGE pfn 2d7e

0: kd> u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
8297ef4c 55              push    ebp

0: kd> !kext.dd 00000000`001a8000+8*0x002 L1
#  1a8010 001ab001

0: kd> !kext.dd 00000000`001ab000+8*0x014 L1
#  1ab068 01b09063

0: kd> !kext.db 00000000`02c00000+1*0x17EF4C L1
# 2d7ef4c 55 U...............
```

## x86-64 페이지 테이블

### 64비트 페이징, 4 KB 페이지

이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 KERNEL32!BaseThreadInitThunk 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 아래는 가상 메모리 주소로부터 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name: DESKTOP-TKOG2TV
Windows 10 Kernel Version 19041 MP (2 procs) Free x64
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.amd64fre.vb_release.191206-1406
Kernel base = 0xfffff800`02e00000 PsLoadedModuleList = 0xfffff800`03a2a770

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 00000000`004f4454 00000000`00000000 00000000`00000000 00000000`00000000

Crashing Stack
============================================
  *** Stack trace for last set context - .thread/.cxr resets it
 # Child-SP          RetAddr               Call Site
00 fffff800`09499b58 fffff800`032b965a     nt!KeBugCheckEx
01 fffff800`09499b60 fffff800`01f015b0     nt!HalBugCheckSystem+0x7a
02 fffff800`09499ba0 fffff800`033bb9ae     PSHED!PshedBugCheckSystem+0x10
03 fffff800`09499bd0 fffff800`032bdd12     nt!WheaReportHwError+0x46e
04 fffff800`09499cb0 fffff800`03312f64     nt!HalHandleNMI+0x142
05 fffff800`09499ce0 fffff800`0320a642     nt!KiProcessNMI+0x134
06 fffff800`09499d30 fffff800`0320a412     nt!KxNmiInterrupt+0x82
07 fffff800`09499e70 fffff800`0303920e     nt!KiNmiInterrupt+0x212
08 ffffd306`22f57330 fffff800`030370dd     nt!RtlpHpVsContextAllocateInternal+0xbe
09 ffffd306`22f57390 fffff800`037b7074     nt!ExAllocateHeapPool+0x6ed
0a ffffd306`22f574d0 fffff800`034b5395     nt!ExAllocatePoolWithTag+0x64
0b ffffd306`22f57520 fffff800`03447fd0     nt!PfpCopyUserPfnPrioRequest+0xe5
0c ffffd306`22f57580 fffff800`0341248c     nt!PfpPfnPrioRequest+0x60
0d ffffd306`22f57600 fffff800`0341090b     nt!PfQuerySuperfetchInformation+0x2ec
0e ffffd306`22f57740 fffff800`0340e8b7     nt!ExpQuerySystemInformation+0x1f0b
0f ffffd306`22f57a80 fffff800`03211138     nt!NtQuerySystemInformation+0x37
10 ffffd306`22f57ac0 00007ffe`4802d6a4     nt!KiSystemServiceCopyEnd+0x28
11 000000c6`b9779b88 00007ffe`406bed47     ntdll!NtQuerySystemInformation+0x14
12 000000c6`b9779b90 00007ffe`406a9766     sysmain!PfPdPfnsQuery+0x297
13 000000c6`b977ce70 00007ffe`406a954c     sysmain!PfRbMemoryQuery+0x162
14 000000c6`b977ceb0 00007ffe`406aa68f     sysmain!AgPdProcessorCallback+0x2c
15 000000c6`b977cee0 00007ffe`406aa3b1     sysmain!PfSvSuperfetchProcessAction+0x53
16 000000c6`b977cf60 00007ffe`406e6fe1     sysmain!PfSvSuperfetchProcessActions+0x59
17 000000c6`b977cfa0 00007ffe`406fb88e     sysmain!PfSvcMainThreadWorker+0xf91
18 000000c6`b977f600 00007ffe`406e81bf     sysmain!PfSvcMainThread+0x22
19 000000c6`b977f640 00007ff7`b4e84340     sysmain!SysMtServiceMain+0x10f
1a 000000c6`b977f680 00007ffe`4697dfd8     svchost!ServiceStarter+0x310
1b 000000c6`b977f7b0 00007ffe`47017344     sechost!ScSvcctrlThreadA+0x28
1c 000000c6`b977f7e0 00007ffe`47fe26b1     KERNEL32!BaseThreadInitThunk+0x14
1d 000000c6`b977f810 00000000`00000000     ntdll!RtlUserThreadStart+0x21

0: kd> r cr3
Last set context:
cr3=0000000018573000
0: kd> !process 0 0 System
PROCESS ffffe70601a7a080
    SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 007d4000  ObjectTable: ffff8f007c61fc80  HandleCount: 2454.
    Image: System

0: kd> .formats KERNEL32!BaseThreadInitThunk+0x14
Evaluate expression:
  Hex:     00007ffe`47017344
  Decimal: 140730089698116
  Decimal (unsigned) : 140730089698116
  Octal:   0000003777710700271504
  Binary:  00000000 00000000 01111111 11111110 01000111 00000001 01110011 01000100
  Chars:   ...G.sD
  Time:    Wed Jun 13 06:10:08.969 1601 (UTC + 9:00)
  Float:   low 33139.3 high 4.59149e-041
  Double:  6.95299e-310
```

System 프로세스의 메모리 주소 변환에 필요한 PML4를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x0000000018573000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (4 KB 페이지)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">4</td><td>PML4</td><td style="text-align: center;"><code>01111111 1</code></td><td style="text-align: center;"><code>0x0FF</code></td><td><code>0a000000`1857f867</code></td></tr><tr><td style="text-align: center;">3</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>1111110 01</code></td><td style="text-align: center;"><code>0x1F9</code></td><td><code>0a000000`18582867</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Directory</td><td style="text-align: center;"><code>000111 000</code></td><td style="text-align: center;"><code>0x038</code></td><td><code>0a000000`185c8867</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Table</td><td style="text-align: center;"><code>00001 0111</code></td><td style="text-align: center;"><code>0x017</code></td><td><code>01000000`0174a025</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>0011 01000100</code></td><td style="text-align: center;"><code>0x344</code></td><td><code>c88b</code> (WORD)</td></tr></tbody></table>

```windbg
0: kd> !pte KERNEL32!BaseThreadInitThunk+0x14
                                           VA 00007ffe47017344
PXE at FFFFCEE773B9D7F8    PPE at FFFFCEE773AFFFC8    PDE at FFFFCEE75FFF91C0    PTE at FFFFCEBFFF2380B8
contains 0A0000001857F867  contains 0A00000018582867  contains 0A000000185C8867  contains 010000000174A025
pfn 1857f     ---DA--UWEV  pfn 18582     ---DA--UWEV  pfn 185c8     ---DA--UWEV  pfn 174a      ----A--UREV

0: kd> u KERNEL32!BaseThreadInitThunk+0x14 L1
KERNEL32!BaseThreadInitThunk+0x14:
00007ffe`47017344 8bc8            mov     ecx,eax

0: kd> !kext.dq 00000000`18573000+8*0x0FF L1
#185737f8 0a000000`1857f867

0: kd> !kext.dq 00000000`1857f000+8*0x1F9 L1
#1857ffc8 0a000000`18582867

0: kd> !kext.dq 00000000`18582000+8*0x038 L1
#185821c0 0a000000`185c8867

0: kd> !kext.dq 00000000`185c8000+8*0x017 L1
#185c80b8 01000000`0174a025

0: kd> !kext.db 00000000`0174A000+1*0x344 L2
# 174a344 8b c8 ................
```

### 64비트 페이징, 2 MB 페이지
이번 예시는 System 프로세스로부터 커널 충돌을 야기한 스택에서 nt!KeBugCheckEx 코드가 실제 물리 메모리의 어느 주소에 위치하는지 확인한다. 아래는 가상 메모리 주소로부터 물리 메모리 주소로 변환하기 위해 필요한 정보들을 살펴본다.

```windbg
0: kd> !mex.crash
Dump Info
============================================
Computer Name: DESKTOP-TKOG2TV
Windows 10 Kernel Version 19041 MP (2 procs) Free x64
Product: WinNt, suite: TerminalServer SingleUserTS
Edition build lab: 19041.1.amd64fre.vb_release.191206-1406
Kernel base = 0xfffff800`02e00000 PsLoadedModuleList = 0xfffff800`03a2a770

Bugcheck details
============================================
Bugcheck code 00000080
Arguments 00000000`004f4454 00000000`00000000 00000000`00000000 00000000`00000000

Crashing Stack
============================================
  *** Stack trace for last set context - .thread/.cxr resets it
 # Child-SP          RetAddr               Call Site
00 fffff800`09499b58 fffff800`032b965a     nt!KeBugCheckEx
01 fffff800`09499b60 fffff800`01f015b0     nt!HalBugCheckSystem+0x7a
02 fffff800`09499ba0 fffff800`033bb9ae     PSHED!PshedBugCheckSystem+0x10
03 fffff800`09499bd0 fffff800`032bdd12     nt!WheaReportHwError+0x46e
04 fffff800`09499cb0 fffff800`03312f64     nt!HalHandleNMI+0x142
05 fffff800`09499ce0 fffff800`0320a642     nt!KiProcessNMI+0x134
06 fffff800`09499d30 fffff800`0320a412     nt!KxNmiInterrupt+0x82
07 fffff800`09499e70 fffff800`0303920e     nt!KiNmiInterrupt+0x212
08 ffffd306`22f57330 fffff800`030370dd     nt!RtlpHpVsContextAllocateInternal+0xbe
09 ffffd306`22f57390 fffff800`037b7074     nt!ExAllocateHeapPool+0x6ed
0a ffffd306`22f574d0 fffff800`034b5395     nt!ExAllocatePoolWithTag+0x64
0b ffffd306`22f57520 fffff800`03447fd0     nt!PfpCopyUserPfnPrioRequest+0xe5
0c ffffd306`22f57580 fffff800`0341248c     nt!PfpPfnPrioRequest+0x60
0d ffffd306`22f57600 fffff800`0341090b     nt!PfQuerySuperfetchInformation+0x2ec
0e ffffd306`22f57740 fffff800`0340e8b7     nt!ExpQuerySystemInformation+0x1f0b
0f ffffd306`22f57a80 fffff800`03211138     nt!NtQuerySystemInformation+0x37
10 ffffd306`22f57ac0 00007ffe`4802d6a4     nt!KiSystemServiceCopyEnd+0x28
11 000000c6`b9779b88 00007ffe`406bed47     ntdll!NtQuerySystemInformation+0x14
12 000000c6`b9779b90 00007ffe`406a9766     sysmain!PfPdPfnsQuery+0x297
13 000000c6`b977ce70 00007ffe`406a954c     sysmain!PfRbMemoryQuery+0x162
14 000000c6`b977ceb0 00007ffe`406aa68f     sysmain!AgPdProcessorCallback+0x2c
15 000000c6`b977cee0 00007ffe`406aa3b1     sysmain!PfSvSuperfetchProcessAction+0x53
16 000000c6`b977cf60 00007ffe`406e6fe1     sysmain!PfSvSuperfetchProcessActions+0x59
17 000000c6`b977cfa0 00007ffe`406fb88e     sysmain!PfSvcMainThreadWorker+0xf91
18 000000c6`b977f600 00007ffe`406e81bf     sysmain!PfSvcMainThread+0x22
19 000000c6`b977f640 00007ff7`b4e84340     sysmain!SysMtServiceMain+0x10f
1a 000000c6`b977f680 00007ffe`4697dfd8     svchost!ServiceStarter+0x310
1b 000000c6`b977f7b0 00007ffe`47017344     sechost!ScSvcctrlThreadA+0x28
1c 000000c6`b977f7e0 00007ffe`47fe26b1     KERNEL32!BaseThreadInitThunk+0x14
1d 000000c6`b977f810 00000000`00000000     ntdll!RtlUserThreadStart+0x21

0: kd> r cr3
Last set context:
cr3=0000000018573000
0: kd> !process 0 0 System
PROCESS ffffe70601a7a080
    SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 007d4000  ObjectTable: ffff8f007c61fc80  HandleCount: 2454.
    Image: System

0: kd> .formats nt!KeBugCheckEx
Evaluate expression:
  Hex:     fffff800`031fd5b0
  Decimal: -8796040604240
  Decimal (unsigned) : 18446735277668947376
  Octal:   1777777600000307752660
  Binary:  11111111 11111111 11111000 00000000 00000011 00011111 11010101 10110000
  Chars:   ........
  Time:    ***** Invalid FILETIME
  Float:   low 4.69712e-037 high -1.#QNAN
  Double:  -1.#QNAN
```

System 프로세스의 메모리 주소 변환에 필요한 PML4를 가리키는 포인터 테이블은 CR3 레지스터(혹은 DirBase)가 반환한 `0x0000000018573000` 물리 메모리 주소에 위치한다. 주소 변환에 있어 다음 페이지로 진입할 포인터를 가리키는 인덱스들은 다음과 같으며, 유도 과정도 함께 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (2 MB 페이지)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">3</td><td>PML4</td><td style="text-align: center;"><code>11111000 0</code></td><td style="text-align: center;"><code>0x1F0</code></td><td><code>00000000`04709063</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>0000000 00</code></td><td style="text-align: center;"><code>0x000</code></td><td><code>00000000`0460a063</code></td></tr><tr><td style="text-align: center;">1</td><td>Page Directory</td><td style="text-align: center;"><code>000011 000</code></td><td style="text-align: center;"><code>0x018</code></td><td><code>0a000000`02a001a1</code></td></tr><tr><td style="text-align: center;">0</td><td>Page</td><td style="text-align: center;"><code>11111 11010101 10110000</code></td><td style="text-align: center;"><code>0x1FD5B0</code></td><td><code>48894c2408</code> (5-Byte)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                                           VA fffff800031fd5b0
PXE at FFFFCEE773B9DF80    PPE at FFFFCEE773BF0000    PDE at FFFFCEE77E0000C0    PTE at FFFFCEFC00018FE8
contains 0000000004709063  contains 000000000460A063  contains 0A00000002A001A1  contains 0000000000000000
pfn 4709      ---DA--KWEV  pfn 460a      ---DA--KWEV  pfn 2a00      -GL-A--KREV  LARGE PAGE pfn 2bfd   

u nt!KeBugCheckEx L1
nt!KeBugCheckEx:
fffff800`031fd5b0 48894c2408      mov     qword ptr [rsp+8],rcx

0: kd> !kext.dq 00000000`18573000+8*0x1F0 L1
#18573f80 00000000`04709063

0: kd> !kext.dq 00000000`04709000+8*0x000 L1
# 4709000 00000000`0460a063

0: kd> !kext.dq 00000000`0460A000+8*0x18 L1
# 460a0c0 0a000000`02a001a1

0: kd> !kext.db 00000000`02a00000+1*0x1FD5B0 L5
# 2bfd5b0 48 89 4c 24 08 H.L$............
```

# ARM64 페이지 테이블
