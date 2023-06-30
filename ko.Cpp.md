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

## 프로젝트
다음은 비주얼 스튜디오 2022을 위주로 C++ 프로젝트 구축에 대하여 설명한다.

![비주얼 스튜디오 C++ 프로그래밍을 위한 구성요소](./images/visual_studio_cpp.png)

아래는 C++를 실행하는 가장 기초적인 코드와 함께 코드에 대한 설명이다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">간단한 C++ 코드 및 설명</caption>
<colgroup><col style="width: 50%;"/><col styl="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">코드</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr>
<td>

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello World!\n";
}
```
</td><td>
<ul><li><code>#include &lt;iostream&gt;</code><br/>Iostream <a href="#헤더-파일">헤더 파일</a>로부터 C++ 표준 입출력 라이브러리를 <a href="#포함-지시문">불러오며</a>, 터미널에 텍스트를 출력하는 <a href="#파일-입출력"><code>std::cout</code></a> 등을 제공한다.</li><li><code>int main() { ... }</code><br/>C++가 시작되는 함수, 일명 <a href="#진입점">진입점</a>이다.</li></ul>
</td></tr></tbody>
</table>

# 구문
[구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)은 프로그래밍 언어에서 문자 및 기호들의 조합이 올바른 문장 또는 표현식을 구성하였는지 정의하는 규칙이다. 각 프로그래밍 언어마다 규정하는 구문이 다르며, 이를 준수하지 않을 시 해당 프로그램은 빌드되지 않거나, 실행이 되어도 오류 및 의도치 않은 동작을 수행한다.

다음은 C 언어에서 구문에 관여하는 요소들을 소개한다:

* **[표현식](https://ko.wikipedia.org/wiki/식_(프로그래밍))(expression)**
    
    값을 반환하는 구문적 존재를 가리킨다. 표현식에 대한 결과를 도출하는 것을 평가(evaluate)라고 부른다.
    
    ```cpp
    2 + 3           // 숫자 5를 반환
    2 < 3           // 논리 참을 반환
    ```

* **[토큰](https://learn.microsoft.com/en-us/cpp/cpp/character-sets)(token)**

    표현식을 구성하는 가장 기본적인 요소이며, 대표적으로 [키워드](https://learn.microsoft.com/en-us/cpp/cpp/keywords-cpp)(keyword), [식별자](#식별자)(identifier), [숫자](https://learn.microsoft.com/en-us/cpp/cpp/numeric-boolean-and-pointer-literals-cpp) 및 [문자열 리터럴](https://learn.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp)(literal) 등이 있다.

    ```cpp
    variable        // 식별자
    2               // 정수 리터럴
    ```

* **[문장](https://ko.wikipedia.org/wiki/문_(프로그래밍))(statement)**
    
    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 러스트 프로그래밍 언어는 [세미콜론](https://ko.wikipedia.org/wiki/새줄_문자)(semicolon) `;`을 기준으로 문장을 분별한다. 

    ```cpp
    int variable = 2 + 3;      // 숫자 5를 "variable" 변수에 초기화
    if (2 < 3) statement;      // 논리가 참이면 "statement" 문장 실행
    ```

* **[블록](https://ko.wikipedia.org/wiki/블록_(프로그래밍))(block)**

    한 개 이상의 문장들을 한꺼번에 관리할 수 있도록 묶어놓은 소스 코드상 그룹이다. 블록 안에 또 다른 블록이 상주할 수 있으며, 이를 네스티드 블록(nested block)이라고 부른다. C++에서는 한 쌍의 중괄호 `{}`로 표시된다.

    ```cpp
    {
        int variable = 2 + 3;
        if (2 < 3) statement;
    }
    ```

### 식별자
[식별자](https://learn.microsoft.com/en-us/cpp/cpp/identifiers-cpp)(identifier)는 프로그램을 구성하는 데이터들을 구별하기 위해 사용되는 명칭이다. 즉, 식별자는 개발자가 데이터에 직접 붙여준 이름이다. C++에서 식별자를 선정하는데 아래의 규칙을 지켜야 한다.

1. 알파벳, 숫자, 밑줄 `_`만 허용 (그 외 특수문자 및 공백 사용 불가)
2. 식별자의 첫 문자는 숫자가 될 수 없음
3. 대소문자 구분 필수
4. [예약어](https://ko.wikipedia.org/wiki/예약어) 금지

### 주석
[주석](https://learn.microsoft.com/en-us/cpp/cpp/comments-cpp)(comment)은 프로그램의 소스 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데, C++에는 한줄 주석 그리고 블록 주석이 존재한다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">C++ 주석 종류</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">한줄 주석</th><th style="text-align: center;">블록 주석</th></tr></thead>
<tbody>
<tr><td colspan="2">주석은 컴파일 직전에 <a href="#전처리기">전처리기</a>에 의해 소스 코드에 제거된다. 즉, 실행 파일 안에는 주석의 어떠한 정보도 저장되지 않는다.</td></tr>
<tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```cpp
// 한줄 주석: 코드 한 줄을 차지하는 주석이다.
```
</td><td>

```cpp
/*
블록 주석:
코드 여러 줄을 차지하는 주석이다.
*/
```
</td></tr>
</tbody>
</table>

# 전처리기
C++가 컴파일되기 이전에 전처리기로부터 `#include`와 같은 전처리기 지시문이 우선적으로 처리된다. 전처리기 지시문은 C++ 컴파일러 설정 및 프로그래밍의 편리성을 제공한다. 본 장에서는 일부 유용한 전처리기 지시문에 대하여 소개한다.

## 매크로 정의
[매크로](https://en.cppreference.com/w/cpp/preprocessor/replace)(macro)란 식별자가 있는 코드 조각이다. 코드 조각은 숫자나 문자와 같은 간단한 데이터가 될 수 있으며(객체형식 매크로; object-like macro), 전달인자를 받는 표현식이나 문장이 될 수도 있다(함수형식 매크로; function-like macro). 매크로는 `#define` 지시문으로 정의되며, 각 매크로에 해당하는 데이터 및 표현식이 소스 코드에 대체된다. 정의된 매크로는 `#undef` 지시자로 제거할 수 있다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">C++의 매크로 유형</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">객체형식 매크로</th><th style="text-align: center;">함수형식 매크로</th></tr></thead>
<tbody><tr style="vertical-align: top;"><td>

```cpp
#define MACRO    7

printf("%d", MACRO);

#undef MACRO
```
</td><td>

```cpp
#define MACRO(x, y)    (x * y)

printf("%d", MACRO(3, 4));

#undef MACRO
```
</td></tr><tr style="vertical-align: top;"><td>

```terminal
7
```
</td><td>

```terminal
12
```
</td></tr>
</tbody>
</table>

한 번 정의된 매크로는 런타임 도중에 변경이 불가하다. 정의된 매크로는 마치 전역 변수인 마냥 헤더 파일에서 `#include`와 같은 포함 지시문을 통해 다른 스크립트에서도 사용할 수 있다.

컴파일러에는 공통된 표준 매크로 및 컴파일러마다 전용 매크로가 내장되어 있다.

* **Visual C++**: [Microsoft Docs - 미리 정의된 매크로](https://learn.microsoft.com/en-us/cpp/preprocessor/predefined-macros)
* **GCC**: [GCC Online Documentation - Predefined Macros](https://gcc.gnu.org/onlinedocs/cpp/Predefined-Macros.html)
* **그 외**: [SourceForge Wiki](https://sourceforge.net/p/predef/wiki/Compilers/)

### 쉼표 연산자
[쉼표 연산자](https://en.cppreference.com/w/cpp/language/operator_other)(comma operator)는 앞에 있는 표현식을 평가하되 반환되지 않고, 뒤에 있는 표현식이 평가되어 반환된다. 흔히 매크로 정의를 간결하게 하기 위해 사용된다. 아래의 예시 코드에 의하면 먼저 할당 연산자로 `value1`은 4가 되고, 이후에 증가 연산자에 의해 5가 된다.

```cpp
int value1 = 1, value2 = 3;
int variable = (value1 += value2, ++value1);
printf("%d", variable);
```
```terminal
5
```

## 조건 포함문
[조건 포함문](https://en.cppreference.com/w/cpp/preprocessor/conditional)(conditional inclusion)은 조건여부에 따라 컴파일 시 특정 범위의 코드를 포함시킬 것인지 배제할 것인지 결정한다. 비록 조건 포함문이 일반 조건문의 키워드와 유사할지라도 절대 [`if`](#if-조건문) 및 [`else`](#if-조건문) 조건문을 대체하기 위해 사용되지 말아야 한다.

```cpp
#if    SOMETHING > value
    ...
#elif  SOMETHING < value
    ...
#else
    ...
#endif
```


### 매크로 조건
조건 포함문은 매크로의 정의 여부를 판단할 수 있다.

```cpp
// 만일 64비트 ARM 혹은 x64 아키텍쳐로 컴파일 할 경우...
#ifdef    _WIN64

    ...

#endif

// 만일 64비트 ARM 혹은 x64 아키텍쳐로 컴파일되지 않은 경우...
#ifndef   _WIN64

    ...

#endif
```

## Pragma 지시문
[Pragma 지시문](https://en.cppreference.com/w/cpp/preprocessor/impl)(pragma directive)은 컴파일러의 기능과 옵션을 설정하기 위해 사용되는 전처리기 지시문이다. 개발사마다 제작한 컴파일러는 기술적 성능이 각각 다르기 때문에 pragma는 비공통적인 컴파일러 특정 전처리기 지시문이다.

> Pragma란 용어는 pragmatic의 줄임말로, 사전적 의미로는 "실용적인"을 뜻한다. 이는 실질적 컴파일러 동작 및 처리 방식에 관여한 것을 보아 붙여진 용어라고 판단된다.

* **Visual C++**: [Microsoft Docs - Pragma Directives and the Pragma Keyword](https://learn.microsoft.com/en-us/cpp/preprocessor/pragma-directives-and-the-pragma-keyword)
* **GCC**: [GCC Online Documentation - Pragmas](https://gcc.gnu.org/onlinedocs/gcc/Pragmas.html)

본 장은 마이크로소프트의 비주얼 스튜디어에서 제공하는 Visual C++ 컴파일러의 pragma 지시문을 위주로 다룬다.

### `#pragma once`
`#pragma once`는 컴파일 작업 시 `#include` 지시문을 통해 중복 포함된 헤더 파일을 한 번만 포함시키는 pragma 지시문이다.

```cpp
#pragma once
```

결과적으로 하나의 소스 파일에 헤더 파일이 중복적으로 포함이 되는 것을 제한하므로써 정의가 반복되는 현상을 막을 수 있는데, 이러한 기능을 [헤더 중복 방지](https://en.wikipedia.org/wiki/Include_guard)(include guard)라고 부른다. 추가적으로 `#pragma once` 지시문을 사용하면 처리하는 헤더 파일 횟수가 줄어들어 컴파일 작업 시간도 함께 줄이게 된다.

아래의 코드는 `#pragma once` 지시문을 사용하지 않고 헤더 중복 방지 기능을 구현하는 방법이다.

```cpp
// 헤더 파일: "header.h"
#ifndef HEADER_FILE
#define HEADER_FILE

    ...

#endif
```

만일 `header.h` 헤더 파일이 아직 처리되지 않았으면 컴파일러는 처음으로 `HEADER_FILE` 매크로를 정의한다. 그러나 헤더 파일을 다시 한 번 마주하였을 시, `HEADER_FILE`이 이미 정의되어 있기에 매크로 조건에 의해 컴파일러는 헤더 파일을 처리하지 않는다.

### `#pragma region`
컴파일 작업에는 직접적인 영향을 주지 않지만, `#pragma region` 및 `#pragma endregion` 쌍은 가독성을 위해 비주얼 스튜디오에서 지정된 코드 부분을 한 줄로 압축하거나 펼치는 기능을 제공한다.

```cpp
#pragma region REGIONNAME

    ...

#pragma endregion REGIONNAME
```
