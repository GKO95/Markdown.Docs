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

## .edata 섹션
**[.edata 섹션](https://learn.microsoft.com/windows/win32/debug/pe-format#the-edata-section-image-only)**(export data section)은 동적 링크로 다른 프로그램이 접근할 수 있도록 DLL이 노출한 [심볼](Symbol.md) 정보들이 담겨있다. 흔히 [`__declspec(dllexport)`](https://learn.microsoft.com/en-us/cpp/build/exporting-from-a-dll-using-declspec-dllexport) 키워드와 함께 선언된 변수, 함수 프로토타입, 또는 C++ 클래스가 해당한다. [Export Address Table](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#export-address-table)은 내보내진 심볼들을 알파벳 순서대로 순번(ordinal)과 함께 나열하고 이들의 RVA는 다음 둘 중 한 가지 방법으로 안내한다.

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Export Address Table의 RVA<sup>†</sup> 유형</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">Export RVA</th><th style="text-align: center;">Forwarder RVA</th></tr></thead><tbody><tr style="text-align: center;"><td>.edata 섹션 범위 밖의 주소를 가리킨다.</td><td>.edata 섹션 범위 안의 주소를 가리킨다.</td></tr><tr><td>DLL에 정의된 변수, 함수, 클래스 등의 코드가 위치한 상대적 주소</td><td><a href="https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#export-name-table">Export Name Table</a>에 명시된 심볼명 문자열을 가리키는 포인터가 위치한 상대적 주소</td></tr></tbody></table>

<sup>_† RVA(relative virtual address)는 모듈이 가상 주소 공간에 매핑되었을 때 "BaseImage + RVA", 즉 DLL 이미지의 기반 주소로부터 상대적 거리를 제시하여 가상 주소를 찾아갈 수 있도록 한다._</sup>

Export 섹션에서 심볼을 내보내기 위해서 Export Address Table는 반드시 필요한 존재이다; 심볼에 순번을 매기는 건 기본적인 구조이다. 반면, Export Name Table은 심볼을 이름으로 탐색하기 위한 선택 사항이며, 이를 활용하기 위해서는 아래 두 테이블의 도움이 필요하다.

> Export Name Table은 private 심볼을 내보낼 때는 다른 이름의 public 심볼로 명명할 수 있으며, 뿐만 아니라 타 DLL의 심볼로 안내하는 포워딩에 활용된다.

* [Export Name Pointer Table](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#export-name-pointer-table): Export Name Table의 심볼명 문자열로 가리키는 포인터를 저장하며, 이진 검색을 허용하기 위해 심볼의 알파벳 순서로 나열한다.
* [Export Ordinal Table](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#export-ordinal-table): Export Name Pointer Table의 포인터와 동일한 인덱스에 해당 심볼의 순번을 저장한다. <sub><i>(Biased ordinal = Ordinal - Ordinal base)</i></sub>

두 배열은 한 쌍을 이루어 병행으로 동작하지만, 이렇게 나누어서 관리하는 이유는 "자연스러운" [데이터 정렬](Memory.md#데이터-정렬)을 유지하기 위해서이다.

## .idata 섹션
**[.idata 섹션](https://learn.microsoft.com/windows/win32/debug/pe-format#the-idata-section)**(import data section)은 프로그램이 실행될 때 동적 링크로 불러와야 할 DLL 모듈과 각각 참조된 심볼들을 목록이 담겨있다. C 표준의 [`extern`](C.md#변수) 키워드로 심볼들을 불러올 수 있지만, [`__declspec(dllimport)`](https://learn.microsoft.com/en-us/cpp/build/importing-into-an-application-using-declspec-dllimport) 키워드로 선언하면 약간 더 효율적인 코드를 컴파일한다.
