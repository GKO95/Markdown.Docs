---
category: 프로그래밍
title: C++
---
# C++
[C++](https://learn.microsoft.com/en-us/cpp/cpp/)는 본래 [클래스](#클래스)를 지원하는 [C 언어](ko.C.md)의 확장판으로 시작된 고급 범용 프로그래밍 언어로, 현재는 그 외에도 [함수](#함수)를 더 유연하게 사용할 수 있거나 [템플릿](#템플릿) 등 다양한 기능을 지원한다. 이러한 유래로 C++는 C 언어와 상당한 [호환성](https://en.wikipedia.org/wiki/Compatibility_of_C_and_C%2B%2B)을 자랑하지만, 오랜 역사를 거쳐온 탓에 엄연히 서로 다른 언어이다. C++는 현재 [데스크탑](https://ko.wikipedia.org/wiki/응용_소프트웨어)이나 [서버](https://ko.wikipedia.org/wiki/서버), 또는 [운영체제](ko.Windows.md)와 같은 [임베디드](https://en.wikipedia.org/wiki/Embedded_software) 분야에서도 활약하고 있다.

C++ 언어에 필요한 컴파일러는 흔히 소스 코드 편집, 프로그램 빌드, 그리고 디버깅 기능을 제공하는 [통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)을 설치하면 대체로 권장되는 [컴파일러](#컴파일러)가 함께 설치된다.

> [비주얼 스튜디오 코드](https://code.visualstudio.com/)(Visual Studio Code; VS Code)는 엄연히 말해 "텍스트 편집기"이며, 아래의 IDE를 사용할 것을 권장한다.

* [비주얼 스튜디오](https://visualstudio.microsoft.com/) <sub>(윈도우, macOS)</sub>
* [엑스코드](https://developer.apple.com/xcode/) <sub>(macOS)</sub>
* [CLion](https://www.jetbrains.com/clion/) <sub>(윈도우, macOS, 리눅스)</sub>

## 컴파일러
C++ 언어는 [컴파일 언어](ko.Compiler.md)(compiled language)이다. C++ 컴파일러는 [국제 표준화 기구](https://www.iso.org/home.html)(International Organization for Standardization; ISO)에서 표준을 발표한 년도에 따라 버전이 나뉘어진다. 본격적으로 표준화된 버전은 C++98이지만, 본 문서는 다양한 기능들이 추가된 <span style="color: red;">*C++11 표준*</span>을 위주로 설명한다.

컴파일러는 개발사와 목적에 따라 다양한 종류가 존재하지만, 전부 동일한 ISO 표준에 따라 동작하므로 일반적인 경우에는 어떤 컴파일러를 사용하던 무관하다. 아래는 대표적인 C++ 언어 컴파일러들을 나열한다.

* [Microsoft Visual C++](https://ko.wikipedia.org/wiki/마이크로소프트_비주얼_C%2B%2B) (일명 MSVC): 마이크로소프트
* [GNU C Compiler](https://ko.wikipedia.org/wiki/GNU_C_컴파일러) (일명 GCC): GNU 프로젝트
* [Clang](https://ko.wikipedia.org/wiki/클랭): LLVM Developer Group, 애플
