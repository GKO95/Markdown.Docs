# C
[C 언어](https://learn.microsoft.com/en-us/cpp/c-language/)는 [유닉스](https://ko.wikipedia.org/wiki/유닉스)(UNIX) 컴퓨터를 위한 소프트웨어 제작을 위해 개발된 [B 언어](https://ko.wikipedia.org/wiki/B_(프로그래밍_언어))의 후속작이다. 현재 C 언어는 가장 널리 사용되고 있는 프로그래밍 언어로 [C++](Cpp.md), [C#](Csharp.md), [파이썬](Python.md), 자바 등 여러 프로그래밍 언어에 영향을 주었다. C 언어는 다른 프로그래밍 언어에 비해 매우 빠른 처리 속도와 훌륭한 호환성을 가지고 있어 [응용 프로그램](https://ko.wikipedia.org/wiki/응용_소프트웨어) 및 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 개발에 여전히 활용되고 있다.

C 언어에 필요한 컴파일러는 흔히 소스 코드 편집, 프로그램 빌드, 그리고 [디버깅](https://ko.wikipedia.org/wiki/디버그) 기능을 제공하는 [통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)을 설치하면 대체로 권장되는 [컴파일러](#컴파일러)가 함께 설치된다.

> [비주얼 스튜디오 코드](https://code.visualstudio.com/)(Visual Studio Code; VS Code)는 엄연히 말해 "텍스트 편집기"이며, 아래의 IDE를 사용할 것을 권장한다.

* [비주얼 스튜디오](https://visualstudio.microsoft.com/) <sub>(윈도우, macOS)</sub>
* [엑스코드](https://developer.apple.com/xcode/) <sub>(macOS)</sub>
* [CLion](https://www.jetbrains.com/clion/) <sub>(윈도우, macOS, 리눅스)</sub>

## 컴파일러
C 언어는 [컴파일 언어](Compiler.md)(compiled language)이다. C 컴파일러는 [국제 표준화 기구](https://www.iso.org/home.html)(International Organization for Standardization; ISO)에서 표준을 발표한 년도에 따라 버전이 나뉘어진다. 가장 널리 사용되고 있는 버전으로는 ANSI C(일명 C89)와 C99가 있다. 본 문서는 <span style="color: red;">*ANSI C 컴파일러 기준*</span>으로 C 언어를 설명한다.

컴파일러는 개발사와 목적에 따라 다양한 종류가 존재하지만, 전부 동일한 ISO 표준에 따라 동작하므로 일반적인 경우에는 어떤 컴파일러를 사용하던 무관하다. 아래는 대표적인 C 언어 컴파일러들을 나열한다.

* [Microsoft Visual C++](https://ko.wikipedia.org/wiki/마이크로소프트_비주얼_C%2B%2B) (일명 MSVC): 마이크로소프트
* [GNU C Compiler](https://ko.wikipedia.org/wiki/GNU_C_컴파일러) (일명 GCC): GNU 프로젝트
* [Clang](https://ko.wikipedia.org/wiki/클랭): LLVM Developer Group, 애플

## 프로젝트
다음은 비주얼 스튜디오 2022을 위주로 C 언어 프로젝트 구축에 대하여 설명한다.

![비주얼 스튜디오 C 언어 프로그래밍 화면](./images/visual_studio_c.png)

엑스코드와 달리, 비주얼 스튜디오는 C 언어를 위한 별도 프로젝트 생성 옵션이 존재하지 않는다. 대안으로 [C++](Cpp.md)에서 빈 프로젝트(Empty Project)를 생성한 다음, 소스 코드를 .c 확장자로 직접 추가할 수 있다. MSVC 컴파일러는 C++를 주요 대상으로 설계된 컴파일러이지만, C 언어도 함께 지원하기 때문이다.

아래는 C 언어 프로그램을 실행하는 가장 기초적인 코드와 함께 코드에 대한 설명이다.

* `#include <stdio.h>`: Stdio.h [헤더 파일](#헤더-파일)로부터 C 표준 입출력 라이브러리를 [불러오며](#포함-지시문), 터미널에 텍스트를 출력하는 [`printf()`](#파일-입출력) [함수](#함수) 등을 제공한다.
* `int main() { ... }`: C 언어가 시작되는 함수, 일명 [진입점](#진입점)이다.
* `return 0;`: 함수를 종료하며 0 값을 반환하는 데, 이는 `EXIT_SUCCESS`에 대응하며 성공적으로 프로그램이 종료되었음을 의미한다.

### C 런타임 라이브러리
> *출처: [Security Features in the CRT | Microsoft Learn](https://learn.microsoft.com/en-us/cpp/c-runtime-library/security-features-in-the-crt)*

C [런타임 라이브러리](https://ko.wikipedia.org/wiki/런타임_라이브러리), 일명 CRT는 C/C++ 프로그램이 [런타임 환경](https://ko.wikipedia.org/wiki/런타임_시스템)에서 동작하기 위해 필요한 함수들의 집합체이다. 대표적으로 메모리 할당 및 해제, 파일 입출력, 문자열 조작, 예외 처리 등이 해당한다. 매우 기초적이지만 필연적인 기능이므로, 일부 핵심 CRT(예를 들어 `malloc`, `printf`, `strcpy` 등)는 운영체제가 설치될 때 함께 내장되기도 한다. 런타임 라이브러리는 개발자나 서드 파티의 (정적 혹은 동적) [라이브러리](#라이브러리)와 전혀 다른 개념이므로 혼돈되어서는 안된다.

비주얼 스튜디오 2015 이후부터 `_s` 접미사가 붙은 안정성이 강화된 새로운 CRT 함수들이 소개되며(예를 들어 [`printf_s`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/printf-s-printf-s-l-wprintf-s-wprintf-s-l), [`strcpy_s`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/strcpy-s-wcscpy-s-mbscpy-s) 등), 기존의 옛 CRT 함수들을 사용하려 할 시 [C4996](https://learn.microsoft.com/en-us/cpp/error-messages/compiler-warnings/compiler-warning-level-3-c4996) 컴파일러 경고가 나타난다. 하지만 옛 CRT의 활용이 권장되지 않을 뿐, 사라진 게 아니므로 경고를 무시하려면 아래와 같이 매크로를 소스 코드 또는 프로젝트에 정의한다.

```c
#define _CRT_SECURE_NO_WARNINGS
```

![비주얼 스튜디오 프로젝트에 <code>CRT_SECURE_NO_WARNINGS</code> 매크로 설정](./images/visual_studio_crt_warnings.png)

# 구문
[구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)은 프로그래밍 언어에서 문자 및 기호들의 조합이 올바른 문장 또는 표현식을 구성하였는지 정의하는 규칙이다. 각 프로그래밍 언어마다 규정하는 구문이 다르며, 이를 준수하지 않을 시 해당 프로그램은 빌드되지 않거나, 실행이 되어도 오류 및 의도치 않은 동작을 수행한다.

다음은 C 언어에서 구문에 관여하는 요소들을 소개한다:

* **[표현식](https://en.wikipedia.org/wiki/Expression_(computer_science))(expression)**
    
    값을 반환하는 구문적 존재를 가리킨다. 표현식에 대한 결과를 도출하는 것을 평가(evaluate)라고 부른다.
    
    ```c
    2 + 3           // 숫자 5를 반환
    2 < 3           // 논리 참을 반환
    ```

* **[토큰](https://learn.microsoft.com/en-us/cpp/c-language/c-tokens)(token)**

    표현식을 구성하는 가장 기본적인 요소이며, 대표적으로 [키워드](https://learn.microsoft.com/en-us/cpp/c-language/c-keywords)(keyword), [식별자](#식별자)(identifier), [상수](https://learn.microsoft.com/en-us/cpp/c-language/c-constants)(constant), [문자열 리터럴](https://learn.microsoft.com/en-us/cpp/c-language/c-string-literals)(string literal) 등이 있다.

    ```c
    variable        // 식별자
    2               // 상수
    ```

* **[문장](https://en.wikipedia.org/wiki/Statement_(computer_science))(statement)**
    
    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 러스트 프로그래밍 언어는 [세미콜론](https://ko.wikipedia.org/wiki/새줄_문자)(semicolon) `;`을 기준으로 문장을 분별한다. 

    ```c
    int variable = 2 + 3;      // 숫자 5를 "variable" 변수에 초기화
    if (2 < 3) statement;      // 논리가 참이면 "statement" 문장 실행
    ```

* **[블록](https://en.wikipedia.org/wiki/Block_(programming))(block)**

    한 개 이상의 문장들을 한꺼번에 관리할 수 있도록 묶어놓은 소스 코드상 그룹이다. 블록 안에 또 다른 블록이 상주할 수 있으며, 이를 네스티드 블록(nested block)이라고 부른다. C 언어에서는 한 쌍의 중괄호 `{}`로 표시된다.

    ```c
    {
        int variable = 2 + 3;
        if (2 < 3) statement;
    }
    ```

### 식별자
[식별자](https://learn.microsoft.com/en-us/cpp/c-language/c-identifiers)(identifier)는 프로그램을 구성하는 데이터들을 구별하기 위해 사용되는 명칭이다. 즉, 식별자는 개발자가 데이터에 직접 붙여준 이름이다. C 언어에서 식별자를 선정하는데 아래의 규칙을 지켜야 한다.

1. 알파벳, 숫자, 밑줄 `_`만 허용 (그 외 특수문자 및 공백 사용 불가)
2. 식별자의 첫 문자는 숫자가 될 수 없음
3. 대소문자 구분 필수
4. [예약어](https://ko.wikipedia.org/wiki/예약어) 금지

### 주석
[주석](https://learn.microsoft.com/en-us/cpp/c-language/c-comments)(comment)은 프로그램의 소스 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데 C 언어에는 한줄 주석 그리고 블록 주석이 존재한다.

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어 주석 종류</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">한줄 주석</th><th style="text-align: center;">블록 주석</th></tr></thead><tbody><tr><td colspan="2">주석은 컴파일 직전에 <a href="#전처리기">전처리기</a>에 의해 소스 코드에 제거된다. 즉, 실행 파일 안에는 주석의 어떠한 정보도 저장되지 않는다.</td></tr><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```c
// 한줄 주석: 코드 한 줄을 차지하는 주석이다.
```
</td><td>

```c
/*
블록 주석:
코드 여러 줄을 차지하는 주석이다.
*/
```
</td></tr>
</tbody>
</table>

## 자료형
[자료형](https://en.wikipedia.org/wiki/Data_type)(data type)은 데이터를 어떻게 표현할 지 결정하는 요소이며, C 언어에서는 다음과 같이 존재한다. 단, 본 문서는 ANSI C 언어를 기준으로 소개하므로, 이후 C99부터 소개된 일부 자료형(`bool`, `long long` 등)은 목록에 제외되었다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://learn.microsoft.com/en-us/cpp/c-language/storage-of-basic-types">C 언어 자료형</a> (ANSI C 기준)</caption><colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">키워드</th><th style="text-align: center;">자료형</th><th style="text-align: center;">크기 (바이트)</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>char</code></td><td style="text-align: center;">문자</td><td style="text-align: center;">1</td><td>단일 ANSI 문자</td></tr><tr><td style="text-align: center;"><code>short</code></td><td style="text-align: center;">정수</td><td style="text-align: center;">2</td><td>가장 작은 정수 자료형</td></tr><tr><td style="text-align: center;"><code>int</code></td><td style="text-align: center;">정수</td><td style="text-align: center;">2 <sub>(최소)</sub></td><td>워드 크기의 기본 정수 자료형; <code>short</code>보다 작아서는 안되며, 32비트 시스템 이후로는 4바이트가 일반화되었다.</td></tr><tr><td style="text-align: center;"><code>long</code></td><td style="text-align: center;">정수</td><td style="text-align: center;">4 <sub>(최소)</td><td>정수 자료형 <code>int</code>보다 작아서는 안되며, 4바이트와 8바이트 중 어느 크기를 채택하였는지 컴파일러마다 다르다.</td></tr><tr><td style="text-align: center;"><code>float</code></td><td style="text-align: center;">부동소수점</td><td style="text-align: center;">4</td><td>32비트 단정밀도 실수</td></tr><tr><td style="text-align: center;"><code>double</code></td><td style="text-align: center;">부동소수점</td><td style="text-align: center;">8</td><td>64비트 배정밀도 실수</td></tr><tr><td style="text-align: center;"><code>void</code></td><td style="text-align: center;">보이드</td><td style="text-align: center;">1</td><td>불특정 자료형</td></tr></tbody/></table>

> [바이트](https://ko.wikipedia.org/wiki/바이트)(byte)란, 컴퓨터에서 메모리에 저장하는 가장 기본적인 단위이다. 자료형마다 크기가 정해진 이유는 효율적인 메모리 관리 차원도 있으나 CPU 연산과도 깊은 연관성을 갖는다. 한 바이트는 여덟 개의 [비트](https://ko.wikipedia.org/wiki/비트_(단위))(bit)로 구성된다.

`unsigned` 키워드는 자료형 중에서 [최상위 비트](https://ko.wikipedia.org/wiki/최상위_비트)를 정수의 [부호](https://ko.wikipedia.org/wiki/Signed와_unsigned)를 결정하는 요소로 사용하지 않도록 한다. 아래의 16비트 정수형인 `short`는 원래 최상위 비트를 제외한 나머지 15개의 비트로 정수를 표현한다. `unsigned` 키워드를 사용하면 음의 정수를 나타낼 수 없지만, 16개의 비트로 양의 정수를 더 많이 표현할 수 있다.

```c
short             // 표현 가능 범위: -32768 ~ +32767
unsigned short    // 표현 가능 범위:     +0 ~ +65535
``` 

### 자료형 변환
자료형 변환(type casting)은 데이터를 다른 자료형으로 바꾸는 작업이며, 불가피하게 데이터가 손실될 수 있으므로 유의하도록 한다.

* **암시적 자료형 변환**(implicit type casting)

    코드에서 별도로 자료형을 명시하지 않아도 컴파일러에 의해 데이터가 자동적으로 적합한 자료형으로 변환되는 경우이다.

    ```c
    float num1 = 314.159;
    short num2 = num1;                   // num2 저장값: 314
    ```

* **명시적 자료형 변환**(explicit type casting)

    코드에서 소괄호 `()` 안에 자료형을 직접 기입하여 원하는 자료형으로 변환되는 경우이다.

    ```c
    float num1 = 314.159;
    short num2 = (unsigned char)num1;    // num2 저장값: 58
    ```

### `sizeof` 연산자
[`sizeof`](https://en.cppreference.com/w/c/language/sizeof) 연산자는 데이터나 자료형의 메모리에 할당된 바이트 크기를 반환한다.

```c
sizeof(int);      // 크기: 4바이트
sizeof(char);     // 크기: 1바이트
```

## 변수
변수(variable)는 데이터를 지정된 [자료형](#자료형)으로 저장하는 메모리 공간이다. 아래 코드는 `variable` [식별자](#식별자)를 정수형 변수로 "정의(definition)"하여 메모리 공간을 확보한 다음 상수 3을 할당한다. 여기서 변수로의 최초 할당을 "초기화(initialization)"라고 부르며, 변수를 정의한 이후에 별도로 이루어질 수 있다. 초기화가 누락되면 변수에 가공되지 않은 메모리가 연동되어 잠재적 위험을 초래할 수 있기 때문에 일반적으로 C 언어 컴파일러는 이를 오류로 치부한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption>C 언어의 변수 정의 및 초기화</caption><colgroup><col style="width: 50%;"/></col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">변수 정의 및 초기화 (일괄식)</th><th style="text-align: center;">변수 정의 및 초기화 (개별식)</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
int variable = 3;
```
</td><td>

```c
int variable;
variable = 3;
```
</td></tr></tbody></table>

동일한 자료형의 변수 여러 개를 한꺼번에 정의하려면, 식별자마다 쉼표 `,`로 구분지을 수 있다.

```c
// 다수의 정수 자료형 변수 정의
int variable1 = 3, variable2 = 4, variable3;
```

한 번 정의된 변수는 컴파일러 측에서 존재를 알고 있는 "선언(declaration)"된 상태로 이후 변수를 호출할 때 자료형을 언급하지 않는다. 선언된 변수는 메모리의 컴파일러에 존재성만 알린 것으로 확보된 메모리 공간이 부재하다. 다음은 변수에 특수한 성질을 부여하는 선언 키워드를 소개한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">변수 선언 키워드 및 특징</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">키워드</th><th style="text-align: center;">특징</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/constant_expression"><code>const</code></a></td><td>초기화된 이후로 변경이 불가한 상수(constant)로 지정한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/storage_duration#Static_local_variables"><code>static</code></a></td><td><a href="#함수">함수</a>를 탈출하여도 데이터가 소멸되지 않는 특수한 <a href="#지역-변수">지역 변수</a>, 일명 <a href="https://en.cppreference.com/w/cpp/language/storage_duration#Static_local_variables">정적 변수</a>이다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/language/extern"><code>extern</code></a></td><td>아직 정의되지 않은 변수 혹은 함수를 미리 호출할 수 있도록 선언만 하는 <a href="#외부-변수">외부 변수</a>이다.</td></tr></tbody></table>

C/C++ 언어 [ISO 표준](https://github.com/cplusplus/draft)의 § 6.2 Declarations and definitions 부문에 의하면 일반적인 변수의 선언은 정의와 동일하다고 간주한다.<sup>[<a href="https://learn.microsoft.com/en-us/cpp/cpp/declarations-and-definitions-cpp">참고</a>]</sup> 단, 다음은 변수가 선언되었으나 정의되지 않은 예외를 나열한다:

* 함수 전방선언
* 함수 매개변수 선언
* `extern` 키워드 선언
* `typedef` 선언

변수가 소스 코드 중에서 어디에 정의되었는지에 따라 지역 변수와 전역 변수로 구분된다. 

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption>C 언어의 지역 및 전역 변수</caption><colgroup><col style="width: 50%;"/></col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">지역 변수</th><th style="text-align: center;">전역 변수</th></tr></thead><tbody><tr><td>

```c
int main () {

    // 지역 변수
    int variable;

    return 0;
}
```
</td><td>

```c
// 전역 변수
int variable;

int main() {

    return 0;
}
```
</td></tr><tr><td>블록 내부에서 정의된 변수이다. 지역 변수에 저장된 데이터는 블록 밖에서는 소멸되므로 외부에서 사용할 수 없다.</td><td>어떠한 블록에도 속하지 않은 외부에 정의된 변수이다. 전역 변수는 어느 블록에서라도 호출하여 지역 변수와 함께 사용할 수 있다.</td></tr></tbody></table>

변수는 지정된 자료형 외의 데이터를 할당받을 수 있다. 아래 예시 코드는 문자 자료형 변수에 값 75로 초기화할 시, ASCII 코드에 의하여 대문자 'K'로 저장된다.

```c
char variable = 75;    // ASCII에 의해 문자 'K'가 저장
```

거의 모든 프로그래밍 언어는 할당 기호를 기준으로 왼쪽에는 피할당자(변수), 오른쪽에는 피할당자로 전달하려는 표현식(값 혹은 데이터)이 위치한다. 반대로 놓여질 경우, 오류가 발생하거나 원치 않는 결과가 도출될 수 있다.

## 연산자
[연산자](https://en.wikipedia.org/wiki/Operator_(computer_programming))(operator)는 피연산 데이터를 조작할 수 있는 가장 간단한 형태의 연산 요소이다. 연산자는 피연산자의 접두부, 접미부, 혹은 두 데이터 사이에 위치시켜 사용한다. 가독성을 위해 데이터와 연산자 사이에 공백을 넣어도 연산에는 아무런 영향을 주지 않는다. 다음은 [C/C++ 연산자](https://ko.wikipedia.org/wiki/C와_C++의_연산자)들을 간략히 소개한다.

### 산술 연산자
<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.cppreference.com/w/cpp/language/operator_arithmetic">산술 연산자</a>(arithmetic operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">산술</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>+</code></td><td style="text-align: center;">덧셈</td><td>좌측과 우측 피연산자의 값을 더하여 반환한다.</td></tr><tr><td style="text-align: center;"><code>-</code></td><td style="text-align: center;">뺄셈</td><td>좌측 피연산자에서 우측 피연산자를 뺀 값을 반환한다.</td></tr><tr><td style="text-align: center;"><code>*</code></td><td style="text-align: center;">덧셈</td><td>좌측 피연산자를 우측 피연산자의 값만큼 곱하여, 즉 반복 덧셈하여 반환한다.</td></tr><tr><td style="text-align: center;"><code>/</code></td><td style="text-align: center;">나눗셈</td><td>좌측 피연산자에서 우측 피연산자를 나눈 <a href="https://en.wikipedia.org/wiki/Quotient">몫</a>을 반환한다.</td></tr><tr><td style="text-align: center;"><code>%</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Modular_arithmetic">모듈러</a></td><td>좌측 피연산자에서 우측 피연산자를 나눈 <a href="https://en.wikipedia.org/wiki/Remainder">나머지</a>를 반환한다.</td></tr></tbody></table>

### 증감 연산자
[증가 연산자](https://en.cppreference.com/w/cpp/language/operator_incdec)(increment operator) `++` 및 [감소 연산자](https://en.cppreference.com/w/cpp/language/operator_incdec)(decrement operator) `--`는 데이터를 1만큼 증가 혹은 감소하는데 간략하게 한 줄로 표현한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">증감 연산자의 위치에 따른 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">접두부</th><th style="text-align: center;">접미부</th></tr></thead><tbody><tr><td>피연산자를 1만큼 증가/감소시킨 다음에 표현식을 평가한다.</td><td>표현식을 평가한 다음에 피연산자를 1만큼 증가/감소시킨다.</td></tr><tr><td>

```c
x = ++y;  // 동일: { y = y + 1; x = y; }
x = --y;  // 동일: { y = y - 1; x = y; }
```
</td><td>

```c
x = y++;  // 동일: { x = y; y = y + 1; }
x = y--;  // 동일: { x = y; y = y - 1; }
```
</td></tr></tbody></table>

### 비트 연산자
<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.wikipedia.org/wiki/Bitwise_operations_in_C">비트 연산자</a>(bitwise operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">비트연산</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>&</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#AND">AND</a></td><td>두 피연산자의 각 비트를 비교하여 모두 1이면 1을, 아니면 0을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><code>|</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#OR">OR</a></td><td>두 피연산자의 각 비트를 비교하여 하나라도 1이 있으면 1을, 아니면 0을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><code>^</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#XOR">XOR</a></td><td>두 피연산자의 각 비트를 비교하여 값이 같으면 0을, 다르면 1을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><code>~</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#NOT">NOT</a></td><td>피연산자의 각 비트마다 반전시킨 값을 반환한다.</td></tr><tr><td style="text-align: center;"><code>&lt;&lt;</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operations_in_C#Left_shift_%3C%3C">좌향 시프트</a></td><td>피연산자(左)의 비트를 전반적으로 일정 값(右)만큼 왼쪽으로 이동시킨다.</td></tr><tr><td style="text-align: center;"><code>&gt;&gt;</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operations_in_C#Right_shift_%3E%3E">우향 시프트</a></td><td>피연산자(左)의 비트를 전반적으로 일정 값(右)만큼 오른쪽으로 이동시킨다.</td></tr></tbody></table>

### 할당 연산자
단순 할당 연산자를 산술 및 비트 연산자와 조합하여 코드를 더욱 간결하게 작성할 수 있으며, 아래는 다양한 할당 연산자 중 일부만 보여준다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.cppreference.com/w/cpp/language/operator_assignment">할당 연산자</a>(assignment operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">할당</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>=</code></td><td style="text-align: center;">단순 할당</td><td>피연산자(右)가 <a href="#변수">변수</a>와 같은 피할당자(左)로 할당된 값을 반환한다.</td></tr><tr><td style="text-align: center;"><code>+=</code></td><td style="text-align: center;">덧셈 대입</td><td>

```c
x += y;  // 동일: x = x + y;
```
</td></tr><tr><td style="text-align: center;"><code>*=</code></td><td style="text-align: center;">곱셈 대입</td><td>

```c
x *= y;  // 동일: x = x * y;
```
</td></tr><tr><td style="text-align: center;"><code>&=</code></td><td style="text-align: center;">AND 대입</td><td>

```c
x &= y;  // 동일: x = x & y;
```
</td></tr><tr><td style="text-align: center;"><code>&lt;&lt;=</code></td><td style="text-align: center;">좌향 시프트 대입</td><td>

```c
x <<= y;  // 동일: x = x << y;
```
</td></tr></tbody></table>

### 비교 연산자
아래 비교 연산자의 설명은 참을 반환할 조건을 소개하며, 그 외에는 모두 0을 반환한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.cppreference.com/w/cpp/language/operator_comparison">비교 연산자</a>(relational operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">관계</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>&gt;</code></td><td style="text-align: center;">초과</td><td>좌측 피연산자가 우측 피연산자보다 크면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>&lt;</code></td><td style="text-align: center;">미만</td><td>좌측 피연산자가 우측 피연산자보다 작으면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>&gt;=</code></td><td style="text-align: center;">이상</td><td>좌측 피연산자가 우측 피연산자보다 크거나 같으면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>&lt;=</code></td><td style="text-align: center;">이하</td><td>좌측 피연산자가 우측 피연산자보다 작거나 같으면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>==</code></td><td style="text-align: center;">동일</td><td>두 피연산자의 값이 같으면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>!=</code></td><td style="text-align: center;">상이</td><td>두 피연산자의 값이 같지 않으면 1을 반환한다.</td></tr></tbody></table>

### 논리 연산자
(논리 부정을 제외한) 아래 논리 연산자의 설명은 참을 반환할 조건을 소개하며, 그 외에는 모두 0을 반환한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.cppreference.com/w/cpp/language/operator_logical">논리 연산자</a>(logical operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/><col style="width: "/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">논리</th><th style="text-align: center;">설명 </th></tr></thead><tbody><tr><td style="text-align: center;"><code>&&</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Logical_conjunction">논리곱</a></td><td>좌측 그리고 우측 <a href="https://en.wikipedia.org/wiki/Proposition">명제</a>(피연산자)가 모두 참이면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>||</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Logical_disjunction">논리합</a></td><td>좌측 또는 우측 명제(피연산자)가 하나라도 참이면 1을 반환한다.</td></tr><tr><td style="text-align: center;"><code>!</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Negation">부정</a></td><td>명제(피연산자)가 참이면 거짓으로, 혹은 그 반대로 반전된 값을 반환한다.</td></tr></tbody></table>

## 탈출 문자
[탈출 문자](https://en.wikipedia.org/wiki/Escape_character)(escape character)는 백슬래시 기호 `\`를 사용하며, [문자열](#문자열)로부터 탈출하여 텍스트 데이터 내에서 특정 연산을 수행하도록 한다. 예시에서 `\n` 탈출 문자를 사용하여 문자열 줄바꿈을 구현한 것을 보여주었다.

```c
printf("Hello,\nWorld!");
```
```terminal
Hello,
World!
```

# 파일 입출력
C 언어의 [파일 입출력](https://en.wikipedia.org/wiki/C_file_input/output)(일명 I/O)은 [`stdio.h`](https://en.cppreference.com/w/cpp/header/cstdio) 헤더로부터 관련 함수들을 호출할 수 있으며, 단순 파일뿐만 아니라 터미널로부터 텍스트를 입력받거나 출력할 때에도 관여한다. C 언어의 파일 입출력은 다른 프로그래밍 언어에 비해 신경써야 할 부분이 많아 별도의 장을 마련하여 소개한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어의 파일 출력 함수</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">출력 함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/io/putchar"><code>putchar</code></a></td><td>문자(character) 하나를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/io/puts"><code>puts</code></a></td><td>일련의 문자들, 일명 <a href="#문자열">문자열</a>을 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/io/c/fprintf"><code>printf</code></a></td><td><a href="#형식-지정자">형식 지정자</a>에 따라 데이터를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/io/c/fprintf"><code>fprintf</code></a></td><td><a href="https://en.wikipedia.org/wiki/Stream_(computing)">스트림</a>을 지정할 수 있는 출력 함수이다; <code>printf</code>는 "표준 출력" 스트림의 <code>fprintf(<a href="https://en.wikipedia.org/wiki/Standard_streams#Standard_output_(stdout)">stdout</a>)</code>과 동일하다.

```c
fprintf(stdout, "Number: %f", 3.14159);
// Number: 3.141590
```
</td></tr></tbody></table>

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어의 파일 입력 함수</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">입력 함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/io/getchar"><code>getchar</code></a></td><td>터미널로부터 문자(character) 하나를 입력받아 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/io/gets"><code>gets</code></a></td><td>터미널로부터 입력받은 텍스트를 <a href="#문자열">문자열</a> 형태로 인자에 전달한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/io/c/fscanf"><code>scanf</code></a></td><td>터미널로부터 입력받은 텍스트를 <a href="#형식-지정자">형식 지정자</a>에 따른 자료형으로 변환하여 인자에 전달한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/io/c/fscanf"><code>fscanf</code></a></td><td><a href="https://en.wikipedia.org/wiki/Stream_(computing)">스트림</a>을 지정할 수 있는 입력 함수이다; <code>scanf</code>는 "표준 입력" 스트림의 <code>fscanf(<a href="https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)">stdin</a>)</code>과 동일하다.

```c
char variable[32];
fscanf(stdin, "%s", variable);
```
</td></tr></tbody></table>

> [스트림](https://ko.wikipedia.org/wiki/스트림_(컴퓨팅))(stream)이란 사전적 의미로 "물이 흐르는 개울"을 의미한다. 즉, 컴퓨터 통신 용어에서 스트림은 데이터가 흐르는 길을 의미한다.

여기서 `fscanf()` 입력 함수는 입력된 텍스트를 빈칸(띄어쓰기, [줄바꿈](https://ko.wikipedia.org/wiki/새줄_문자) 등) 및 형식 지정자가 수용할 수 있는 문자 개수를 기준으로 데이터를 나누어 변수에 전달한다. 만일 전달받을 변수의 개수가 입력보다 적을 시, 남은 입력은 다음 입력 함수에서 변수로 전달될 때까지 스트림 [버퍼](https://ko.wikipedia.org/wiki/버퍼_(컴퓨터_과학))에 잔여한다.

## 형식 지정자
형식 지정자(format specifier)는 입출력 함수가 데이터를 어떻게 받아들일 것인지 결정한다. 이는 자료형 이외에도 [출력 함수](https://learn.microsoft.com/en-us/cpp/c-runtime-library/format-specification-syntax-printf-and-wprintf-functions)의 경우에는 화면에 어떻게 표시할 지, 그리고 [입력 함수](https://learn.microsoft.com/en-us/cpp/c-runtime-library/format-specification-fields-scanf-and-wscanf-functions)의 경우에는 입력된 데이터를 어떻게 수용할 지 등에 대하여 서식을 지정한다. 본 부문은 형식 지정자에 대한 개략적인 예시만을 소개한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">입출력 함수의 형식 지정자</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">입력 함수</th><th style="text-align: center;">출력 함수</th></tr></thead><tbody><tr><td>본래 데이터의 성질이나 값이 변한다.</td><td>본래 데이터는 그대로 유지되나, 어떻게 표시되는지만 달라진다.</td></tr><tr><td>

```c
int variable = 0;
scanf("%c", &variable);
```
</td><td>

```c
int variable = 51;
printf("%c", variable);
```
</td></tr><tr><td>

```terminal
INPUT: 3
variable = 51 (char '3' equals to ASCII code 51)
```
</td><td>

```terminal
OUTPUT: 3
variable = 51 (unchanged)
```
</td></tr></tbody></table>

## 파일 관리
C 언어에서 파일을 열고 닫기 위해서는 각각 `fopen()` 및 `fclose()` 함수를 사용한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">파일 입출력 및 모드 옵션</caption><colgroup><col style="width: 50%;"/><col style="width: 10%;"/><col style="width: 25%;"/><col style="width: 15%;"/></colgroup><thead><tr><th style="text-align: center;">파일 관리 코드</th><th colspan="2" style="text-align: center;">파일 열기 옵션: <code>mode</code></th><th style="text-align: center;">부재시 생성 여부</th></tr></thead><tbody><tr><td rowspan="6">

```c
FILE* fptr = fopen("filename.txt", mode);

    ...

fclose(fptr);
```
</td><td style="text-align: center;"><code>"r"</code></td><td>읽기 모드</td><td style="text-align: center;">❌</td></tr><tr><td style="text-align: center;"><code>"w"</code></td><td>덮어쓰기 모드</td><td style="text-align: center;">⭕</td></tr><tr><td style="text-align: center;"><code>"a"</code></td><td>덧붙이기 모드</td><td style="text-align: center;">⭕</td></tr><tr><td style="text-align: center;"><code>"r+"</code></td><td>읽기 + 쓰기 모드</td><td style="text-align: center;">❌</td></tr><tr><td style="text-align: center;"><code>"w+"</code></td><td>읽기 + 덮어쓰기 모드</td><td style="text-align: center;">⭕</td></tr><tr><td style="text-align: center;"><code>"a+"</code></td><td>읽기 + 덧붙이기 모드</td><td style="text-align: center;">⭕</td></tr></tbody></table>

[`fopen()`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/fopen-wfopen) 함수는 파일을 성공적으로 열었을 때 파일 스트림 객체를 가리키는 `FILE` [자료형](#구조체) [포인터](#포인터)를 반환한다. 해당 포인터를 `fprintf()` 및 `fscanf()` 입출력 함수의 스트림으로 전달하면 파일을 작성하거나 읽어올 수 있다.

더 이상 사용하지 않는 파일은 `fclose()` 함수로 닫아 리소스 낭비를 줄이는데 기여할 수 있다. [예외](#예외-처리)가 발생하여도 정상적으로 파일을 닫을 수 있도록 예외 처리문 혹은 `EOF`(End-of-File)를 활용한 조건문을 사용할 것을 권장한다.

> [EOF](https://ko.wikipedia.org/wiki/파일_끝)란, End-of-File의 약자로 파일의 끝에 도달하였으면 트리거되는 데이터이다.

# 제어문
제어문(control statement)은 코드 실행을 제어하는 문장을 가리키며, 프로그래밍에 있어 기초적이면서 가장 흔히 사용되는 코드 유형 중 하나이다. 제어문을 크게 세 분류로 나누면 [조건문](#조건문), [반복문](#반복문), 그리고 [이동문](#이동문)이 존재한다.

## 조건문
조건문(conditional statement)은 주어진 조건의 논리에 따라서 코드 실행 여부를 결정하는 제어문이다:

### `if` 조건문
[`if`](https://en.cppreference.com/w/c/language/if) 조건문은 조건 혹은 논리가 참일 경우 코드를 실행하며, 거짓일 경우에는 코드를 실행하지 않는다.

```c
if (condition) {
    statements;
}

// 간략화된 문장
if (condition) statement;
```

* **`else` 조건문**

    단독으로 사용될 수 없으며 반드시 `if` 조건문 이후에 사용되어야 한다. 조건부가 거짓으로 판정되면 실행할 코드를 포함한다.

    ```c
    if (condition) {
        statements;
    }
    else {
        statements; 
    }
    ```

* **`else if` 조건문**

    `else`와 `if` 조건문의 조합으로 이전 조건이 거짓일 때 새로운 조건을 제시한다.

    ```c
    if (condition) {
        statements;
    }
    else if (condition) {
        statements;
    }
    else {
        statements;
    }
    ```

### 조건 연산자
[조건 연산자](https://en.cppreference.com/w/c/language/operator_other#Conditional_operator)(ternary operator) `?:`는 세 가지 인수만을 사용하여 조건문을 아래와 같이 간략하게 표현한다. 조건 연산자는 가독성을 감소시키므로 과용해서는 안되지만 변수 할당에 유용하다.

```c
condition ? true_return : false_return;
```

### `switch` 조건문
[`switch`](https://en.cppreference.com/w/c/language/switch) 조건문은 전달받은 인자를 `case`의 상수와 동일한지 비교하여 논리가 참일 경우 해당 지점부터 코드를 실행하며, 거짓일 경우에는 다음 `case`로 넘어간다. 선택사항으로 `default` 키워드를 통해 어떠한 `case` 조건에도 부합하지 않으면 실행될 지점을 지정한다.

```c
switch (argument) {
case value1:
    statements;
    break;

case value2:
    statements;
    break;

case value3:
    statements;
    break;

default:
    statements;
}
```

`switch` 조건문이 어느 `case` 코드를 실행할지 결정하는 것이라고 쉽사리 착각할 수 있으나, 이는 사실상 [`break`](#break-문) 탈출문 덕분이다. 탈출문이 없었더라면 아래 예시 코드처럼 해당 조건의 `case` 코드 실행을 마쳤어도 다음 `case` 코드로 계속 진행하는 걸 확인할 수 있다. 즉, `case` 키워드는 코드 실행 영역을 분별하는 것이 아니라 진입 포인트 역할을 한다.

```c
int variable = 2;

// switch 조건문의 동작 예시
switch (variable) {
case 1:
    printf("Statement 1\n");

case 2:
    printf("Statement 2\n");

case 3:
    printf("Statement 3\n");
 
default:
    printf("Statement 4\n");
}
```
```terminal
Statement 2
Statement 3
Statement 4
```

## 반복문
반복문(loop statement)은 주어진 조건의 논리에 따라서 코드를 얼마나 반복적으로 실행할 지 결정하는 제어문이다:

### `while` 반복문
[`while`](https://en.cppreference.com/w/c/language/while) 반복문은 조건 혹은 논리가 참일 동안 코드를 반복적으로 실행하며, 거짓일 경우에는 반복문을 종료한다.

```c
while (condition) {
    statements;
}

// 간략화된 문장
while (condition) statement;
```

* **[`do`](https://en.cppreference.com/w/c/language/do) 반복문**

    코드를 우선 실행하고 조건 혹은 논리가 참일 경우 코드를 반복하며, 거짓일 경우에는 반복문을 종료한다.

    ```c
    do {
        statements;
    } while (condition);
    ```

### `for` 반복문
[`for`](https://en.cppreference.com/w/c/language/for) 반복문은 조건 혹은 논리가 참일 동안 코드를 반복적으로 실행하며, 거짓일 경우에는 반복문을 종료한다. `for` 반복문은 조건 평가 외에도 지역 변수를 초기화 및 증감할 수 있는 인자가 있다.

```c
for (initialize; condition; increment) {
    statements;
}

// 간략화된 문장
for (initialize; condition; increment) statement;
```

`for` 반복문의 우선 `initialize`에서 반복문 지역 변수를 정의하거나 외부 변수를 불러와 반복문을 위한 초기값을 할당한 다음 `condition`에서 조건을 평가한다. 논리가 참이면 코드를 반복적으로 실행하며, 거짓일 경우에는 반복문을 종료한다. 블록 내의 코드가 마무리되었거나 `continue` 문을 마주하면 `increment`에서 변수를 증감하고, `condition`으로 돌아가 절차를 반복한다.

## 이동문
이동문(jump statement)은 아무런 조건이 필요없이 코드 실행 지점을 이동시키는 제어문이다:

### `break` 탈출문
[`break`](https://en.cppreference.com/w/cpp/language/break) 탈출문은 (1) 반복문을 조기 종료시키거나, (2) `switch` 조건문에서 경우에 따라 실행되어야 할 코드를 구분짓기 위해 사용된다.

### `continue` 연속문
[`continue`](https://en.cppreference.com/w/cpp/language/continue) 연속문은 반복문을 종료하지 않은 채 나머지 실행 코드를 전부 무시하고 반복문의 조건부로 되돌아간다.

### `goto` 이동문
[`goto`](https://en.cppreference.com/w/c/language/goto) 이동문은 다른 문장으로써는 절대로 접근이 불가한 코드에 도달할 수 있도록 한다 (일명 제어 전달; control transfer). `goto` 키워드에 명시된 [레이블](https://en.cppreference.com/w/cpp/language/statements#Labels)로 제어를 전달하나, 이 둘은 반드시 동일한 [함수](#함수) 내에 위치해야 한다. 레이블은 `goto` 문 이전이나 이후에 위치하여도 무관하다.

```c
int main() {
    
    // 제어 전달: "label"로 이동
    goto label;    

    // "label" 레이블
label:
    statements;

}
```

단, `goto` 이동문을 사용할 때에는 매우 조심해야 하며 무리한 남용은 [스파게티 코드](https://ko.wikipedia.org/wiki/스파게티_코드)의 원인이 된다.

### `return` 반환문
[`return`](https://en.cppreference.com/w/cpp/language/return) 반환문은 [함수](#함수)를 종료하면서 지정된 자료형으로 데이터를 반환한다. 하단에 코드가 남아 있음에도 불구하고 반환문이 실행되면 함수는 즉시 종료된다.

```c
// return 반환문이 있는 사용자 정의 함수
int function() {
    printf("Hello World!\n");
    return 1 + 2;
}
    
printf("%d\n", function());
```
```terminal
Hello World!
3
```

# 배열
[배열](https://en.cppreference.com/w/cpp/language/array)(array)은 동일한 자료형의 데이터를 일련의 순서로 담는 저장공간이다. 식별자 뒤에는 대괄호 `[]`가 위치하여 배열이 담을 수 있는 데이터 용량 크기를 [정수 리터럴](https://en.cppreference.com/w/cpp/language/integer_literal)이나 [상수](#변수)로 지정한다. 배열의 데이터 초기화는 중괄호 `{}` 내에 항목을 순서대로 쉼표로 나누어 나열한다. 만일 배열 용량을 지정하지 않으면 데이터 개수만큼 크기가 정해지며, 아래는 배열을 정의하는 두 방식을 보여준다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">배열 크기를 지정하는 여부에 따른 정의 방식</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">명시적 배열 크기</th><th style="text-align: center;">암묵적 배열 크기</th></tr></thead><tbody><tr><td>

```c
int arr[size] = {value1, value2, ... };
```
</td><td>

```c
int arr[] = {value1, value2, ... };
```
</td></tr></tbody></table>

한 번 정의된 배열의 크기는 변경이 불가하다. 초기 데이터 개수는 배열 용량을 초과해서는 안되지만, 반면에 데이터 개수가 용량을 미치지 못하면 나머지는 `0` 혹은 `NULL`로 초기화된다.

배열의 각 요소에 할당된 데이터는 대괄호 `[]`를 사용해 0번부터 시작하는 인덱스 위치를 호출할 수 있다. 그러나 배열 자체를 호출하면 컴퓨터 메모리에 배열이 저장된 주소가 반환된다. 배열의 메모리 주소는 첫 번째 요소(즉, 인덱스 0번)의 주소와 일치하는데, 그 다음 주소에는 다음 인덱스 요소가 연쇄적으로 할당되어 있다. 자세한 내용은 [포인터](#포인터)를 다룰 때 소개한다.

```c
int arr[3] = {value1, value2, value3};

printf("%p\n", arr);        // 출력: 00D2FBA8
printf("%p\n", &arr[0]);    // 출력: 00D2FBA8
printf("%p\n", &arr[1]);    // 출력: 00D2FBAC (= 00D2FBA8 + 정수형 4바이트)
```

이러한 배열의 특징으로 인해 배열은 정의 외에 한꺼번에 할당이 불가능하다. 그렇지만 개별 요소를 재할당하여 데이터를 변경할 수 있다.

```c
int arr[3];

// 배열의 개별 요소 할당
arr[0] = value1;
arr[1] = value2;
arr[2] = value3;
```

### 다차원 배열
배열은 또 다른 배열을 요소로 가질 수 있으나, 자료형이 동일해야 하며 요소로 작용하는 배열들의 크기는 모두 같아야 하는 제약을 갖는다. 다차원 배열도 첫 번째 차원의 크기를 별도로 명시하지 않아도 되지만, 나머지 차원의 크기는 반드시 지정해야 한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">다차원 배열의 1차원 크기를 지정하는 여부에 따른 정의 방식</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">명시적 배열 크기</th><th style="text-align: center;">암묵적 배열 크기</th></tr></thead><tbody><tr><td>

```c
int arr[size1][size2] = {
    { value11, value12, ... },
    { value11, value12, ... },
    ...
};
```
</td><td>

```c
int arr[][size2] = {
    { value11, value12, ... },
    { value11, value12, ... },
    ...
};
```
</td></tr></tbody></table>

### 배열의 크기
[`sizeof`](#sizeof-연산자) 연산자가 배열에 사용되면 배열의 크기가 아닌, 배열이 차지하는 총 바이트 수를 반환한다. 배열의 각 요소마다 자료형만큼 메모리를 차지하므로 배열의 크기를 구하기 위해서는 다음과 같은 표현식을 사용한다. 자료형의 요소들로 구성된 배열을 해당 자료형으로 나누면 요소의 개수가 계산된다.

```c
int arr[3];
printf("%d", sizeof(arr)/sizeof(int));    // 출력: 3 (= 배열의 크기)
```

## 문자열
C 언어는 일련의 문자들, 일명 [문자열](https://en.cppreference.com/w/cpp/string/byte)(string)을 한 개 이상의 `char` 문자들과 널 문자 `\0`로 구성된 배열로 문자열을 표현한다.

```c
// C 형식 문자열
char arr[] = "Hello";    // 즉, arr[] = {'H', 'e', 'l', 'l', 'o', '\0'};
char* ptr = "World!";    // 포인터를 활용한 문자열 표현 방법
```

[`string.h`](https://en.cppreference.com/w/cpp/header/string) 헤더 파일은 아래와 같이 C 표준 라이브러리로부터 제공하는 문자열 관련 함수들을 제공한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어의 문자열 함수</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/string/byte/strcat"><code>strcat</code></a></td><td>배열의 문자열에 다른 배열의 문자열을 덧붙인다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/string/byte/strcpy"><code>strcpy</code></a></td><td>배열의 문자열을 다른 배열로 복사한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/string/byte/strlen"><code>strlen</code></a></td><td>널 문자를 제외한 문자열 길이를 반환한다.</td></tr></tbody></table>

# 함수
[함수](https://learn.microsoft.com/en-us/cpp/c-language/overview-of-functions)(function)는 독립적인 코드 블록으로써 데이터를 처리하며, 재사용이 가능하고 호출 시 처리된 데이터를 보여주어 유동적인 프로그램 코딩을 가능하게 한다. 함수는 이름 뒤에 소괄호가 있는 `function()` 형식으로 구별된다.

```c
int variable[4] = {0, 3, 5, 9};
printf("%d", sizeof(variable));
// 터미널에 텍스트를 출력하는 "printf()" 함수
```
```
16
```

함수를 정의하기 위해서 (1) 여러 문장의 코드들을 하나로 묶는 [블록](#구문)과 (2) [`return`](#return-반환문) 키워드에 의해 반환될 데이터 유형을 결정하는 [자료형](#자료형)이 반드시 필요하다. 함수 안에 새로운 함수를 정의하는 건 허용되지 않는다. 정의된 함수를 호출하여 사용하는 데, 함수명 뒤에 소괄호 `()` 기입 여부에 따라 의미하는 바가 다르다:

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">함수 식별자의 호출 방식에 따른 차이</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><code>function()</code> 호출</th><th style="text-align: center;"><code>function</code> 호출</th></tr></thead><tbody><tr><td>함수에 정의된 코드를 실행한다.</td><td>함수의 <a href="#포인터">메모리 주소</a>를 가리키며, 정의된 코드를 실행하지 않는다.</td></tr><tr style="vertical-align: top;"><td>

```c
int function() {
    printf("%d\n", 1 + 2);
    return 7;
}

int variable = function();
printf("반환: %p", variable);
```
</td><td>

```c
int function() {
    printf("%d\n", 1 + 2);
    return 7;
}

int variable = function;
printf("반환: %p", variable);
```
</td></tr><tr style="vertical-align: top;"><td>

```terminal
3
반환: 0000000000000007
```
</td><td>

```terminal
반환: 00000000249513ED
```
</td></tr></tbody></table>

함수가 정의하기도 전에 호출되면 순차적으로 실행되는 C 언어 특성상 존재하지 않는 함수를 호출하는 것으로 간주되어 오류가 발생한다. 함수 [프로토타입](https://en.cppreference.com/w/c/language/function_declaration)(prototype), 일명 전방선언(forward declaration)은 컴파일러에게 미리 함수의 존재를 알려주어 정의되기 전에 호출할 수 있다. 프로토타입은 선택사항이며, 우선적으로 선언될 수 있게 스크립트 상단부에 기입하는 게 일반적이다.

```c
// 함수 프로토타입
void function();

// 함수 호출
function();

// 함수 정의
void function() {
    printf("%d", 1 + 2);
}
```

다음은 함수에 대해 논의할 때 중요하게 언급되는 매개변수와 전달인자의 차이에 대하여 설명한다.

* **전달인자 (argument)**: 간략하게 "인자"라고도 부르며, 함수로 전달되는 데이터이다.
* **매개변수 (parameter)**: 전달인자를 할당받는 함수 내의 지역 변수이다. 그러므로 매개변수는 함수 외부에서 호출이 불가능하다. 매개변수 선언은 함수의 소괄호 `()` 내에서 이루어진다.

> 매개변수와 전달인자는 개념적으로 다른 존재이지만, 동일한 데이터를 가지고 있는 관계로 흔히 두 용어는 혼용되어 사용하는 경우가 많다.

아래의 예제는 함수의 매개변수와 전달인자가 어떻게 동작하는지 보여준다.

```c
int function(int arg1, float arg2);

function(1, 3.14);
// 반환: 4 (= 1 + 3.14의 정수형만 추출)

int function(int arg1, float arg2) {
    return arg1 + arg2;
}
```

배열은 위와 동일한 구문으로 인자를 매개변수로 건네줄 수 없으며, 아래의 두 가지 방법이 존재한다:

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">배열을 매개변수로 전달하는 방법</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="#배열">배열</a>로 선언</th><th style="text-align: center;"><a href="#포인터">포인터</a>로 선언</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
void function(int arg[]);

int arr[3] = {value1, value2, value3};
function(arr);

// 넘겨받은 인자를 배열 그대로 받아들인다.
void function(int arg[]) {
    statements;
    return;
}
```
</td><td>

```c
void function(int *arg);

int arr[3] = {value1, value2, value3};
function(arr);

// 넘겨받은 인자를 배열이 아닌 포인터로 받아들인다.
void function(int *arg) {
    statements;
    return;
}
```
</td></tr></tbody></table>

배열 자체를 호출하면 배열의 첫 번째 요소의 메모리 주소를 가져오기 때문에 가능하다. 특히 배열의 각 요소가 할당된 메모리 주소는 연쇄적이므로, 바로 옆 (`int` 정수형이면 +4) 메모리 주소에는 배열의 다음 요소가 저장된 메모리 공간이다.

## 진입점
[진입점](https://ko.wikipedia.org/wiki/엔트리_포인트)(entry point)는 프로그램이 시작되는 부분을 의미하며, C 언어의 경우 [`main()`](https://en.cppreference.com/w/cpp/language/main_function) 함수에서부터 코드가 실행된다. 진입점은 프로토타입이 존재하지 않으며, 유일해야 하므로 복수의 `main()` 함수가 존재하거나 찾지 못하면 요류가 발생하여 컴파일이 불가하다.

```c
// C 언어 진입점: main()
int main(int argc, char **argv) {

    return 0;
}
```

> 본 문서의 대부분 예시에는 `main()` 함수가 직접 언급되지 않았으나, 전역 변수와 함수 등을 제외한 코드들은 `main()` 함수 내에서 작성되어야만 실행된다.

C/C++ 언어 표준에 의하면 `main()` 함수는 반드시 `int` 정수형을 반환해야 하며, `EXIT_SUCCESS`(혹은 정수 `0`) 그리고 `EXIT_FAILURE`이 있다. 만일 반환문이 없을 시, 컴파일러는 자동적으로 `return 0;` 문장을 `main()` 함수의 말단에 삽입한다.

`main()` 진입점은 아래와 같은 매개변수를 함축적으로 가진다.

* **`argc`**: 전달인자 개수(argument count).
* **`argv`**: 전달인자 데이터 배열(argument vector); 매개변수 정의는 `char *argv[]`로 대체 가능하다.

## 콜백 함수
[콜백 함수](https://ko.wikipedia.org/wiki/콜백)(callback function)는 인자로 전달되는 함수이다. 여기서 콜백이란, 전달인자로 전달된 함수가 다른 함수에서 언젠가 다시 호출(call back)되어 실행된다는 의미에서 붙여진 용어이다. 콜백 함수를 전달받는 함수, 일명 호출 함수(calling function)는 블록 내에서 매개변수 호출을 통해 콜백 함수를 실행한다.

아래는 콜백 함수의 예시이며, 이에 대한 자세한 원리는 차후 [함수 포인터](#함수-포인터)에서 설명한다.

```c
// 호출 함수
float calling(float (*function)(int, float), int arg) {
    // 콜백 함수의 호출
    return function(arg, 3.14159);
}

// 콜백 함수
float callback(int arg1, float arg2) {
    return (float)arg1 + arg2;
}

printf("%f", calling(callback, 1));
```
```
4.141590
```

## 인라인 함수
[인라인 함수](https://en.cppreference.com/w/c/language/inline)(inline function)는 인라인 확장에 사용될 `inline` 키워드로 지정된 함수이다.

> [인라인 확장](https://ko.wikipedia.org/wiki/인라인_확장)(inline expansion)은 컴파일 과정에서 함수 [호출지](https://en.wikipedia.org/wiki/Call_site)(call site)를 함수 코드로 치환하는 최적화 기법이다. 

프로그램 실행 (즉, 런타임) 도중에 함수를 호출하는데 소모되는 시간이 없으므로 속도가 소폭 향상되는 효과가 있으나, 과도한 사용은 프로그램 크기가 커지고 RAM 메모리를 더 많이 사용하는 단점으로 작용한다. 그러므로 인라인은 코드가 작지만 자주 사용되는 함수에 가장 적합하다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">인라인 함수를 활용한 코드 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">인라인 함수가 적용된 코드</th><th style="text-align: center;">동일 코드</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
// 인라인 함수
inline void function(char* arg) {
    printf("%s", arg);
}

int main() {
    function("Hello World!");
    return 0;
}
```
</td><td>

```c
int main() {
    printf("%s", "Hello World!");
    return 0;
}
```
</td></tr></tbody></table>

## 재귀 함수
[재귀 함수](https://ko.wikipedia.org/wiki/재귀_(컴퓨터_과학))(recursive function)는 스스로를 호출하는 함수이다. 재귀 함수는 반드시 스스로를 호출하는 반복으로부터 탈출하는 기저 조건(base case)이 필요하다. 기저 조건이 없으면 무한 재귀가 발생하는데 프로그램 실행에 기여하는 [메모리](#스택-영역)가 부족하여 충돌이 발생한다.

```c
// 예제: 펙토리얼 "!"
int factorial(int arg) {
    // 기저 조건: 재귀로부터 탈출하는 조건
    if (arg == 1)
        return 1;
    else
        return arg * factorial(arg - 1);
}
```

# 포인터
[포인터](https://en.cppreference.com/w/cpp/language/pointer)(pointer)는 정의된 데이터나 코드가 할당받은 [메모리](Memory.md)를 가리키는(가리키다; point) [변수](#변수) 혹은 주소(address)이다. 포인터가 가리키는 메모리 주소 안에는 해당 데이터나 코드가 저장되어 있는데, 이러한 메모리 주소를 통해 접근이 가능한 특징이 C 언어의 핵심이자 많은 코딩 입문자들을 기피하게 만든다. 포인터에 대한 이해를 위해 컴퓨터 구조, 특히 메모리와 관련된 개념이 함께 설명될 필요가 있다.

포인터를 선언할 때에는 변수와 마찬가지로 [자료형](#자료형)이 명시되어야 하지만, 자료형과 식별자 사이에 별표 `*`(영문: [asterisk](https://en.wikipedia.org/wiki/Asterisk))를 기입하여 포인터임을 알린다.

```c
// int 자료형의 포인터 정의
int *ptr = &variable;
```

위의 예시는 [참조 연산자](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_address-of_operator) `&`(영문: [ampersand](https://en.wikipedia.org/wiki/Ampersand))를 통해 변수 `variable`가 저장된 메모리 주소를 정수형 자료형의 포인터 `ptr`에 알려주는 문장이다. 단, 포인터가 가리킬 수 있는 메모리 주소는 lvalue에 속하는 값 유형에만 해당한다.

* **[lvalue](https://learn.microsoft.com/en-us/cpp/c-language/l-value-and-r-value-expressions)**: 접근 가능한 메모리 주소를 할당받은 데이터로 변수, [함수](#함수) 등이 해당한다.
* **[rvalue](https://learn.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp)**
    * *prvalue*: 접근 가능한 메모리 주소를 할당받지 아니한 데이터로 정수 및 문자열 [리터럴](https://ko.wikipedia.org/wiki/리터럴) 등이 해당한다.
    * *xrvalue*: 메모리 주소를 할당받았지만 더 이상 접근이 불가한 데이터이다.

> lvalue와 rvalue는 각각 할당 기호 `=`의 좌측(left)과 우측(right)에 위치한, 즉 데이터를 저장하는 피할당자와 값을 전달하는 표현식 관계이다.

32비트와 64비트 프로그램은 메모리 주소를 각각 4바이트(8자리의 십육진수)와 8바이트(16자리의 십육진수)로 표현한다. 그렇지만 메모리 주소는 개발자가 직접 수기로 작성하면 절대 안되며, 이는 오히려 [NTSTATUS](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) [0xC0000005](https://learn.microsoft.com/en-us/shows/inside/c0000005) STATUS_ACCESS_VIOLATION이란 유효하지 않은 메모리 주소 접근 오류를 유발한다.

메모리 주소에는 오로지 한 바이트의 데이터만 저장할 수 있다. 예를 들어, `int` 정수를 표현하려면 4바이트가 필요하므로 이웃하는 네 개의 메모리 주소가 하나의 데이터를 저장하는데 관여한다. 포인터의 자료형은 이러한 특성을 고려하여 가리키고 있는 메모리 주소로부터 어디까지 참조해야 완전한 데이터를 표현할 수 있는지 알려주는 역할을 한다. 그러므로 포인터에 [역참조 연산자](https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_indirection_operator) `*`를 사용하면 자료형을 반영한 메모리 주소에 저장된 값을 반환한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">포인터와 자료형의 관계</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">동일한 자료형</th><th style="text-align: center;">상이한 자료형</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
int variable = 365;
int *ptr = &variable;

printf("%p\n%d\n", ptr, *ptr);
```
</td><td>

```c
int variable = 365;
char *ptr = &variable;

printf("%p\n%d\n", ptr, *ptr);
```
</td></tr><tr style="vertical-align: top;"><td>

```terminal
000000526132F5E4 (주소)
365              (값)
```
</td><td>

```terminal
000000526132F5E4 (주소)
109              (값)
```
</td></tr></tbody></table>

여기서 역참조 연산자의 `*`와 포인터 선언에 사용된 별표는 기호만 동일한 뿐, 연관성이 없는 전혀 다른 존재이다. 만일 `variable` 변수의 값이 변하면 해당 메모리 주소를 참조하는 `ptr` 포인터의 역참조로도 관측이 가능하다. "참조에 의한 호출(call by reference)"은 이러한 매커니즘을 기반하며, 이미 함수에서 [배열](#배열)을 매개변수로 전달하는 방법을 소개할 때 선보였다.

* **널 포인터(null pointer)**

    아무런 메모리를 가리키지 않는 포인터이다. C 언어에서 포인터가 더 이상 사용되지 않는 메모리 주소를 계속 가리키고 있으면, 이는 자칫 NTSTATUS 0xC0000005 메모리 접근 오류를 유발할 수 있다. 안전한 포인터 사용을 위해 해당 포인터에 [`NULL`](https://en.cppreference.com/w/cpp/types/NULL)을 할당한다.

    ```c
    int *ptr = NULL;
    printf("%p", ptr);
    ```
    ```
    0000000000000000
    ```

* **보이드 포인터(void pointer)**

    지정된 자료형이 없는, 즉 `void` 자료형의 포인터이다. 이는 자료형과 무관하게 단순히 가리키고자 하는 메모리 주소만을 저장하기 위한 방법으로 사용된다.

    ```c
    int variable = 356;
    
    void *ptr = &variable;
    printf("%d", *(int*)ptr);
    ```
    ```
    365
    ```

### 함수 포인터
함수 포인터(function pointer)는 함수 자체를 가리키는 보이드 포인터이다. 배열 자체가 첫 번째 요소 메모리 주소를 가리키는 것과 동일한 맥락이다. 함수 포인터를 활용한 대표적인 예시로 [콜백 함수](#콜백-함수)가 있다. 함수 포인터의 선언은 아래와 같아야 하며, 이를 만족하지 않을 시 컴파일 오류가 발생한다.

1. 포인터의 자료형은 함수의 자료형과 일치해야 한다. 
1. 함수가 갖는 매개변수의 자료형과 개수가 동일해야 한다.

```c
int function(int arg1, float arg2) {
    statements;
    return 0;
}

int main() {

    // 함수 포인터 선언 및 호출
    int (*ptr)(int, float) = function;
    ptr(1, 3.14);

    return 0;
}
```

## 엔디언
[엔디언](https://ko.wikipedia.org/wiki/엔디언)(endianess)이란 컴퓨터가 메모리로부터 데이터를 표현하기 위해 바이트 단위의 정보를 어떻게 정렬할 것인지를 가리킨다. 특히 포인터가 메모리 주소를 접근 및 호출하기 때문에 엔디언의 기본적인 개념 이해는 필요하다고 본다.

엔디언이 C 언어 프로그래밍에 어떠한 영향을 주는지 설명하기 위해, 아래 예시는 십진수 정수를 십육진수로 변환 및 메모리 주소를 출력하였다.

> 본 예시는 이해를 돕기 위해 32비트 어플리케이션으로 빌드한 것이며, 64비트 어플리케이션은 메모리 주소 길이가 배로 늘어날 뿐 동일한 결과를 보여준다.

```c
int variable = 123456789;

printf("십육진수: %#010x\n", variable);
printf("포인터: 0x%p\n", &variable);
```
```terminal
십육진수: 0x075bcd15
포인터: 0x00CFF790
```

위의 바이트 네 개, `0x07`, `0x5b`, `0xcd`, 그리고 `0x15`는 각각 `int` 정수 자료형을 정의하면 할당되는 인접한 네 개의 메모리 주소에 저장된다. 그리고 포인터를 호출하면 전체 메모리 중에서 첫 번째 주소만 호출한다고 설명하였다. 그렇다면 한 바이트만 저장할 수 있는 첫 번째 메모리 주소에는 실제로 어떤 값이 들어있는가: `0x07` 아니면 `0x15`인가?

아래는 두 유형의 엔디언에 대하여 간략하게 소개한다.

* **빅 엔디언(big-endian; BE)**: 최상위 바이트가 첫 주소에 할당된다.

    ```terminal
    +---------------------------------------------------+
    | 0x00CFF790 | 0x00CFF791 | 0x00CFF792 | 0x00CFF793 |
    |------------+------------+------------+------------|
    |    0x07    |    0x5b    |    0xcd    |    0x15    |
    +---------------------------------------------------+
    ```

* **리틀 엔디언(little-endian; LE)**: 최하위 바이트가 첫 주소에 할당된다.

    ```terminal
    +---------------------------------------------------+
    | 0x00CFF790 | 0x00CFF791 | 0x00CFF792 | 0x00CFF793 |
    |------------+------------+------------+------------|
    |    0x15    |    0xcd    |    0x5b    |    0x07    |
    +---------------------------------------------------+
    ```

십육진수 `0x075bcd15`와 `0x15cd5b07`는 각각 십진수로 변환하면 123456789 그리고 365779719가 나온다. 그러나 결론적으로 프로그램의 각 메모리 주소를 확인해 보면 *리틀 엔디언*으로 데이터가 저장되고 있음을 확인할 수 있다.

```c
int variable = 123456789;
unsigned char* ptr = (void*)&variable;

for (int index = 0; index < sizeof(variable); index++) {
    printf("0x%#p : %#04x\n", ptr + index, *(ptr + index));
}
```
```terminal
0x00CFF790 : 0x15
0x00CFF791 : 0xcd
0x00CFF792 : 0x5b
0x00CFF793 : 0x07
```

비록 숫자를 읽을 때에는 빅 엔디언이 익숙하겠지만, 컴퓨터 메모리에서는 리틀 엔디언으로 데이터를 저장한다는 점을 명시하도록 한다.

# 동적 할당
소스 코드에서 정의된 [변수](#변수)와 [함수](#함수)들은 [메모리](Memory.md)의 [스택](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%83%9D)(stack) 영역에서 [레지스터](Processor.md)에 의한 푸쉬(push)와 팝(pop)이 빠른 속도로 이루어지면서 [프로세스](Process.md)가 실행된다. 하지만 스택 구조의 특성상 메모리 데이터를 저장하기에 부적합하며, 특히 블록 내에 정의된 변수를 외부에서 사용할 수 없는 점도 스택에 의한 현상이다. 이러한 한계점을 극복하기 위한 기술이 바로 프로세스 [런타임](https://ko.wikipedia.org/wiki/런타임) 도중에 메모리를 확보하는 "[동적 할당](https://ko.wikipedia.org/wiki/C_동적_메모리_할당)(dynamic allocation)"이다. 만일 [배열](#배열)을 변수에 정의하였다면, 프로세스 실행 당시에 애초부터 이를 고려하여 스택상 메모리가 미리 확보된 점과 상반되는 동작이다.

동적 할당은 [힙](https://en.wikipedia.org/wiki/Memory_management#HEAP)(heap) 영역에 메모리를 할당하여, 스택의 영향을 전혀 받지 않은 채 데이터를 저장할 수 있다.

> 힙 영역은 [힙 자료구조](https://ko.wikipedia.org/wiki/힙_(자료_구조))와 전혀 상관이 없으며, 사전적으로 "(데이터) 더미"를 뜻하는 순수히 물리 메모리의 주소공간 영역을 지칭하는 용어이다.

[`stdlib.h`](https://en.cppreference.com/w/cpp/header/cstdlib) 헤더 파일을 통해 개발자가 원하는 만큼 메모리를 할당받아 사용할 수 있지만, 반면 사용하지 않게 된다면 개발자가 직접 할당받은 메모리를 해제(free)하여 시스템에 반환해야 한다. 이러한 작업이 충분히 이루어지지 않는다면 메모리 누수(memory leak)가 발생하여 리소스 고갈로 프로세스 충돌을 야기한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어의 동적 할당 함수</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">함수</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/memory/malloc"><code>malloc</code></a></td><td>원하는 바이트 크기만큼 메모리 공간을 할당받는다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/memory/calloc"><code>calloc</code></a></td><td>원하는 바이트 크기만큼 메모리 공간을 지정한 횟수만큼 반복하여 할당 및 영값으로 초기화한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/memory/realloc"><code>realloc</code></a></td><td>동적 할당받은 메모리를 새로운 크기로 재할당받는다.</td></tr><tr><td style="text-align: center;"><a href="https://en.cppreference.com/w/c/memory/free"><code>free</code></a></td><td>동적 할당받은 메모리를 해제한다.</td></tr></tbody></table>

```c
#include <stdlib.h>

int* ptr = malloc(10);
ptr = realloc(ptr, 20);

free(ptr);
```

* **[메모리 누수](https://ko.wikipedia.org/wiki/메모리_누수)(memory leak)**

    더 이상 사용되지 않는 동적 할당된 메모리가 계속 잔여하여, 프로세스의 [가상 주소 공간](Process.md#가상-주소-공간)에 할당할 수 있는 메모리 리소스가 점차 줄어드는 현상이다. 가상 주소 공간에 더 이상 할당받을 수 있는 메모리가 없으면 프로세스 충돌이 발생하여 종료된다.

* **[허상 포인터](https://ko.wikipedia.org/wiki/허상_포인터)(dangling pointer)**

    *[NTSTATUS](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) [0xC0000005](https://learn.microsoft.com/en-us/shows/inside/c0000005) STATUS_ACCESS_VIOLATION 참고*

# 사용자 정의 자료형
**사용자 정의 자료형**(user-defined data type)이란, `int`, `float`, 또는 `char` 등의 기존 C 언어의 [자료형](#자료형)을 활용하여 특정 목적을 위해 제작한 커스텀 자료형이다. 완전히 새로운 자료형을 창조하는 게 아니며, 사용자 정의 자료형으로부터 초기화된 데이터를 **인스턴스**(instance)라고 부른다.

## 구조체
[구조체](https://en.cppreference.com/w/c/language/struct)(structure)는 여러 내부 변수, 일명 맴버(member)들을 자료형과 무관하게 하나의 단일 데이터로 통합시킨 사용자 정의 자료형이며, `struct` 키워드로 정의된다. 아래 C 언어 예시는 문자형과 정수형 맴버를 가진 구조체를 선언한다.

```c
struct STRUCTURE {
    char  field1;
    int   field2;
};
```

구조체로 변수를 정의하려면 기존 자료형처럼 변수 앞에 구조체를 기입하는데, 이때 `struct` 키워드도 함께 명시하여 컴파일러에게 구조체임을 알려야 한다.

1. 구조체 맴버에 값을 할당하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">순차적 맴버 정의</th><th style="text-align: center;">명시적 맴버 정의</th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    struct STRUCTURE variable = { 'A', 3 };
    ```
    </td><td>
    
    ```c
    struct STRUCTURE variable = { 
        .field2 = 3,
        .field1 = 'A'
    };
    ```
    </td></tr><tr><td>구조체에서 맴버를 선언한 순서대로 데이터를 나열한다.</td><td>구조체 맴버를 직접 명시하고 할당함으로 순서의 영향을 받지 않는다.</td></tr>
    </tbody>
    </table>

1. 구조체 선언과 동시에 변수를 함께 정의하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">명명된 구조체</th><th style="text-align: center;">익명 구조체</th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    struct STRUCTURE {
        char  field1;
        int   field2;
    } variable = { 'A', 3 };
    ```
    </td><td>
    
    ```c
    struct {
        char  field1;
        int   field2;
    } variable = { 'A', 3 };
    ```
    </td></tr><tr style="vertical-align: top;"><td>차후 해당 구조체로부터 다른 변수를 정의하는 등 재활용이 가능하다.</td><td>재활용이 불가하지만, 사실상 이름을 지정할 필요가 없는 네스티드(nested), 즉 다른 사용자 정의 자료형 내에 흔히 사용된다.</td></tr></tbody></table>

1. 구조체 맴버를 호출하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators">객체 맴버 연산자</a></th><th style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators">포인터 맴버 연산자</a></th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    printf("%c\n%d",
        variable.field1, variable.field2);
    ```
    </td><td>
    
    ```c
    struct STRUCTURE *ptr = &variable;
    printf("%c\n%d", ptr->field1, ptr->field2);
    ```
    </td></tr><tr style="vertical-align: top;"><td>객체 맴버 연산자 <code>.</code>를 활용한 "값에 의한 호출"이다.</td><td>포인터 맴버 연산자 <code>-&gt;</code>를 활용한 "참조에 의한 호출"이다.</td></tr></tbody></table>

각 맴버에 새로운 값을 할당하는 건 [배열](#배열)과 마찬가지로 가능하지만, 만일 모든 맴버들을 한꺼번에 할당할 필요가 있다면 [캐스팅](#자료형-변환)을 활용할 수 있다.

```c
variable = (struct STRUCTURE){ 'B', 7 };
```

### 데이터 구조 정렬
위의 예시로 소개된 구조체의 크기를 반환하면 다음과 같이 출력된다.

```c
struct STRUCTURE {
    char  field1;
    int   field2;
};

printf("%d", sizeof(struct STRUCTURE));
```
```terminal
8
```

분명 1바이트의 `char`과 4바이트의 `int`를 취합하면 총 5바이트가 계산되어야 한다고 생각이 들겠지만, 이는 시스템 프로세서 차원에서 메모리 접근성을 위한 [데이터 구조 정렬](https://en.wikipedia.org/wiki/Data_structure_alignment)(data structure alignment)이 반영된 결과이다. 만일 n-바이트 자료형의 데이터가 n-바이트 배수 간격의 메모리 주소에 할당되었을 시, 시스템 관점에서는 "자연스럽게 정렬(naturally aligned)"되었다고 하며 하드웨어 성능 효율이 가장 높다.

대체적으로 자료형마다 지정된 정렬 크기는 해당 자료형 크기와 일치하는 편이다: `char`은 1바이트 정렬, `short`는 2바이트 정렬, `int` 및 `float`는 4바이트 정렬이다. 다양한 자료형 맴버들로 구성될 수 있는 구조체의 경우, 메모리 공간 절약보다 접근 효율이 우선시되기 때문에 맴버 자료형이 갖는 가장 큰 정렬 크기의 배수만큼 메모리를 할당받아 맴버들을 정의된 순서대로 정렬시킨다.

> 그러므로 위의 예시는 가장 큰 자료형인 `int`에 따라 4바이트 정렬되어 총 8바이트의 크기가 도출된 것이다.

다음은 몇 가지 경우에 따라 데이터 구조 정렬이 어떻게 이루어진 것인지 설명한다:

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">맴버 구성에 따른 데이터 구조 정렬 비교</caption><colgroup><col style="width: 33.3%;"/><col style="width: 33.4%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th style="text-align: center;">8바이트: <code>char</code>-<code>int</code> 구조</th><th style="text-align: center;">8바이트: <code>char</code>-<code>short</code>-<code>int</code> 구조</th><th style="text-align: center;">12바이트: <code>char</code>-<code>int</code>-<code>short</code> 구조</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[3];  // +3
// ------ Addr: 0x00000004
    int   field2;       // +4
// ------ Addr: 0x00000008
};
```
</td><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[1];  // +1
    short field2;       // +2
// ------ Addr: 0x00000004
    int   field3;       // +4
// ------ Addr: 0x00000008
};
```
</td><td>

```c
struct STRUCTURE {
// ------ Addr: 0x00000000
    char  field1;       // +1
//  char  Padding1[3];  // +3
// ------ Addr: 0x00000004
    int   field2;       // +4
// ------ Addr: 0x00000008
    short field3;       // +2
//  char  Padding2[2];  // +2
// ------ Addr: 0x0000000C
};
```
</td></tr><tr style="vertical-align: top;"><td>정렬에 의해 맴버 간 여분이 발생하면 메모리의 연속성을 위해 패딩으로 메워진다.</td><td>구조체 자체의 정렬을 위해, 구조체 크기는 정렬 크기의 배수이어야 한다. 맨 마지막 맴버의 자료형 크기가 정렬 크기에 미치지 못하면 나머지를 패딩으로 채운다.</td><td>비록 <code>short</code> 자료형이 2바이트 정렬인 관계로 <code>char</code> 자료형 맴버 사이에 1바이트 패딩이 메워지지만, <code>int</code> 자료형에 의한 4바이트 크기의 정렬 경계 내에 두 맴버를 모두 담아낼 수 있기 때문이다.</td></tr></tbody></table>

## 공용체
[공용체](https://en.cppreference.com/w/cpp/language/union)(union)는 여러 내부 변수, 일명 맴버(member)들을 자료형과 무관하게 하나의 단일 데이터로 통합시킨 사용자 정의 자료형이며, `union` 키워드로 정의된다. 각 맴버마다 주어진 메모리가 있는 [구조체](#구조체)와 달리, 공용체의 맴버들은 하나의 메모리를 공용한다. 즉, 공용체의 한 맴버에 값이 변경되면 나머지 맴버에도 영향을 미친다.

```c
union UNION {    
    char  field1;
    int   field2;
};
```

공용체로 변수를 정의하려면 기존 자료형처럼 변수 앞에 공용체를 기입하는데, 이때 `union` 키워드도 함께 명시하여 컴파일러에게 공용체임을 알려야 한다.

1. 공용체 맴버에 값을 할당하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">최전방 맴버 자료형 기준</th><th style="text-align: center;">명시적 맴버 자료형 기준</th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    union UNION variable = { 365 };
    // field1 = 'm' (0x0000006d)
    // field2 = 109 (0x0000006d)
    ```
    </td><td>
    
    ```c
    union UNION variable = { .field2 = 365 };
    // field1 = 'm' (0x0000006d)
    // field2 = 365 (0x0000016d)
    ```
    </td></tr><tr><td>숫자 365(<code>0x16d</code>) 중에서 가장 먼저 선언된 문자형 맴버에 의해 1바이트만 공용체에 저장되어, <code>field2</code>에는 숫자 109(<code>0x6d</code>)만 출력된다. </td><td>공용체 맴버를 직접 명시하고 할당하면 해당 맴버의 자료형에 따라 값이 저장되어, <code>field2</code>에는 숫자 365(<code>0x16d</code>)가 온전히 출력된다.</td></tr></tbody></table>

1. 공용체 선언과 동시에 변수를 함께 정의하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">명명된 공용체</th><th style="text-align: center;">익명 공용체</th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    union UNION {
        char  field1;
        int   field2;
    } variable = { 365 };
    ```
    </td><td>
    
    ```c
    union {
        char  field1;
        int   field2;
    } variable = { 365 };
    ```
    </td></tr><tr style="vertical-align: top;"><td>차후 해당 공용체로부터 다른 변수를 정의하는 등 재활용이 가능하다.</td><td>재활용이 불가하지만, 사실상 이름을 지정할 필요가 없는 네스티드(nested), 즉 다른 사용자 정의 자료형 내에 흔히 사용된다.</td></tr></tbody></table>

1. 공용체 맴버를 호출하는 방법:

    <table style="width: 95%; margin-left: auto; margin-right: auto;"><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators">객체 맴버 연산자</a></th><th style="text-align: center;"><a href="https://en.cppreference.com/w/cpp/language/operator_member_access#Built-in_member_access_operators">포인터 맴버 연산자</a></th></tr></thead><tbody><tr style="vertical-align: top;"><td>
    
    ```c
    printf("%c\n%d",
        variable.field1, variable.field2);
    ```
    </td><td>
    
    ```c
    union UNION *ptr = &variable;
    printf("%c\n%d", ptr->field1, ptr->field2);
    ```
    </td></tr><tr style="vertical-align: top;"><td>객체 맴버 연산자 <code>.</code>를 활용한 "값에 의한 호출"이다.</td><td>포인터 맴버 연산자 <code>-&gt;</code>를 활용한 "참조에 의한 호출"이다.</td></tr></tbody></table>

## 열거형
> 초창기 C 컴파일러인 [*K&R C*](https://ko.wikipedia.org/wiki/C_(프로그래밍_언어)#K&R_C)에는 존재하지 않았으나, 본 문서에서 다루는 보편적인 컴파일러 버전인 [*ANSI C*](https://ko.wikipedia.org/wiki/ANSI_C)부터 추가되었다.

[열거형](https://en.cppreference.com/w/cpp/language/enum)(enumeration)은 열거된 항목, 일명 열거자(enumerator)들을 정수로 순번을 매기며, `enum` 키워드 정의된다. [구조체](#구조체)와 [공용체](#공용체)처럼 커스텀 자료형을 제작하는 게 아닌, 정수를 [식별자](#식별자)로 대신 치환하여 소스 코드 가독성을 높여주는 역할을 한다. 다음은 열거형에 대한 유의사항이다:

1. 열거자들은 정수 0부터 시작하여 다음 열거자마다 1만큼 증가한다. 할당 연산자 `=`로 정수를 직접 지정하지 않는 이상, 이러한 규칙은 계속 유지된다.

    ```c
    enum ENUMERATION {
        enumerator1,     // = 0
        enumerator2,     // = 1
        enumerator3 = 7, // = 7
        enumerator4      // = 8
    };
    ```

1. 비록 다른 열거형에 정의된 열거자여도 식별자는 전역적으로 유일해야 한다.

    ```c
    enum ENUMERATION1 {
        enumerator1,
        enumerator2,
    };
    
    enum ENUMERATION2 {
        enumeration2,    // <- [C2086] 'enumerator2': 재정의: 이전 정의는 '열거자'입니다.
        enumeration3,
    };
    ```

열거형 선언 이후, 열거자를 추가하거나 열거형 값을 변경하는 건 불가하다. 그래도 열거형으로부터 정의된 변수는 범위 외의 정수나 타 열거형의 열거자를 할당받아 사용할 수 있다. 아래는 선언된 열거형으로부터 변수를 정의하는 구문을 보여준다.

```c
enum ENUMERATION variable = enumerator1;
```

## `typedef` 키워드
[`typedef`](https://en.cppreference.com/w/c/language/typedef) 키워드는 C 언어 내장 자료형 및 사용자 지정 자료형에 별칭(alias)을 선언하여 가독성을 높이는 역할을 한다.

```c
// unsigned 문자 자료형의 BYTE 별칭 선언
typedef unsigned char BYTE;
```

`typedef` 키워드는 구조체와 공용체 등의 사용자 정의 자료형을 더 간편하게 사용할 수 있도록 하는 역할도 지닌다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">사용자 정의 자료형의 <code>typedef</code> 활용법</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><code>typedef struct</code></th><th style="text-align: center;"><code>typedef union</code></th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
typedef struct {
    char field1;
    int  field2;
} STRUCTURE;

STRUCTURE variable;
variable = (STRUCTURE){ 'A', 3 };
```
</td><td>

```c
typedef union {
    int  field1;
    char field2;
} UNION;

UNION variable;
variable = (UNION){ 365 };
```
</td></tr></tr></tbody></table>

# 예외 처리
[예외](https://ko.wikipedia.org/wiki/예외_처리)(exception)는 [런타임](https://ko.wikipedia.org/wiki/런타임) 도중에 잘못된 데이터 처리나 적절하지 않은 알고리즘 등에 의해 프로그램상 실행 불가한 코드 오류이다. C 언어의 구문적 문제가 아닌 관계로 정상적으로 빌드되지만, 예외를 마주하게 되면 [프로세스](Process.md)는 충돌하여 즉시 종료된다. 그러므로 예외 처리(exception handling)란, 프로세스가 오류를 처음으로 마주한 순간인 [1차 시도 예외](ProcDump.md#예외-처리)(1<sup>st</sup> chance exception)에서 유연하게 대처하여 종료되는 것을 방지하고 안정적으로 실행을 유지하는 게 주목표이다.

## 오류 번호
[오류 번호](https://en.cppreference.com/w/c/error/errno)(error number) 혹은 `errno` [매크로](#매크로-정의)는 `errno.h` 헤더 파일 내에 정의된 전역 변수이다. 매크로를 사용하기 위해서는 먼저 정수 0으로 초기화하고, 런타임 도중에 오류가 발생하면 이에 대응하는 오류 번호가 자동으로 할당된다. MSVC의 경우, 오류 번호와 내용은 [여기](https://learn.microsoft.com/en-us/windows/win32/debug/system-error-codes)에서 확인할 수 있다.

아래의 예시 코드는 존재하지 않는 파일을 읽기 모드로 열려고 할 때 발생하는 오류를 `errno` 매크로로 감지한다.

```c
#include <errno.h>

// errno 전역 변수 선언
extern int errno;

int main(){

    // errno 전역 변수 초기화
    errno = 0;
    
    FILE* fptr = fopen("./non_existance.txt", "r");
    
    // 파일 열기 실패 경우...
    if (fptr == NULL) {
        // 오류명 및 번호: ENOENT 2 (해당 파일 혹은 경로 미발견)
        fprintf(stderr, "파일 열기 오류 발생! 오류 코드: %d\n", errno);
        exit(-1);
    }

    fclose(fptr);
    return 0;
}
```
```
파일 열기 오류 발생! 오류 코드: 2
```

### 오류 설명
각종 오류들은 정수형으로 표현되어 `errno` 매크로를 통해 전역 변수에 저장된다. 그러나 해당 오류를 정수가 아닌 텍스트로 된 내용을 보기 위해서는 [`perror()`](https://en.cppreference.com/w/c/io/perror) 함수를 사용한다.

```c
int main(){
    
    FILE* fptr = fopen("./non_existance.txt", "r");
    if (fptr == NULL) {

        // 오류명 및 번호: ENOENT 2 (해당 파일 혹은 경로 미발견)
        perror("오류 설명");
        exit(-1);
    }

    fclose(fptr);
    return 0;
}
```
```
오류 설명: No such file or directory
```

# 전처리기
C 언어가 컴파일되기 이전에 전처리기로부터 `#include`와 같은 전처리기 지시문이 우선적으로 처리된다. 전처리기 지시문은 C 언어 컴파일러 설정 및 프로그래밍의 편리성을 제공한다. 본 장에서는 일부 유용한 전처리기 지시문에 대하여 소개한다.

## 매크로 정의
[매크로](https://en.cppreference.com/w/cpp/preprocessor/replace)(macro)란 식별자가 있는 코드 조각이다. 코드 조각은 숫자나 문자와 같은 간단한 데이터가 될 수 있으며(객체형식 매크로; object-like macro), 전달인자를 받는 표현식이나 문장이 될 수도 있다(함수형식 매크로; function-like macro). 매크로는 `#define` 지시문으로 정의되며, 각 매크로에 해당하는 데이터 및 표현식이 소스 코드에 대체된다. 정의된 매크로는 `#undef` 지시자로 제거할 수 있다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 언어의 매크로 유형</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">객체형식 매크로</th><th style="text-align: center;">함수형식 매크로</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
#define MACRO    7

printf("%d", MACRO);

#undef MACRO
```
</td><td>

```c
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
</td></tr></tbody></table>

한 번 정의된 매크로는 런타임 도중에 변경이 불가하다. 정의된 매크로는 마치 전역 변수인 마냥 헤더 파일에서 `#include`와 같은 포함 지시문을 통해 다른 스크립트에서도 사용할 수 있다.

컴파일러에는 공통된 표준 매크로 및 컴파일러마다 전용 매크로가 내장되어 있다.

* **Visual C++**: [Microsoft Docs - 미리 정의된 매크로](https://learn.microsoft.com/en-us/cpp/preprocessor/predefined-macros)
* **GCC**: [GCC Online Documentation - Predefined Macros](https://gcc.gnu.org/onlinedocs/cpp/Predefined-Macros.html)
* **그 외**: [SourceForge Wiki](https://sourceforge.net/p/predef/wiki/Compilers/)

### 쉼표 연산자
[쉼표 연산자](https://en.cppreference.com/w/c/language/operator_other#Comma_operator)(comma operator)는 앞에 있는 표현식을 평가하되 반환되지 않고, 뒤에 있는 표현식이 평가되어 반환된다. 흔히 매크로 정의를 간결하게 하기 위해 사용된다. 아래의 예시 코드에 의하면 먼저 할당 연산자로 `value1`은 4가 되고, 이후에 증가 연산자에 의해 5가 된다.

```c
int value1 = 1, value2 = 3;
int variable = (value1 += value2, ++value1);
printf("%d", variable);
```
```terminal
5
```

## 조건 포함문
[조건 포함문](https://en.cppreference.com/w/cpp/preprocessor/conditional)(conditional inclusion)은 조건여부에 따라 컴파일 시 특정 범위의 코드를 포함시킬 것인지 배제할 것인지 결정한다. 비록 조건 포함문이 일반 조건문의 키워드와 유사할지라도 절대 [`if`](#if-조건문) 및 [`else`](#if-조건문) 조건문을 대체하기 위해 사용되지 말아야 한다.

```c
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

```c
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

```c
#pragma once
```

결과적으로 하나의 소스 파일에 헤더 파일이 중복적으로 포함이 되는 것을 제한하므로써 정의가 반복되는 현상을 막을 수 있는데, 이러한 기능을 [헤더 중복 방지](https://en.wikipedia.org/wiki/Include_guard)(include guard)라고 부른다. 추가적으로 `#pragma once` 지시문을 사용하면 처리하는 헤더 파일 횟수가 줄어들어 컴파일 작업 시간도 함께 줄이게 된다.

아래의 코드는 `#pragma once` 지시문을 사용하지 않고 헤더 중복 방지 기능을 구현하는 방법이다.

```c
// 헤더 파일: "header.h"
#ifndef HEADER_FILE
#define HEADER_FILE

    ...

#endif
```

만일 header.h 헤더 파일이 아직 처리되지 않았으면 컴파일러는 처음으로 `HEADER_FILE` 매크로를 정의한다. 그러나 헤더 파일을 다시 한 번 마주하였을 시, `HEADER_FILE`이 이미 정의되어 있기에 매크로 조건에 의해 컴파일러는 헤더 파일을 처리하지 않는다.

### `#pragma region`
컴파일 작업에는 직접적인 영향을 주지 않지만, `#pragma region` 및 `#pragma endregion` 쌍은 가독성을 위해 비주얼 스튜디오에서 지정된 코드 부분을 한 줄로 압축하거나 펼치는 기능을 제공한다.

```c
#pragma region REGIONNAME

    ...

#pragma endregion REGIONNAME
```

# 링커
[링커](https://ko.wikipedia.org/wiki/링커_(컴퓨팅))(linker) 혹은 링크 편집기(link editor)는 소스 코드를 구성하는 각 .c 확장자 파일마다 기계어로 컴파일된 [오브젝트 파일](https://ko.wikipedia.org/wiki/목적_파일)들을 하나의 완전한 프로그램으로 동작할 수 있도록 서로 연동시키는 도구이다. C 언어의 [빌드](https://ko.wikipedia.org/wiki/소프트웨어_빌드) 과정은 결국 "[전처리기](#전처리기) → [컴파일러](#컴파일러) → 링커" 순서로 진행되는 작업을 함축한다. 링커를 통해 소스 코드는 외부 스크립트 또는 [라이브러리](#라이브러리)에 정의된 데이터나 코드를 불러와 활용할 수 있다.

## 포함 지시문
[포함 지시문](https://en.cppreference.com/w/cpp/preprocessor/include)(inclusive directive) `#include`는 전처리기 지시문 중 하나로 대표적으로 [`stdio.h`](#파일-입출력)와 같은 [헤더 파일](#헤더-파일)을 불러오기 위해 사용된다. `#include` 지시문의 역할은 헤더 파일에 작성된 코드 전체를 해당 위치에 삽입하여 함수 프로토타입과 전역 및 [`extern`](#extern-키워드) 변수를 선언한다. 소스 코드에 데이터와 함수가 정의되었다면, 헤더 파일은 데이터와 함수를 선언하는 목적으로 사용된다.

[진입점](#진입점)인 `main()` 함수는 선언부가 없다는 점을 고려하면 메인 스크립트를 다음과 같이 구성할 수 있다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">헤더 파일과 소스 코드 나누기</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">헤더 파일: <code>main.h</code></th><th style="text-align: center;">소스 코드: <code>main.c</code></th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
#include <stdio.h>

int variable;
void func(int, float);
```
</td><td>

```c
#include "main.h"

int main() {

    variable = 'A';
    func(variable, 0.01);

    return 0;
}

void func(int x, float y) {
    printf("%.3f\n", x + y);
}
```
</td></tr></tbody></table>

위의 소스 파일의 헤더 파일 포함은 결과적으로 `#include` 지시문으로 인해 다음과 같이 표현된 것과 동일하다.

```c
// #include "main.h" 코드 시작
#include <stdio.h>

int variable;
void func(int, float);
// #include "main.h" 코드 끝

int main() {

    variable = 'A';
    func(variable, 0.01);

    return 0;
}

void func(int x, float y) {
    printf("%.3f\n", x + y);
}
```

### 헤더 파일
[헤더 파일](https://ko.wikipedia.org/wiki/헤더_파일)(header file)은 데이터 및 기능의 존재를 알리는 역할을 하는 .h 확장자 파일이며, 링커로부터 오브젝트 파일들을 연동하기 위해 필요한 핵심 요소이다. 다른 스크립트 파일 또는 라이브러리에 정의된 데이터와 코드를 헤더 파일로 통해 다른 스크립트에서도 사용할 수 있도록 한다. 헤더 파일을 불러오는 방식은 두 가지가 존재한다:

```c
#include <stdio.h>
#include "header.h"
```

이 둘은 [전처리기](#전처리기)가 헤더 파일을 어느 위치에서 찾을 것인지 차이점을 가진다.

* **`#include <header.h>`**
    
    컴파일러 혹은 IDE에서 지정한 경로를 위주로 헤더 파일을 찾으며, 일반적으로 시스템 헤더 파일에 사용된다.

* **`#include "header.h"`**
    
    현재 소스 파일이 위치한 경로를 위주로 헤더 파일을 찾는다. 만일 찾지 못하였을 시, `#include <header.h>`와 같이 지정된 경로에서 헤더 파일을 재탐색한다. 일반적으로 사용자 정의 헤더 파일에 사용된다.

아래는 프로그래밍 언어에서 흔히 사용되는 데이터와 기능들은 바로 사용할 수 있도록 미리 컴파일된 [표준 라이브러리](https://ko.wikipedia.org/wiki/C_표준_라이브러리)를 불러오는 헤더 파일 일부를 나열한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">C 표준 라이브러리의 헤더 파일</caption><colgroup><col style="width: 18%;"/><col style="width: 12%;"/><col style="70%;"/></colgroup><thead><tr><th style="text-align: center;">유형</th><th style="text-align: center;">헤더 파일</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="#파일-입출력">표준 입출력</a></td><td style="text-align: center;"><code>stdio</code></td><td>파일 입출력 함수를 제공한다: <code>printf()</code>, <code>scanf()</code> 등</td></tr><tr><td style="text-align: center;">표준 라이브러리</td><td style="text-align: center;"><code>stdlib</code></td><td>메모리 할당, 예외처리를 포함한 범목적 기능들을 제공한다: <code>malloc()</code>, <code>free()</code> 등</td></tr><tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/C_날짜와_시간_함수">날짜 및 시간</a></td><td style="text-align: center;"><code>time</code></td><td>날짜 및 시간과 관련된 함수를 제공한다: <code>time()</code>, <code>clock()</code> 등</td></tr><tr><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/C_수식_함수">수식</a></td><td style="text-align: center;"><code>math</code></td><td>수학적 함수를 제공한다: <code>exp()</code>, <code>cos()</code> 등</td></tr></tbody></table>

헤더 파일 개수나 선언된 데이터가 매우 많은 경우, 프로젝트 빌드 시간을 줄이기 위한 방안으로 헤더 파일을 중간체 형태로 미리 변환시킨 [컴파일된 헤더](https://en.wikipedia.org/wiki/Precompiled_header)(precompiled header)를 활용하기도 한다. 허나 컴파일된 헤더를 사용하면 컴파일 작업 자체에는 시간이 다소 걸리는 단점이 있어, 용량이 작은 프로젝트나 자주 수정을 해야 하는 헤더 파일이 있다면 오히려 비효율적이다. MSVC 컴파일러에서는 `pch.h` 혹은 `stdafx.h`가 해당한다.

### `extern` 키워드
[`extern`](https://en.cppreference.com/w/c/language/extern) 키워드는 [변수](#변수)를 정의 없이 선언만 한다. C 언어에서 변수를 소개하였을 당시 특수한 경우를 제외하고 선언과 정의는 동일하게 취급한다고 언급한 점에서 상당히 대비된다. 변수나 함수를 정의(definition)하면 메모리를 할당받아, 동일한 [식별자](#식별자)로 다시 정의할 수 없게 된다. 반면, 선언(declaration)은 단순히 컴파일러에게 변수나 함수의 존재를 알려줄 뿐이며 메모리를 할당받지 않아 여러 번 선언이 가능하다.

위에서 설명한 특징을 상기하며, 아래의 예시를 통해 `extern` 키워드에 의해 스크립트에 미치는 영향을 살펴본다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><code>extern</code> 키워드를 설명하기 위한 예시</caption><colgroup><col style="width: 33.3%;"/><col style="width: 33.4%;"/><col style="width: 33.3%;"/></colgroup><thead><tr><th style="text-align: center;">모듈 헤더 파일: <code>module.h</code></th><th style="text-align: center;">모듈 소스 코드: <code>module.c</code></th><th style="text-align: center;">메인 소스 코드: <code>main.c</code></th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```c
#include <stdio.h>

// variable 변수 선언
extern char variable;

void func(int, float);
```
</td><td>

```c
#include "module.h"

// variable 변수 정의
char variable = 'A';

void func(int x, float y) {
    printf("%.3f", x + y);
}
```
</td><td>

```c
#include "module.h"

int main() {

    func(variable, 0.01);

    return 0;
}
```
</td></tr><tr><td colspan="3">위의 예시에서 두 소스 코드 <code>module.c</code>와 <code>main.c</code>는 하나의 헤더 파일 <code>module.h</code>에 의해 링커로부터 서로 연동된다. <code>extern</code> 키워드에 의해 변수는 여러 번 선언이 가능하지만, 실제 코드에서 사용하기 위해서는 단 한 번의 정의가 필요하다. <code>module.c</code>에서의 <code>variable</code>를 정의한 이유가 바로 이러한 이유가 배경이 된 것이다.</td></tr></tbody></table>

위의 예시에서 자칫 잘못하면 경우에 따라 MSVC 컴파일러 기준으로 두 가지 오류가 발생할 수 있다.

1. **[C2374](https://learn.microsoft.com/en-us/cpp/error-messages/compiler-errors-1/compiler-error-c2374)**: 동일한 식별자의 변수가 두 번 이상 정의되면서 발생한 컴파일러 오류이다; 예시에서 `extern` 키워드를 사용하지 않으면 나타난다.
1. **[LNK2001](https://learn.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk2001)**: 컴파일된 코드가 참조 혹은 호출하려는 [심볼](Symbol.md)의 정의를 찾을 수 없어 발생하는 링커 오류이다; 선언된 변수를 정의하지 않으면 나타난다.

한편, 함수 [프로토타입](#함수)는 원래부터 정의가 아닌 (또 다른 이름인 "전방선언"에서도 알 수 있듯이) 선언이므로 `extern` 키워드가 필요하지 않는다.

## 라이브러리
[라이브러리](https://ko.wikipedia.org/wiki/라이브러리_(컴퓨팅))(library)는 변수나 함수 등을 제공하지만, 소스 코드 형태가 아닌 이미 컴파일 및 링크된 완전한 형태의 이진 파일이다. 라이브러리에 연동된 헤더 파일이 있어 `#include` 포함 지시문으로 불러와 사용할 수 있다. 즉, 라이브러리 관점에서 헤더 파일은 [API](https://ko.wikipedia.org/wiki/API)를 제공하는 역할을 한다. 라이브러리로 컴파일을 하면 파일 용량이 줄어들고 배포하기 편리하며, 또한 소스 코드 유출을 방지할 수 있다.

![비주얼 스튜디오 라이브러리 컴파일 설정](./images/visual_studio_library.png)

라이브러리는 크게 두 종류로 나뉘어진다:

* **[정적 라이브러리](https://ko.wikipedia.org/wiki/정적_라이브러리)(static library)**

    소스 코드를 컴파일하면 라이브러리도 함께 프로그램의 일부로 융합되어 외부 환경에 대한 의존도를 상당히 낮출 수 있다. 다만, 프로그램 용량이 커지고 업데이트된 라이브러리를 적용하려면 소스 코드를 새로 컴파일해야 하는 단점이 있다. 윈도우 NT에서 .lib 확장자를 가진다(유닉스의 경우 .a).

* **[동적 라이브러리](https://ko.wikipedia.org/wiki/동적_링커)(dynamic library)**

    소스 코드를 컴파일하여도 라이브러리는 프로그램 일부로 융합되지 않아 프로그램 용량이 획기적으로 줄어들고 라이브러리 업데이트가 매우 편리하다. 하지만 컴파일된 프로그램이 라이브러리를 찾지 못하면 실행이 불가하거나 정상적으로 동작하지 않으며, VCRUNTIME140.dll을 찾을 수 없다는 오류창이 대표적인 예시이다. 윈도우 NT에서 .dll 확장자를 가진다(유닉스의 경우 .so).

비주얼 스튜디오에서 C 언어로 라이브러리를 빌드하려면 위의 그림과 같이 프로젝트 속성에서 구성 유형을 정적 혹은 동적 라이브러리로 변경한다. 소스 코드에서 데이터와 함수를 정의하되, `main()`은 [진입점](#진입점)이 아닌 일반 함수로 동작하는 점에 유의한다. 정의된 변수나 함수를 접근할 수 있도록 헤더 파일에 선언하여 컴파일하면 라이브러리가 완성된다.
