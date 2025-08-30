# 동적 링크 라이브러리
**[동적 링크 라이브러리](https://en.wikipedia.org/wiki/Dynamic-link_library)**(dynamic-link library; DLL)는 [함수](C.md#함수), [클래스](Cpp.md#클래스), [변수](C.md#변수), 또는 아이콘과 같은 리소스를 담고 있는 [컴파일](Programming.md#컴파일러)된 .dll 확장자 [모듈](https://en.wikipedia.org/wiki/Modular_programming)이다. DLL을 참조한 프로그램은 [런타임](https://en.wikipedia.org/wiki/Execution_(computing)#Runtime) 도중에 모듈 이미지를 [프로세스](Process.md)의 [가상 주소 공간](Process.md#가상-주소-공간)에 [매핑](Memory.md#메모리-맵-파일)한다. 이러한 동작은 프로그램 이미지 크기를 상당히 줄일 수 있고 라이브러리 업데이트가 매우 편리한 장점을 지닌다.

다음은 Windows NT에서 대표적인 DLL 일부를 나열 및 소개한다.

* Kernel32.dll: [메모리](Memory.md), [프로세스](Process.md) 및 [스레드](Thread.md)를 관리하는 함수들을 제공한다.
* User32.dll: 창 생성 및 메시지 전송 등 사용자 상호작용 작업에 동원되는 함수들을 제공한다.
* GDI32.dll: 시각적 이미지를 그리거나 텍스트를 표시하는 함수들을 제공한다.

### 정적 링크 라이브러리
**[정적 링크 라이브러리](https://en.wikipedia.org/wiki/Static_library)**(static-link library; LIB)는 [DLL](#동적-링크-라이브러리)과 마찬가지로 함수, 클래스, 변수 등을 담고 있는 .lib 확장자 모듈읻다. 다만, 빌드 과정 중 [링크 타임](Programming.md#링커) 때 모듈이 프로그램 이미지에 아예 통합되는 차이점이 있다. 빌드된 프로그램은 이미지 크기가 커지겠지만, 라이브러리 파일의 부재나 의존성에 의한 오류를 방지하여 안정성을 높을 수 있다.

한편, DLL 모듈이 한 개 이상의 함수나 변수를 내보낸다면<sup>[[참고](https://learn.microsoft.com/cpp/build/exporting-from-a-dll)]</sup> 빌드 과정에서 [링커](Programming.md#링커)는 단일 .lib 파일을 함께 생성한다: 어떠한 코드도 정의되어 있지 않으며, 단순히 내보내진 함수 및 변수의 심볼 목록이 나열된 매우 작은 파일이다. 해당 DLL의 심볼을 참조하는 타 프로그램을 빌드할 시, 링커는 .lib 파일을 단순히 참조한 외부 심볼이 어느 DLL에 있는지 파악하는 단서로써 활용한다.

## DLL 프로그래밍
DLL의 일부 동작 원리를 이해하기 위해서는 이에 대한 프로그래밍 관련 내용을 함께 살펴본다. [Visual Studio](https://visualstudio.microsoft.com/)의 일환인 [DUMPBIN](https://learn.microsoft.com/cpp/build/reference/dumpbin-reference) 도구는 실행 프로그램 및 DLL 파일의 정보를 보여주는 데 유용하기 때문에 활용해 보는 걸 적극 권장한다.

아래는 Visual Studio 프로젝트에서 소스 코드를 DLL 모듈로 빌드하기 위해 필요한 설정을 보여준다.

![Visual Studio에서 DLL 빌드를 위한 프로젝트 설정](./images/visual_studio_library.png)

다음 키워드로 선언된 변수, 함수 프로토타임, 그리고 C++ 클래스는 링커에 의해 다음과 같이 처리된다.

* [`__declspec(dllexport)`](https://learn.microsoft.com/cpp/build/exporting-from-a-dll-using-declspec-dllexport) 키워드: 선언된 데이터의 심볼들은 [export 섹션](#export-섹션)에 나열된다.
* [`__declspec(dllimport)`](https://learn.microsoft.com/cpp/build/importing-into-an-application-using-declspec-dllimport) 키워드: 선언된 데이터의 심볼들은 [import 섹션](#import-섹션)에 나열된다.

## Export 섹션
**[.edata 섹션](https://learn.microsoft.com/windows/win32/debug/pe-format#the-edata-section-image-only)**(export data section)은 동적 링크로 다른 프로그램이 접근할 수 있도록 DLL이 노출한 [심볼](Symbol.md) 정보들이 담겨있다. [PE 포맷](https://learn.microsoft.com/windows/win32/debug/pe-format)의 [선택적 헤더](https://learn.microsoft.com/windows/win32/debug/pe-format#optional-header-image-only) 중 데이터 디렉토리의 첫 번째 여덟 바이트가 바로 export 섹션의 위치와 크기를 의미한다.

섹션은 [Export Directory Table](https://learn.microsoft.com/windows/win32/debug/pe-format#export-directory-table)로 시작하여, 안에는 다음 배열들의 RVA를 담고 있어 DLL이 내보내는 심볼 전반의 정보를 파악할 수 있다.

> 여기서 RVA(relative virtual address)란, 모듈이 가상 주소 공간에 매핑되었을 때 "BaseImage + RVA", 즉 DLL 이미지의 기반 주소로부터 상대적 거리를 제시하여 가상 주소를 찾아갈 수 있도록 한다.

* [Export Address Table](https://learn.microsoft.com/windows/win32/debug/pe-format#export-address-table) <sub>(필수)</sub> : 이진 검색을 위해, 심볼명의 알파벳 순서대로 RVA를 배열에 나열한다.

    <table style="table-layout: fixed; width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Export Address Table의 RVA<sup></sup> 유형</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">Export RVA</th><th style="text-align: center;">Forwarder RVA</th></tr></thead><tbody><tr style="text-align: center;"><td>.edata 섹션 범위 밖의 주소를 가리킨다.</td><td>.edata 섹션 범위 안의 주소를 가리킨다.</td></tr><tr><td>DLL에 정의된 변수, 함수, 클래스 등의 코드가 위치한 상대적 주소</td><td><a href="https://learn.microsoft.com/windows/win32/debug/pe-format#export-name-table">Export Name Table</a>에 기입된 다른 DLL로 포워딩 될 심볼 문자열을 가리키는 상대적 주소</td></tr></tbody></table>

* [Export Name Pointer Table](https://learn.microsoft.com/windows/win32/debug/pe-format#export-name-pointer-table) <sub>(선택)</sub> : Export Address Table 순서에 대응하여, Export Name Table의 심볼명 문자열로 가리키는 32비트 포인터를 저장한다.
* [Export Ordinal Table](https://learn.microsoft.com/windows/win32/debug/pe-format#export-ordinal-table) <sub>(선택)</sub> : Export Name Pointer Table의 포인터와 동일한 인덱스에 해당 심볼의 16비트 순번을 저장하며, "자연스러운" [데이터 정렬](Memory.md#데이터-정렬)을 위해 Export Name Pointer Table과 별개의 배열로 관리된다. Biased ordinal(= Ordinal - Ordinal base)는 Export Address Table의 인덱스와 같다.

Export 섹션은 마지막은 [Export Name Table](https://learn.microsoft.com/windows/win32/debug/pe-format#export-name-table)로 심볼의 문자열을 나열한다. Private 심볼을 내보낼 때는 다른 이름의 public 심볼로 명명하거나, 또는 다른 DLL의 심볼로 안내하는 포워딩에 활용된다.

DUMPBIN의 [/EXPORTS](https://learn.microsoft.com/cpp/build/reference/dash-exports) 옵션을 사용하면 위의 Export 섹션의 심볼들을 종합적으로 정리하여 아래와 같이 보여준다.

```
C:\Windows\System32>DUMPBIN /EXPORTS ntshrui.dll
Dump of file C:\Windows\System32\ntshrui.dll

File Type: DLL

  Section contains the following exports for ntshrui.dll

    00000000 characteristics
    6F814EA5 time date stamp
        0.00 version
           1 ordinal base
          15 number of functions
          15 number of names

    ordinal hint RVA      name

          1    0 0002DBB0 CanShareFolder
          2    1 0001E860 DllCanUnloadNow
          3    2 0001B7C0 DllGetClassObject
          4    3 0002DCC0 GetLocalPathFromNetResource
          5    4 0002DCC0 GetLocalPathFromNetResourceA
          6    5 000186B0 GetLocalPathFromNetResourceW
          7    6 0002DCC0 GetNetResourceFromLocalPath
          8    7 0002DCC0 GetNetResourceFromLocalPathA
          9    8 0000AF70 GetNetResourceFromLocalPathW
         10    9 0006CB50 IsFolderPrivateForUser
         11    A 0002DCC0 IsPathShared
         12    B 0002DCC0 IsPathSharedA
         13    C 00002C80 IsPathSharedW
         14    D 0006CDE0 SetFolderPermissionsForSharing
         15    E 0002DCF0 ShowShareFolderUI
```

## Import 섹션
**[.idata 섹션](https://learn.microsoft.com/windows/win32/debug/pe-format#the-idata-section)**(import data section)은 프로그램이 실행될 때 동적 링크로 불러와야 할 DLL 모듈과 각각 참조된 심볼들을 목록이 담겨있다. C 표준의 [`extern`](C.md#변수) 키워드로 심볼들을 불러올 수 있지만, [`__declspec(dllimport)`](https://learn.microsoft.com/cpp/build/importing-into-an-application-using-declspec-dllimport) 키워드로 선언하면 효율적인 코드로 소폭 향상시켜 컴파일한다. [PE 포맷](https://learn.microsoft.com/windows/win32/debug/pe-format)의 [선택적 헤더](https://learn.microsoft.com/windows/win32/debug/pe-format#optional-header-image-only) 중 데이터 디렉토리의 두 번째 여덟 바이트가 바로 import 섹션의 위치와 크기를 의미한다.

섹션은 [Import Directory Table](https://learn.microsoft.com/windows/win32/debug/pe-format#import-directory-table)로부터 시작되며, 각각 프로그램이 불러오는 DLL을 반영하기 때문에 다수의 테이블로 나열되는 경우가 대다수이다. 안에는 다음 배열들의 RVA를 담고 있어 불러오는 DLL의 심볼 정보를 파악할 수 있다.

> 여기서 RVA(relative virtual address)란, 모듈이 가상 주소 공간에 매핑되었을 때 "BaseImage + RVA", 즉 DLL 이미지의 기반 주소로부터 상대적 거리를 제시하여 가상 주소를 찾아갈 수 있도록 한다.

* [Import Lookup Table](https://learn.microsoft.com/windows/win32/debug/pe-format#import-lookup-table): 32비트(PE32) 혹은 64비트(PE32+) 정수의 배열이며, 각 요소마다 아래와 같이 비트 단위를 활용하여 해당 DLL로부터 불러오는 심볼의 순번 혹은 이름 정보를 담는다. 배열의 마지막 요소는 NULL로 장식하여 타 DLL의 Import Lookup Table과 구분한다.

    <table style="table-layout: fixed; width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Import Lookup Table 배열 요소의 비트 단위 정보</caption><colgroup><col style="width: 30%;"/><col style="width: 5%;"/><col style="width: 15%;"/><col style="width: 50%;"/></colgroup><thead><tr><th rowspan="2" style="text-align: center;">순번/이름 플래그<br/>(최상위 1비트)</th><th rowspan="2">...</th><th colspan="2" style="text-align: center; border-bottom-style: dashed;">힌트/이름 테이블 RVA (최하위 31비트)</th></tr><tr><th style="text-align: center;">-</th><th style="text-align: center;">순번 (최하위 16비트)</th></tr></thead><tbody><tr><td rowspan="2"><ul><li>Set: 순번으로 불러온다.</li><li>Clear: 심볼명으로 불러온다.</li></ul></td><td rowspan="2">...</td><td style="text-align: center;">-</td><td style="text-align: center;">해당 DLL 심볼을 Import Addresss Table 인덱스에 대응하는 순번<td></tr><tr><td colspan="2" style="text-align: center;">해당 DLL 심볼을 <a href="https://learn.microsoft.com/windows/win32/debug/pe-format#hintname-table">Hint/Name Table</a> 안의 문자열로 안내하는 RVA</td></tr></tbody></table>

    <sup>_† 순번 플래그가 설정되거나 PE32+의 경우 사용하지 않는 비트가 생기는데 이들은 모두 0으로 초기화된다._</sup>

* Name: 본 Import Directory Table에 해당하는 DLL의 ASCII 문자열을 가리키는 32비트 RVA 포인터를 저장한다.
* [Import Address Table](https://learn.microsoft.com/windows/win32/debug/pe-format#import-address-table): 구조와 내용물은 Import Lookup Table과 동일하지만, 해당 DLL 이미지가 [프로세스](Process.md)에 로드되어 바인딩이 될 때 (심볼의 상대적인 RVA는) [가상 주소 공간](Process.md#가상-주소-공간)에 대응하는 실제 "가상 메모리 주소"로 덮어씌어진다.

DUMPBIN의 [/IMPORTS](https://learn.microsoft.com/cpp/build/reference/dash-exports) 옵션을 사용하면 위의 Import 섹션의 심볼들을 종합적으로 정리하여 아래와 같이 보여준다.

```
C:\Windows\System32>DUMPBIN /IMPORTS:ntdll.dll ntshrui.dll
Dump of file C:\Windows\System32\ntshrui.dll

File Type: DLL

  Section contains the following imports:

    ntdll.dll
             180086388 Import Address Table
             18009C850 Import Name Table
                     0 time date stamp
                     0 Index of first forwarder reference

                         36A RtlCreateUnicodeString
                         713 WinSqmAddToStream
                         52A RtlNtStatusToDosError
                         3B0 RtlDosPathNameToNtPathName_U
                         1AC NtOpenFile
                         419 RtlFreeUnicodeString
                          38 EtwEventActivityIdControl
                         517 RtlMapGenericMask
                          4F EtwRegisterTraceGuidsW
                          56 EtwUnregisterTraceGuids
                          47 EtwGetTraceEnableLevel
                          48 EtwGetTraceLoggerHandle
                          54 EtwTraceMessage
                          45 EtwEventWriteTransfer
                          3C EtwEventSetInformation
                          46 EtwGetTraceEnableFlags
                          3B EtwEventRegister
                          3D EtwEventUnregister

  Section contains the following delay load imports:
```
