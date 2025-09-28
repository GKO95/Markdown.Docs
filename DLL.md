# 동적 링크 라이브러리
> *참고: [Dynamic link library (DLL) - Windows Client | Microsoft Learn](https://learn.microsoft.com/troubleshoot/windows-client/setup-upgrade-and-drivers/dynamic-link-library)*

**[동적 링크 라이브러리](https://en.wikipedia.org/wiki/Dynamic-link_library)**(dynamic-link library; DLL)는 [함수](C.md#함수), [클래스](Cpp.md#클래스), [변수](C.md#변수), 또는 아이콘과 같은 리소스를 담고 있는 [컴파일](Programming.md#컴파일러)된 .dll 확장자 [모듈](https://en.wikipedia.org/wiki/Modular_programming)이다. DLL을 참조한 프로그램은 [런타임](https://en.wikipedia.org/wiki/Execution_(computing)#Runtime) 도중에 모듈 이미지를 [프로세스](Process.md)의 [가상 주소 공간](Process.md#가상-주소-공간)에 [매핑](Memory.md#메모리-맵-파일)한다. 이러한 동작은 프로그램 이미지 크기를 상당히 줄일 수 있고 라이브러리 업데이트가 매우 편리한 장점을 지닌다.

다음은 Windows NT에서 대표적인 DLL 일부를 나열 및 소개한다.

* Kernel32.dll: [메모리](Memory.md), [프로세스](Process.md) 및 [스레드](Thread.md)를 관리하는 함수들을 제공한다.
* User32.dll: 창 생성 및 메시지 전송 등 사용자 상호작용 작업에 동원되는 함수들을 제공한다.
* GDI32.dll: 시각적 이미지를 그리거나 텍스트를 표시하는 함수들을 제공한다.

### 정적 링크 라이브러리
**[정적 링크 라이브러리](https://en.wikipedia.org/wiki/Static_library)**(static-link library; LIB)는 [DLL](#동적-링크-라이브러리)과 마찬가지로 함수, 클래스, 변수 등을 담고 있는 .lib 확장자 모듈읻다. 다만, 빌드 과정 중 [링크 타임](Programming.md#링커) 때 모듈이 프로그램 이미지에 아예 통합되는 차이점이 있다. 빌드된 프로그램은 이미지 크기가 커지겠지만, 라이브러리 파일의 부재나 의존성에 의한 오류를 방지하여 안정성을 높을 수 있다.

한편, DLL 모듈이 한 개 이상의 함수나 변수를 내보낸다면<sup>[[참고](https://learn.microsoft.com/cpp/build/exporting-from-a-dll)]</sup> 빌드 과정에서 [링커](Programming.md#링커)는 단일 .lib 파일을 함께 생성한다: 어떠한 코드도 정의되어 있지 않으며, 단순히 내보내진 함수 및 변수의 심볼 목록이 나열된 매우 작은 파일이다. 해당 DLL의 심볼을 참조하는 타 프로그램을 빌드할 시, 링커는 .lib 파일을 단순히 참조한 외부 심볼이 어느 DLL에 있는지 파악하는 단서로써 활용한다.

## DLL 불러오기
DLL의 일부 동작 원리를 이해하기 위해서는 이에 대한 프로그래밍 관련 내용을 함께 살펴본다. [Visual Studio](https://visualstudio.microsoft.com/)의 일환인 [DUMPBIN](https://learn.microsoft.com/cpp/build/reference/dumpbin-reference) 도구는 실행 프로그램 및 DLL 파일의 정보를 보여주는 데 유용하기 때문에 활용해 보는 걸 적극 권장한다.

아래는 Visual Studio 프로젝트에서 소스 코드를 DLL 모듈로 빌드하기 위해 필요한 설정을 보여준다.

![Visual Studio에서 DLL 빌드를 위한 프로젝트 설정](./images/visual_studio_library.png)

다음 키워드로 선언된 변수, 함수 프로토타임, 그리고 C++ 클래스는 링커에 의해 다음과 같이 처리된다.

<table style="width: 75%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">심볼 내보내기 및 불러오기 키워드</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://learn.microsoft.com/cpp/build/exporting-from-a-dll-using-declspec-dllexport"><code>__declspec(dllexport)</code></a></th><th style="text-align: center;"><a href="https://learn.microsoft.com/cpp/build/importing-into-an-application-using-declspec-dllimport"><code>__declspec(dllimport)</code></a></th></tr></thead><tbody><tr style="text-align: center;"><td>선언된 심볼들은 <a href="PE.md#export-섹션">Export 섹션</a>에 나열된다.</td><td>선언된 심볼들은 <a href="PE.md#import-섹션">Import 섹션</a>에 나열하지만 필수는 아니며, C 표준의 <a href="C.md#변수"><code>extern</code></a> 키워드만으로도 충분하다.</td></tr></tbody></table>

[프로세스](Process.md)가 실행될 때, [로더](https://en.wikipedia.org/wiki/Loader_(computing))(loader)는 Import 섹션에서 명시된 필수 DLL 이미지 파일을 정해진 디렉토리 순서에 따라 탐색하여<sup>[[참고](https://learn.microsoft.com/windows/win32/dlls/dynamic-link-library-search-order)]</sup> [가상 주소 공간](Process.md#가상-주소-공간)으로 매핑한다.
이후 불러올 심볼이 DLL에 존재하는지 확인하기 위해 매핑된 이미지의 Export 섹션을 검토하고, 특이 사항이 없다면 [Import Address Table](PE.md#import-섹션)의 RVA를 실제 가상 메모리 주소로 대체한다. 하지만 Export 섹션에서 심볼이 발견되지 않으면 0xC0000139 STATUS_ENTRYPOINT_NOT_FOUND 오류를 반환한다.

> DLL을 불러오는 작업은 프로세스 초기화 작업에 이루어지기 때문에 런타임 성능에 영향을 주지 않지만, 로드할 DLL 개수가 많아지면 초기화 시간이 길어지게 만든다.

### DLL 지연 로드
**[DLL 지연 로드](https://learn.microsoft.com/cpp/build/reference/linker-support-for-delay-loaded-dlls)**(Delay Loading DLL)는 링크가 내재되어 있으나 해당 모듈의 심볼을 참조할 때까지 DLL을 아직 로드하지 않는 기술이다. 프로세스 초기화 과정에 다수의 DLL을 불러오는데 소요되는 시간을 분산시키는 등의 용도로 활용될 수 있다. 단, DLL 지연 로드에는 아래와 같이 제약이 존재한다.

* 변수만 내보내는 DLL은 지연 로드가 불가하다.
* Kernel32.dll은 모듈을 불러오기 위해 필요한 [LoadLibrary](https://learn.microsoft.com/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryw) 및 [GetProcAddress](https://learn.microsoft.com/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress) 함수를 제공하기 때문에 DLL 지연 로드가 불가하다.
* DllMain 진입점 안에서 지연 로드되는 DLL의 함수를 호출하면 프로세스가 충돌할 수 있어 기피해야 한다.

DLL의 지연 로드되는 과정에서 자체 제작한 코드를 수행하도록 [후킹](#후킹)을 지원하며, 자세한 내용을 확인하려면 마이크로소프트 [기술 문서](https://learn.microsoft.com/cpp/build/reference/understanding-the-helper-function)를 참고한다.

## 알려진 DLL
Windows OS에서 제공하는 몇몇 DLL은 특수한 취급을 받으며, 이들의 집합을 **[알려진 DLL](https://www.oreilly.com/library/view/windows-r-via-c-c/9780735639904/ch20s05.html)**(Known DLLs)이라고 부른다. 일반 DLL과 다른 점이 없으나 운영체제는 해당 DLL을 불러오려면 어느 디렉토리에 위치하는지 아래 레지스트리 키로부터 이미 파악하고 있어 (1) 탐색 시간을 줄이고 (2) 동명의 DLL을 잘못 불러오는 참사를 방지한다.

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs

LoadLibrary 함수에 모듈명만 기입하면 일반적인 DLL 탐색 순서를 거치지만, 만일 .dll 확장자까지 함께 명시한다면 KnwonDLLs 레지스트리 키에 해당 모듈명과 일치하는 항목이 존재하는지 우선 살펴본다. 존재가 확인되면 DllDirectory에서 제시한 디렉토리 (기본값: %SystemRoot%\System32)에서 DLL 모듈을 불러온다.

## DLL 주소 재배치
모든 DLL 모듈은 프로세스에 로드될 때, 가상 주소 공간로 매핑될 선호되는 로드 주소를 가지고 있다.

<table style="width: 60%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">모듈 유형과 이미지 크기에 따른 기본 선호 로드 주소</caption><colgroup><col style="width: 20%;"/><col style="width: 40%;"/><col style="width: 40%;"/></colgroup><thead><tr><th style="text-align: center;">크기</th><th style="text-align: center;">EXE 모듈</th><th style="text-align: center;">DLL 모듈</th></tr></thead><tbody><tr><td style="text-align: center;">32비트</td><td>0x00400000</td><td>0x10000000</td></tr><tr><td style="text-align: center;">64비트</td><td>0x00000001`40000000</td><td>0x00000001`80000000</td></tr></tbody></table>

<sup>_† DLL의 로드 주소는 반드시 64 KB 크기의 [할당 입도](Memory.md#페이지)(allocation granularity) 경계 시작 주소에 맞추어야 하며, 즉 0x00010000의 배수이어야 한다._</sup>

만일 불러오는 DLL 간 선호 로드 주소가 겹치게 될 경우, [로더](https://en.wikipedia.org/wiki/Loader_(computing))는 나중에 불러온 DLL의 [Relocation 섹션](PE.md#relocation-섹션)을 [copy-on-write](Memory.md#copy-on-write) 기법으로 새로운 가상 메모리 주소로 변경한다. 다만, Relocation 섹션의 모든 항목을 살펴봐야 하기 때문에 초기화에 시간이 상당히 소요되고 COW에 의한 [페이징 파일](Memory.md#페이징-파일) 의존성이 강제되는 성능 저하가 유발될 수 있기 때문에 가급적 컴파일 때부터 이미지 로드 주소가 중복되지 않도록 선정할 필요가 있다.

### 주소 공간 레이아웃 임의화
> *참고: [/DYNAMICBASE (Use address space layout randomization) | Microsoft Learn](https://learn.microsoft.com/cpp/build/reference/dynamicbase-use-address-space-layout-randomization)*

**[주소 공간 레이아웃 임의화](https://en.wikipedia.org/wiki/Address_space_layout_randomization)**(Address Space Layout Randomization), 일명 **ASLR**은 [컴퓨터 보안](https://en.wikipedia.org/wiki/Computer_security) 기법 중 하나로, 모듈에 정의된 로드 기반 주소를 악용하여 악성 코드를 대신 실행시키는 행위를 방지하기 위해 주소 공간의 핵심 데이터 영역을 무작위로 조정한다.

# 후킹
**[후킹](https://en.wikipedia.org/wiki/Hooking)**(hooking)은 기존 프로그램의 [함수 호출](C.md#함수), 이벤트, 또는 메시지를 가로채어 본래 동작을 바꾸거나 변조시키는 기술이다.
