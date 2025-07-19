# 동적 링크 라이브러리
**[동적 링크 라이브러리](https://en.wikipedia.org/wiki/Dynamic-link_library)**(dynamic-link library; DLL)는 [함수](C.md#함수), [클래스](Cpp.md#클래스), [변수](C.md#변수), 또는 아이콘과 같은 리소스를 담고 있는 [컴파일](Programming.md#컴파일러)된 .dll 확장자 [모듈](https://en.wikipedia.org/wiki/Modular_programming)이며, 이를 원하는 다른 프로그램이 활용할 수 있도록 제공한다. [정적 링크 라이브러리](https://en.wikipedia.org/wiki/Static_library)는 컴파일 대상의 프로그램이 빌드되는 과정에서 융합되는 반면, DLL은 필요할 때 모듈 이미지를 [프로세스](Process.md)의 [가상 주소 공간](Process.md#가상-주소-공간)에 [매핑](Memory.md#메모리-맵-파일)된다. 이러한 동작은 프로그램 이미지 용량을 상당히 줄일 수 있고 라이브러리 업데이트가 매우 편리한 장점을 지닌다.

다음은 Windows NT에서 대표적인 DLL 일부를 나열 및 소개한다.

* Kernel32.dll: [메모리](Memory.md), [프로세스](Process.md) 및 [스레드](Thread.md)를 관리하는 함수들을 제공한다.
* User32.dll: 창 생성 및 메시지 전송 등 사용자 상호작용 작업에 동원되는 함수들을 제공한다.
* GDI32.dll: 시각적 이미지를 그리거나 텍스트를 표시하는 함수들을 제공한다.

## DLL 모듈 아키텍처
