# 러스트
> *출처: [The Rust Programming Language <sub>(영문)</sub>](https://doc.rust-lang.org/book/)*

[Rust](https://www.rust-lang.org/)는 성능과 보안이 매우 강조된 [정적 정형](https://en.wikipedia.org/wiki/Type_system#Static_type_checking) 프로그래밍 언어이며, 특히 자료형 및 메모리 관리 측에서 강점을 지닌다. 뿐만 아니라, 하드웨어 제어가 가능하다는 점에서 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 개발에도 활용할 수 있다. 이러한 특성들에 의해 [C++](Cpp.md) 언어와 유사한 성능을 지닌 동시에 훌륭한 보안성이 보장되어 [마이크로소프트](https://www.microsoft.com/), [구글](https://www.google.com/), [아마존](https://www.amazon.com/) 등의 주요 IT 기업의 관심과 지원을 받고 있다.

## 설치
러스트 프로그래밍 언어를 실행하는 데 필요한 [툴체인](https://ko.wikipedia.org/wiki/툴체인)은 공식 홈페이지에서 rustup_init.exe를 [다운로드](https://www.rust-lang.org/tools/install) 받아 설치한다.

> 설치 과정에서 2013 버전 이상의 [비주얼 스튜디오](https://visualstudio.microsoft.com/downloads/)이 요구되는 데, 미리 Desktop development with C++ 그리고 윈도우 10 또는 11 SDK를 준비하도록 한다.

![<code>rustup_init.exe</code> 시작 화면](./images/rustup_init_setup.png)

여기서 러스트 프로그래밍 언어의 툴체인(toolchain)이란, 소프트웨어 제품 개발에 활용되는 프로그래밍 도구들의 집합을 가리키며 대표적으로 다음 세 개의 명령어 프로그램이 있다.

1. `rustc`: 러스트 [컴파일러](Programming.md#컴파일러)(compiler)
1. [`cargo`](#프로젝트): 러스트 패키지 관리자; 프로젝트를 컴파일 및 실행한다.
1. `rustup`: 러스트 툴체인 관리자, 즉 위의 `rustc` 및 `cargo` 프로그램을 다른 버전으로 설치 및 선택한다.

설치가 완료되면 `%UserProfile%` 디렉토리에 `.rustup` 및 `.cargo` 폴더가 생성된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트 관련 디렉토리 소개</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">디렉토리</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>.rustup</code></td><td><code>rustc</code>, <code>cargo</code> 등을 포함한 러스트 툴체인이 실질적으로 위치하는 디렉토리이며, <code>toolchains</code> 하위폴더에 버전마다 나뉘어 설치된다.</td></tr><tr><td style="text-align: center;"><code>.cargo</code></td><td>설치된 러스트 툴체인에 <a href="https://en.wikipedia.org/wiki/Hard_link">링크</a>된 <a href="https://en.wikipedia.org/wiki/Binary_file">바이너리 파일</a>이 위치하여, 개발자는 러스트의 전반적인 환경 구성을 그대로 유지한 채 <code>rustup</code>에서 선택한 버전으로 변경하여 사용할 수 있다.</td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><i><sub>† <a href="Python.md">파이썬</a>에 빗대어 설명하자면 전자는 실제 파이썬 <a href="Programming.md#인터프리터">인터프리터</a>가, 후자는 Py Launcher가 위치한 폴더에 해당한다.</sub></i></caption></table>

### 통합 개발 환경
[통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)은 최소한 프로그래밍 언어의 소스 코드 편집, 프로그램 빌드, 그리고 디버깅 기능을 제공하는 소프트웨어 개발 프로그램이다. 툴체인은 러스트 코드를 컴퓨터가 실행할 수 있도록 하는 개발 도구이지만 러스트 코드 편집기는 아니다. 러스트 코드를 작성하고 프로그램으로 실행하여 문제가 발생하면 검토할 수 있는 IDE가 절대적으로 필요하다.

* [비주얼 스튜디오 코드](https://code.visualstudio.com/download)<sup>[<a href="https://code.visualstudio.com/docs/languages/rust">참고</a>]</sup>, 일명 VS Code는 마이크로소프트에서 개발한 무료 소스 코드 편집기이다. 비록 기술적으로 IDE는 아니지만, [rust-analyzer 확장도구](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)를 설치하면 코드 자동완성, 자료형 정의, 구문 하이라이트 등의 기능들을 제공한다. 코드 디버깅을 하려면 아래를 함께 설치한다.

    * [마이크로소프트 C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) (윈도우 운영체제)
    * [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) (macOS 또는 리눅스 운영체제)

## 프로젝트
> *참고: [Managing Growing Projects with Packages, Crates, and Modules - The Rust Programming Language](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)*

러스트 프로그래밍 언어는 [`cargo`](https://doc.rust-lang.org/rust-by-example/cargo.html)라는 공식 패키지 관리 도구를 통해 프로젝트를 관리한다.

![VS Code에서 러스트 프로그래밍의 프로젝트](./images/vscode_rust.png)

<table style="width: 70%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><code>cargo</code> 도구의 프로젝트 관리 명령</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">명령어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>cargo build</code></td><td>프로젝트 빌드</td></tr><tr><td style="text-align: center;"><code>cargo run</code></td><td>프로젝트 (빌드 및) 실행</td></tr><tr><td style="text-align: center;"><code>cargo clean</code></td><td>프로젝트 빌드 결과물 제거 및 정리</td></tr><tr><td style="text-align: center;"><code>cargo check</code></td><td>프로젝트 유효성 검사; 컴파일 생략으로 소모 시간을 단축하나 결과물 부재</td></tr></tbody></table>

러스트 프로그래밍은 기본적으로 디버그 모드에서 프로젝트를 빌드하고 실행하며, 컴파일 부산물들은 `.\target\debug` 폴더 안에 위치한다. 만일 정식 배포를 위해 최적화된 프로그램으로 빌드하기 위헤 명령어에 `--release` 플래그를 추가하면 `.\target\release` 폴더에 프로그램이 생성된다.

* VS Code 편집기에 rust-analyzer 확장도구를 사용하면 `main()` 진입점 위에 "▶ Run | Debug" CodeLens 표시가 나타난다. 이는 `cargo`를 대신하여 프로젝트를 컴파일 및 실행하는 데 활용될 수 있다.

### 크레이트
[크레이트](https://doc.rust-lang.org/rust-by-example/crates.html)(crate; 화물상자)는 컴파일로 생성된 가장 작은 단위의 결과물이며, 간단히 말해 `.RS` 소스 파일(일명 크레이트 파일; crate file)로부터 생성된 실행 및 라이브러리 파일이다. 크레이트 파일 중에서 러스트 프로그래밍 컴파일의 근본이 되는 소스 파일을 크레이트 루트(crate root)이라고 부른다.

크레이트는 아래와 같이 두 유형으로 나뉘어진다:

<table style="width: 70%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">이진 및 라이브러리 크레이트 비교</caption><colgroup><col style="width: 16%;"/><col style="width: 44%;"/><col style="width: 44%;"/></colgroup><thead><tr><th></th><th style="text-align: center;">이진 크레이트 (binary crate)</th><th style="text-align: center;">라이브러리 크레이트 (library crate)</th></tr></thead><tbody><td style="text-align: center;">설명</td><td style="text-align: center;"><code>.EXE</code> 실행 프로그램</td><td style="text-align: center;"><code>.RLIB</code> 라이브러리 파일</td></tbody><tbody><td style="text-align: center;">크레이트 루트</td><td style="text-align: center;"><code>src/main.rs</code></td><td style="text-align: center;"><code>src/lib.rs</code></td></tbody><tbody><td style="text-align: center;"><code>main</code> 진입점</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tbody></table>

* [`std`](https://doc.rust-lang.org/std/index.html) 크레이트: 일명 "러스트 표준 라이브러리(Rust Standard Library)"는 러스트 소프트웨어의 기반이 되어 [자료형](#자료형), 표준 매크로, 그리고 다양한 [모듈](#모듈)을 제공한다. 모든 크레이트는 기본적으로 [`use`](https://doc.rust-lang.org/std/keyword.use.html) 키워드를 사용해 아래와 같이 표준 라이브러리를 불러올 수 있다.

    ```rust
    // 표준 라이브러리의 프로세스 환경 모듈
    use std::env;
    ```

### 패키지
패키지(package)는 하나 이상의 [크레이트](#크레이트)를 다루는 [번들](https://en.wikipedia.org/wiki/Product_bundling#Software)(bundle)이다. 각 패키지는 여러 개의 이진 크레이트를 다룰 수 있지만, 라이브러리 크레이트는 오로지 하나만 가능하다. 패키지의 특징 중 하나는 [`Cargo.toml`](#cargotoml) 파일을 가지고 있다는 점인데, 이는 크레이트를 어떻게 빌드를 할 것인지 설명한다.

러스트 프로그래밍에서 프로젝트를 생성한다는 것이 바로 패키지를 가리키며, 새로운 패키지를 생성하려면 아래 명령어를 입력한다.

```terminal
cargo new <프로젝트명>
```

## 표현식
**[표현식](https://doc.rust-lang.org/reference/expressions.html)**(expressions)은 반드시 값을 도출하는 평가(evaluate)가 이루어지면, 이로 인한 부가적인 영향이 발생할 수 있는 구문적 존재이다. 기존 [C](C.md)/[C++](Cpp.md) 언어와 달리, Rust은 표현식 지향 (expression-oriented) 프로그래밍이며 표현식의 개념이 더 포괄적이다.

이해를 돕기 위해, 아래는 Rust의 표현식에 해당하는 항목들 중 일부를 나열한다.

* [제어 흐름식](#제어-흐름식)
* [연산 표현식](https://doc.rust-lang.org/reference/expressions/operator-expr.html): 간단히 "연산자"라고 부른다.
* [블록 표현식](https://doc.rust-lang.org/reference/expressions/block-expr.html): 블록 `{}`은 [제어 흐름](#제어-흐름식) 및 익명의 [네임스페이스](#네임스페이스)의 영역을 나타낸다. 블록 내의 마지막 코드가 표현식이면 이는 블록의 반환값이 된다.<sup>[[출처](https://doc.rust-lang.org/rust-by-example/expression.html)]</sup>
* ([메소드](https://doc.rust-lang.org/reference/expressions/method-call-expr.html)) [호출 표현식](https://doc.rust-lang.org/reference/expressions/call-expr.html): 예를 들어, println! 함수 호출도 표현식이다. 비록 평가된 값은 `()`이지만, 텍스트가 출력되는 영향이 동반되었다.
* 기타 등등

### 문장
**[문장](https://doc.rust-lang.org/reference/statements.html)**(statements)은 [블록 표현식](https://doc.rust-lang.org/reference/expressions/block-expr.html) `{}` 안을 구성하는 구문적 존재이며, 코드의 끝에 [세미콜론](https://en.wikipedia.org/wiki/Semicolon) 여부로 판별된다. 블록 안에 있어도  이는 C 언어의 "문장"과 메우 유사하지만, Rust는 오로지 다음 두 유형의 문장만이 존재한다.

1. 선언문(declaration statement): `let` 키워드로 새로운 [변수](#변수)를 선언하거나 바인딩한다.
1. 표현문(expression statement): 세미콜론에 의해 값이 반환되지 않은 표현식을 가리킨다. 마지막 표현문에 세미콜론을 생략하면 

```rust
fn main() {
    let variable = 3;           // declaration statement
    if 2 < 3 { expressions };   //  expression statement
}
```

단, 블록 표현식 끝에 세미콜론을 붙이면 블록 자체가 값을 반환하지 않는다는 걸 의미하며, 불록 안의 표현식이 문장으로 전환되는 걸 의미하지 않는다.

### 토큰
**[토큰](https://doc.rust-lang.org/reference/tokens.html)**(token)은 프로그래밍 문법을 구성하는 가장 기본적인 요소이며, 대표적으로 [키워드](https://doc.rust-lang.org/reference/keywords.html), [식별자](https://doc.rust-lang.org/reference/identifiers.html), [리터럴](https://doc.rust-lang.org/reference/tokens.html#literals) 등이 있다.

## 자료형
**[자료형](https://doc.rust-lang.org/book/ch03-02-data-types.html)**(data type)은 데이터를 어떻게 표현할 지 결정하는 요소이며, 러스트에서는 다음과 같이 존재한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 <a href="https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#scalar-types">원시 스칼라 자료형</a></caption><colgroup><col style="width: 20%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">키워드</th><th style="text-align: center;">자료형</th><th style="text-align: center;">크기 (바이트)</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>i8</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u8</code>)</i></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/numeric.html">숫자</a></td><td style="text-align: center;">1</td><td>1바이트 정수</td></tr><tr><td style="text-align: center;"><code>i16</code><br/><i>(unsigned: <code style="font-style: normal;">u16</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">2</td><td>2바이트 정수</td></tr><tr><td style="text-align: center;"><code>i32</code><br/><i>(unsigned: <code style="font-style: normal;">u32</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>4바이트 정수</td></tr><tr><td style="text-align: center;"><code>i64</code><br/><i>(unsigned: <code style="font-style: normal;">u64</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>8바이트 정수</td></tr><tr><td style="text-align: center;"><code>i128</code><br/><i>(unsigned: <code style="font-style: normal;">u128</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>16바이트 정수</td></tr><tr><td style="text-align: center;"><code>isize</code><br/><i>(unsigned: <code style="font-style: normal;">usize</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16 <sub>(최소)</sub></td><td><a href="#포인터">포인터</a> 크기의 정수; 예를 들어 x86는 32비트, 그리고 x64는 64비트 정수로 계산된다.</td></tr><tr><td style="text-align: center;"><code>f32</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>32비트 <a href="https://en.wikipedia.org/wiki/Single-precision_floating-point_format">단정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f64</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>64비트 <a href="https://en.wikipedia.org/wiki/Double-precision_floating-point_format">배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f128</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>128비트 <a href="https://en.wikipedia.org/wiki/Quadruple-precision_floating-point_format">4배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>bool</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/boolean.html">논리</a></td><td style="text-align: center;">1</td><td>참(<code>true</code>; 1) 혹은 거짓(<code>false</code>; 0)</td></tr><tr><td style="text-align: center;"><code>char</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/textual.html">텍스트</a></td><td style="text-align: center;">4</td><td>단일 <a href="https://en.wikipedia.org/wiki/UTF-32">UTF-32</a> 문자: 0x0000-0xD7FF 또는 0xE000-0x10FFFF</td></tr></tbody/></table>

> [바이트](https://ko.wikipedia.org/wiki/바이트)(byte)란, 컴퓨터에서 메모리에 저장하는 가장 기본적인 단위이다. 자료형마다 크기가 정해진 이유는 효율적인 메모리 관리 차원도 있으나 CPU 연산과도 깊은 연관성을 갖는다. 한 바이트는 여덟 개의 [비트](https://ko.wikipedia.org/wiki/비트_(단위))(bit)로 구성된다.

여기서 [unsigned](https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int)란, 자료형 중에서 [최상위 비트](https://ko.wikipedia.org/wiki/최상위_비트)를 정수의 [부호](https://ko.wikipedia.org/wiki/Signed와_unsigned)를 결정하는 요소로 사용하지 않는다. 아래의 16비트 정수형인 `i16`는 원래 최상위 비트를 제외한 나머지 15개의 비트로 정수를 표현한다. 반면 unsigned 자료형인 `u16`은 음의 정수를 나타낼 수 없지만, 16개의 비트로 양의 정수를 더 많이 표현할 수 있다.

```rust
i16             // 표현 가능 범위: -32768 ~ +32767
u16             // 표현 가능 범위:     +0 ~ +65535
```

그 외의 자료형들은 차후 러스트 프로그래밍 언어에 대한 개념을 소개하면서 함께 설명할 예정이다.

### 자료형 변환
**[자료형 변환](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)**(type conversion)은 데이터를 다른 자료형으로 바꾸는 작업이며, 불가피하게 데이터가 손실될 수 있으므로 유의하도록 한다. 러스트는 일반적으로 [강제 변환](https://doc.rust-lang.org/reference/type-coercions.html)(coercion)이란 암시적 자료형 변환을 지원하지 않으며, 자동 변환은 오로지 특정 위치의 코드에서 제한된 자료형에만 국한된다.

* **[자료형 캐스팅](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)**(type casting)

    명시적 자료형 변환에 해당하며, 데이터의 우변에 이진 연산자 [`as`](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)와 함께 변환 자료형을 기입하여 lvalue로 전달한다. 암시적 자료형을 시도할 경우, 러스트는 자료형 불일치라는 명목으로 컴파일 오류를 알린다.

    ```rust
    let variable = 'A';
    println!("The ascii value of the variable is {}", variable as u8);
    ```

### 배열
**[배열](https://doc.rust-lang.org/book/ch03-02-data-types.html#the-array-type)**(array)은 동일한 [자료형](#자료형)의 데이터를 일련의 순서로 담는 시퀀스 자료형이다. 배열을 선언할 때 자료형을 지정하는 구문에 대괄호 `[]`를 활용하여 데이터 유형 및 용량 크기를 ([정수 리터럴](https://doc.rust-lang.org/reference/tokens.html#numbers)이나 [상수](#변수)로) 기입하며, 한 번 정의된 크기는 변경이 불가하다.

```rust
let arr : [u8 ; 3] = [value1, value2, value3];
```

배열 초기화만으로 러스트 컴파일러는 배열의 자료형과 크기를 추론할 수 있으므로 생략 가능하다. 초기화 방법은 위의 예시를 포함한 두 가지 방법이 존재한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 배열 초기화 방식</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">개별적 요소 초기화</th><th style="text-align: center;">반복적 요소 초기화</th></tr></thead><tbody><tr><td>

```rust
let arr = [value1, value2, value3];
```
</td><td>

```rust
let arr = [0; 3]; // 값 0을 세 번 연속 초기화
```
</td></tr></tbody></table>

* 메모리 접근 안정성이 부각되는 러스트 언어의 특성상, 배열 초기화는 요소 개수를 초과하거나 미달해서는 컴파일러 오류가 발생한다.

배열의 각 요소에 할당된 데이터는 대괄호 `[]`를 사용해 0번부터 시작하는 인덱스 위치를 호출할 수 있다. 만일 `mut` 키워드를 추가하면 각 요소의 데이터를 변경할 수 있는 가변 배열로 선언된다.

```rust
let mut arr = [value1, value2, value3];

println!("{}", arr[0]);     // 출력: value1
println!("{}", arr[1]);     // 출력: value2

arr[1] = value4;
println!("{:?}", arr[1]);   // 출력: [value1, value4, value3]
```

배열은 또 다른 동일한 자료형과 크기의 배열들을 요소로 가질 수 있으며, 이를 다차원 배열이라고 부른다.

```rust
let arr : [[u8; 3]; 2] = [[1, 2, 3],
                          [4, 5, 6]];

println!("{}", arr[0][1]);  // 출력: 2
println!("{}", arr[1][2]);  // 출력: 6
```

### 튜플
**[튜플](https://doc.rust-lang.org/book/ch03-02-data-types.html#the-tuple-type)**(tuple)은 다양한 [자료형](#자료형)의 데이터를 일련의 순서로 담는 시퀀스 자료형이다. 튜플을 선언할 때 자료형을 지정하는 구문에 소괄호 `()`를 활용하여 데이터 유형을 순서대로 기입하며, 한 번 정의된 튜플 크기와 자료형 순서는 변경이 불가하다.

```rust
let tuple : (i32, char, bool) = (3, 'A', true);
```

튜플 초기화만으로 러스트 컴파일러는 튜플의 자료형과 크기를 추론할 수 있으므로 생략 가능하다.

튜플의 각 요소에 할당된 데이터는 맴버 접근 연산자 `.`를 사용해 0번부터 시작하는 인덱스 위치를 호출할 수 있다. 만일 `mut` 키워드를 추가하면 각 요소의 데이터를 변경할 수 있는 가변 튜플로 선언된다.

```rust
let mut tuple = (3, 'A', true);

println!("{}", tuple.0);    // 출력: 3
println!("{}", tuple.1);    // 출력: A

tuple.1 = 'B';
println!("{:?}", tuple);    // 출력: (3, 'B', true)
```

## 변수
**[변수](https://doc.rust-lang.org/stable/book/ch03-01-variables-and-mutability.html)**(variable)는 [네임 바인딩](https://en.wikipedia.org/wiki/Name_binding) 기법을 통해 지정된 [자료형](#자료형)의 데이터와 엮여 접근하는 데 사용되는 [식별자](#식별자)이며, 간단히 설명하자면 데이터에 이름을 붙인 것이다. 아래 코드는 `variable` 변수를 "선언(declaration)"한 다음 상수 3을 바인딩한다. 여기서 최초 바인딩을 "초기화(initialization)"라고 부르며, 초기화되지 않은 채 변수 호출은 허용되지 않지만 선언된 이후 나중에 바인딩될 수 있다.<sup>[<a href="https://doc.rust-lang.org/rust-by-example/variable_bindings/declare.html">출처</a>]</sup>

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption>러스트의 불변 및 가변 변수, 그리고 키워드 소개</caption><colgroup><col style="width: 50%;"/></col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">불변(immutable) 변수</th><th style="text-align: center;">가변(mutable) 변수</th></tr></thead><tbody><tr style="vertical-align: top;"><td>

```rust
let variable = 3;
```
</td><td>

```rust
let mut variable = 3;
```
</td></tr><tr><td>초기화 이후에 변수는 다른 값으로 변경될 수 없으며, 이는 러스트의 기본적인 특성 중 하나이다.</td><td>타 프로그래밍 언어처럼 초기화가 이루어진 이후에도 <a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#assignment-expressions">할당 표현식</a>을 통해 새로운 값을 전달 받을 수 있도록 선언된 변수이다.</td></tr><tr><td><ul><li><a href="https://doc.rust-lang.org/std/keyword.let.html"><code>let</code></a>: 현 코드 <a href="https://doc.rust-lang.org/rust-by-example/variable_bindings/scope.html">영역범위</a> 내에 새로운 변수를 소개하는 데 사용된다.</li><li><a href="https://doc.rust-lang.org/std/keyword.const.html"><code>const</code></a>: 어디서나 사용할 수 있는 불변의 전역 상수이며, 반드시 자료형과 함께 선언되어야 한다.</li><li><a href="https://doc.rust-lang.org/std/keyword.static.html"><code>static</code></a>: 메모리 주소를 나타내기 때문에, 이를 참조하면 데이터 변경이 가능하여 사실상 전역 변수로 사용된다.</li><ul></td><td><ul><li><a href="https://doc.rust-lang.org/std/keyword.mut.html"><code>mut</code></a>: 다른 데이터에 바인딩할 수 있는 가변 변수를 선언하는 목적으로도 활용되며, <code>let</code> 및 <code>static</code> 키워드와 함께 사용될 수 있다.</li><ul></td></tr></tbody></table>

컴파일러는 데이터의 값과 사용처를 기반하여 변수의 자료형을 추론할 수 있으나, 다음과 같이 명시적으로 변수에 자료형을 지정할 수 있다.

```rust
let variable: u8 = 3;
```

거의 모든 프로그래밍 언어는 할당 기호를 기준으로 왼쪽에는 피할당자(변수), 오른쪽에는 피할당자로 전달하려는 표현식(값 혹은 데이터)이 위치한다. 반대로 놓여질 경우, 오류가 발생하거나 원치 않는 결과가 도출될 수 있다.

### 네임스페이스
**[네임스페이스](https://doc.rust-lang.org/reference/names/namespaces.html)**(namespace)는 선언된 식별자들 간 중복된 이름으로 충돌이 일어나는 걸 방지하기 위한 [영역](https://en.wikipedia.org/wiki/Scope_(computer_science))(scope)을 설정하여 데이터를 분류하는 논리 공간이다. 

## 제어 흐름식
**[제어 흐름식](https://doc.rust-lang.org/book/ch03-05-control-flow.html)**(control flow expression)은 코드 실행을 제어하는 표현식을 가리키며, 프로그래밍에 있어 기초적이면서 가장 흔히 사용되는 코드 유형 중 하나이다. 아래는 Rust에서 지원하는 제어 흐름식으로, 부가 설명이 필요한 제어문의 경우 본 문서에 별도로 소개한다.

* [`if`](https://doc.rust-lang.org/reference/expressions/if-expr.html) <sub>(파생 패턴: [`if let`](https://doc.rust-lang.org/reference/expressions/if-expr.html#if-let-patterns))</sub>
* [`match`](#match-표현식)
* [`loop`](#loop-표현식)
* [`while`](https://en.cppreference.com/w/c/language/while.html) <sub>(파생 패턴: [`while let`](https://en.cppreference.com/w/c/language/do.html))</sub>
* [`for`](https://doc.rust-lang.org/reference/expressions/loop-expr.html#continue-expressions)
* [`break`](https://doc.rust-lang.org/reference/expressions/loop-expr.html#break-expressions)
* [`continue`](https://doc.rust-lang.org/reference/expressions/loop-expr.html#continue-expressions)
* [`return`](https://doc.rust-lang.org/reference/expressions/return-expr.html)

### `match` 표현식
[`match`](https://doc.rust-lang.org/reference/expressions/match-expr.html) 표현식은 전달받은 인자를 쉼표로 구분된 match arms들은 위에서부터 순서대로 [패턴 구문](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html)에 따른 일치 여부를 비교하여, 참일 경우 해당 지점부터 코드를 실행하고 거짓일 경우에는 순서로 넘어간다. 선택사항으로 마지막 `_` match arm은 어떠한 경우에도 부합하지 않을 때 실행되는 패턴이다.

```rust
match argument {
    match_arm => expression,
    match_arm => expression,
    _ => expression
}
```

* C/C++ 언어에서 소개되는 [`switch`](C.md#switch-선택문) 선택문과 유사하지만 더 범용적인 "패턴 일치" 여부를 확인하는 점에 차이가 있다.

### `loop` 표현식
[`loop`](https://doc.rust-lang.org/reference/expressions/loop-expr.html) 표현식은 블록 안에 정의된 코드를 무조건 반복 실행하며, 이를 중단하려면 `break` 표현식으로 탈출해야 한다.

```rust
loop {
    expressions
}
```

`loop` 표현식은 선택사항으로 [레이블](https://doc.rust-lang.org/reference/expressions/loop-expr.html#loop-labels)을 지닐 수 있으며, `break` 및 `continue` 표현식에 해당 레이블을 기입하여 어떤 반복 표현식이 대상인지 지정할 수 있다.

```rust
'outer: loop {
    while true {
        break 'outer;
    }
}
```

## 형식 데이터
> *참고: [Formatted print - Rust By Example](https://doc.rust-lang.org/rust-by-example/hello/print.html)*

다음은 지정된 형식에 따른 데이터 처리에 관여하는 러스트의 매크로들을 소개한다.

* **콘솔 출력**

    지정된 형식대로 텍스트를 콘솔에 나타나도록 출력하는 데 사용된다.

    <table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 출력 매크로</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">출력 매크로</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.print.html"><code>print!</code></a></td><td>주어진 형식 지정자에 따라 텍스트를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.println.html"><code>println!</code></a></td><td>주어진 형식 지정자에 따라 텍스트를 터미널에 출력하며 <code>'\n'</code> 줄바꿈이 기본적으로 보장된다.
    
    ```rust
    println!("Format: {:?}", 5);
    ```
    </td></tr></tbody></table>

* **버퍼 입력**

    지정된 형식대로 텍스트를 버퍼로 전달하도록 입력하는 데 사용된다. 유의할 점으로, 본 매크로는 콘솔창의 사용자 텍스트를 입력으로 받는 매크로가 절대 아니다. 핵심 입출력 기능 및 특성을 제공하는 [`std::io`](https://doc.rust-lang.org/std/io/)의 [`Write`](https://doc.rust-lang.org/std/io/trait.Write.html) 특성이 요구된다.

    <table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 입력 매크로</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">입력 매크로</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.write.html"><code>write!</code></a></td><td>주어진 형식 지정자 따른 텍스트를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.writeln.html"><code>writeln!</code></a></td><td>주어진 형식 지정자 따른 텍스트를 터미널에 출력하며 <code>'\n'</code> 줄바꿈이 기본적으로 보장된다.

    ```rust
    use std::io::Write;
    writeln!(&mut buffer, "Output: {:?}", 5);
    ```
    </td></tr></tbody></table>

텍스트 안에 포함된 중괄호 `{}`에는 인자로 전달된 데이터가 삽입 및 서식된다. 특히 예시에 기입된 [`{:?}`](https://doc.rust-lang.org/std/fmt/trait.Debug.html)은 디버깅 목적으로 실사용에 적용하기 불안정하지만 프로그램 코드를 이해하는 데 도움을 준다. 자세한 내용은 [`std::fmt`](https://doc.rust-lang.org/std/fmt/index.html) 모듈을 참고하도록 한다.

# 소유권
> *참고: [What is Ownership? - The Rust Programming Language](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html)*

러스트 언어가 타 프로그래밍 언어에 비해 어려운 이유 중 하나는 바로 메모리 관리를 소유권(ownership)이란 새로운 개념을 도입하였기 때문이다. 본 장은 소유권의 개념과 동작을 소개한다.

# 모듈
> *참고: [Defining Modules to Control Scope and Privacy - The Rust Programming Language](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html)*

[모듈](https://doc.rust-lang.org/rust-by-example/mod.html)(module)은 함수, 구조체, 심지어 타 모듈을 포함한 코드 항목들을 논리 단위로 나눈 집합체이다.
