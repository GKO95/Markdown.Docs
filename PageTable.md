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

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">레벨에 따른 페이지 테이블의 인덱스 및 엔트리 (4 KB w/ PAE)</caption><colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 25%;"/><col style="width: 15%;"/><col style="width: 30%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">레벨</th><th rowspan="2" style="text-align: center;">테이블</th><th colspan="2" style="text-align: center; border-bottom-style: none;">인덱스</th><th rowspan="2" style="text-align: center;">엔트리 / 데이터</th></tr><tr><th style="text-align: center;">이진수</th><th style="text-align: center;">십육진수</th></tr></thead><tbody><tr><td style="text-align: center;">4</td><td>Page Dir.Pointer Table</td><td style="text-align: center;"><code>10</code></td><td style="text-align: center;"><code>0x002</code></td><td><code>0x001ab001</code></td></tr><tr><td style="text-align: center;">3</td><td>Page Directory</td><td style="text-align: center;"><code>000001 101</code></td><td style="text-align: center;"><code>0x00D</code></td><td><code>0x01b09063</code></td></tr><tr><td style="text-align: center;">2</td><td>Page Table</td><td style="text-align: center;"><code>11110 1110</code></td><td style="text-align: center;"><code>0x1EE</code></td><td><code>0x02fde121</code></td></tr><tr><td style="text-align: center;">1</td><td>Page</td><td style="text-align: center;"><code>1111 01001100</code></td><td style="text-align: center;"><code>0xF4C</code></td><td><code>0x64f04d8b</code> (DWORD)</td></tr></tbody></table>

```windbg
0: kd> !pte nt!KeBugCheckEx
                    VA 81beef4c
PDE at C0602068            PTE at C040DF70
contains 0000000001B09063  contains 0000000002DEC121
pfn 1b09      ---DA--KWEV  pfn 2dec      -G--A--KREV

0: kd> !kext.dd 00000000`001a8000+8*0x002 L1
#  1a8010 001ab001

0: kd> !kext.dd 00000000`001ab000+8*0x00D L1
#  1ab068 01b09063

0: kd> !kext.dd 00000000`01b09000+8*0x1EE L1
# 1b0af00 02fde121

0: kd> !kext.dd 00000000`02dec000+1*0xF4C L1
# 2decf4c 6aec8b55

0: kd> dd nt!KeBugCheckEx L1
81beef4c  6aec8b55
```

### 32비트 페이징, 2 MB 페이지, PAE 활성
![With PAE; 2 MB pages](https://upload.wikimedia.org/wikipedia/commons/0/05/X86_Paging_PAE_2M.svg)

## x86-64 페이지 테이블

### 64비트 페이징, 4 KB 페이지, PAE 비활성


# ARM64 페이지 테이블
