# 러스트
> *출처: [The Rust Programming Language <sub>(영문)</sub>](https://doc.rust-lang.org/book/)*

[러스트](https://www.rust-lang.org/)(Rust)는 성능과 보안이 매우 강조된 [정적 정형](https://en.wikipedia.org/wiki/Type_system#Static_type_checking) 프로그래밍 언어이며, 특히 자료형 및 메모리 관리 측에서 강점을 지닌다. 뿐만 아니라, 하드웨어 제어가 가능하다는 점에서 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 개발에도 활용할 수 있다. 이러한 특성들에 의해 [C++](Cpp.md) 언어와 유사한 성능을 지닌 동시에 훌륭한 보안성이 보장되어 [마이크로소프트](https://www.microsoft.com/), [구글](https://www.google.com/), [아마존](https://www.amazon.com/) 등의 주요 IT 기업의 관심과 지원을 받고 있다.

## 설치
러스트 프로그래밍 언어를 실행하는 데 필요한 [툴체인](https://ko.wikipedia.org/wiki/툴체인)은 공식 홈페이지에서 rustup_init.exe를 [다운로드](https://www.rust-lang.org/tools/install) 받아 설치한다.

> 설치 과정에서 2013 버전 이상의 [비주얼 스튜디오](https://visualstudio.microsoft.com/downloads/)이 요구되는 데, 미리 Desktop development with C++ 그리고 윈도우 10 또는 11 SDK를 준비하도록 한다.

![<code>rustup_init.exe</code> 시작 화면](./images/rustup_init_setup.png)

여기서 러스트 프로그래밍 언어의 툴체인(toolchain)이란, 소프트웨어 제품 개발에 활용되는 프로그래밍 도구들의 집합을 가리키며 대표적으로 다음 세 개의 명령어 프로그램이 있다.

1. `rustc`: 러스트 [컴파일러](Compiler.md)(compiler)
1. [`cargo`](#프로젝트): 러스트 패키지 관리자; 프로젝트를 컴파일 및 실행한다.
1. `rustup`: 러스트 툴체인 관리자, 즉 위의 `rustc` 및 `cargo` 프로그램을 다른 버전으로 설치 및 선택한다.

설치가 완료되면 `%UserProfile%` 디렉토리에 `.rustup` 및 `.cargo` 폴더가 생성된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트 관련 디렉토리 소개</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">디렉토리</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>.rustup</code></td><td><code>rustc</code>, <code>cargo</code> 등을 포함한 러스트 툴체인이 실질적으로 위치하는 디렉토리이며, <code>toolchains</code> 하위폴더에 버전마다 나뉘어 설치된다.</td></tr><tr><td style="text-align: center;"><code>.cargo</code></td><td>설치된 러스트 툴체인에 <a href="https://en.wikipedia.org/wiki/Hard_link">링크</a>된 <a href="https://en.wikipedia.org/wiki/Binary_file">바이너리 파일</a>이 위치하여, 개발자는 러스트의 전반적인 환경 구성을 그대로 유지한 채 <code>rustup</code>에서 선택한 버전으로 변경하여 사용할 수 있다.</td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><i><sub>† <a href="Python.md">파이썬</a>에 빗대어 설명하자면 전자는 실제 파이썬 <a href="Interpreter.md">인터프리터</a>가, 후자는 Py Launcher가 위치한 폴더에 해당한다.</sub></i></caption></table>

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

# 구문
[구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)은 프로그래밍 언어에서 문자 및 기호들의 조합이 올바른 문장 또는 표현식을 구성하였는지 정의하는 규칙이다. 각 프로그래밍 언어마다 규정하는 구문이 다르며, 이를 준수하지 않을 시 해당 프로그램은 빌드되지 않거나, 실행이 되어도 오류 및 의도치 않은 동작을 수행한다.

다음은 러스트 프로그래밍 언어에서 구문에 관여하는 요소들을 소개한다:

* **[표현식](https://en.wikipedia.org/wiki/Expression_(computer_science))(expression)**
    
    값을 반환하는 구문적 존재를 가리킨다. 표현식에 대한 결과를 도출하는 것을 평가(evaluate)라고 부른다.
    
    ```rust
    2 + 3           // 숫자 5를 반환
    2 < 3           // 논리 참을 반환
    ```

* **[토큰](https://doc.rust-lang.org/reference/tokens.html)(token)**

    표현식을 구성하는 가장 기본적인 요소이며, 대표적으로 [키워드](https://doc.rust-lang.org/reference/keywords.html)(keyword), [식별자](#식별자)(identifier)와 [리터럴](https://doc.rust-lang.org/reference/tokens.html#literals)(literal) 등이 있다.

    ```rust
    variable        // 식별자
    2               // 정수 리터럴
    ```

* **[문장](https://en.wikipedia.org/wiki/Statement_(computer_science))(statement)**
    
    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 러스트 프로그래밍 언어는 [세미콜론](https://ko.wikipedia.org/wiki/새줄_문자)(semicolon) `;`을 기준으로 문장을 분별한다. 

    ```rust
    let variable = 2 + 3;      // 숫자 5를 "variable" 변수에 초기화
    if 2 < 3 { statement; }    // 논리가 참이면 "statement" 문장 실행
    ```

* **[블록](https://en.wikipedia.org/wiki/Block_(programming))(block)**

    한 개 이상의 문장들을 한꺼번에 관리할 수 있도록 묶어놓은 소스 코드상 그룹이다. 블록 안에 또 다른 블록이 상주할 수 있으며, 이를 네스티드 블록(nested block)이라고 부른다. 러스트에서는 한 쌍의 중괄호 `{}`로 표시된다.

    ```rust
    {
        let variable = 2 + 3;
        if 2 < 3 { statement; }
    }
    ```

### 식별자
[식별자](https://doc.rust-lang.org/reference/identifiers.html)(identifier)는 프로그램을 구성하는 데이터들을 구별하기 위해 사용되는 명칭이다. 즉, 식별자는 개발자가 데이터에 직접 붙여준 이름이다. 러스트 프로그래밍 언어에서 식별자는 [유니코드 표준 부록 #31](https://www.unicode.org/reports/tr31/tr31-37.html)을 준수하며, 간단히 설명하면 다음과 같다.

1. 알파벳, 숫자, 밑줄 `_`만 허용 (그 외 특수문자 및 공백 사용 불가)
1. 식별자의 첫 문자는 숫자가 될 수 없음
1. 대소문자 구분 필수
1. [예약어](https://doc.rust-lang.org/book/appendix-01-keywords.html) 금지

### 주석
[주석](https://doc.rust-lang.org/reference/comments.html)(comment)은 프로그램의 소스 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데, 크게 비문서 주석 그리고 문서 주석로 나뉘어진다.

<table style="table-layout: fixed; width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트 주석 종류</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">비문서 주석(non-doc comments)</th><th style="text-align: center;">문서 주석(doc comments)</th></tr></thead><tbody><tr><td>일반적인 <a href="C.md">C</a>/<a href="C.md">C++</a> 언어 형식의 <a href="C.md#주석">주석</a>과 동일하다.</td><td>문서 주석은 한줄(<code>///</code>) 그리고 블록(<code>/** */</code>)으로 입력할 수 있으며, 데이터에 대한 간략한 설명을 기입하는 데 사용된다.</td></tr><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```rust
/*
블록 주석:
코드 여러 줄을 차지하는 주석이다.
*/

// 한줄 주석: 코드 한 줄을 차지하는 주석이다.
```
</td><td>

```rust
/**
블록 문서 주석:
코드 여러 줄을 차지하는 주석이다.
*/

/// 한줄 문서 주석: 코드 한 줄을 차지하는 주석이다.
```
</td></tr></tbody></table>

일반 주석과 달리, 사용자가 정의한 데이터에 기입된 문서 주석은 `doc`이란 특수한 속성에 저장되어 정의된 데이터에 대한 설명을 정의한 소스 코드를 찾아가지 않고서도 곧바로 내용을 살펴볼 수 있다.

![VS Code에서의 문서 주석 활용 예시](./images/rust_doccomments.png)

## 형식 데이터
> *참고: [Formatted print - Rust By Example](https://doc.rust-lang.org/rust-by-example/hello/print.html)*

다음은 지정된 형식에 따른 데이터 처리에 관여하는 러스트의 매크로들을 소개한다.

* **콘솔 출력**

    지정된 형식대로 텍스트를 콘솔에 나타나도록 출력하는 데 사용된다.

    <table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 출력 매크로</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">출력 매크로</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.print.html"><code>print!</code></a></td><td>주어진 형식 지정자에 따라 텍스트를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.println.html"><code>println!</code></a></td><td>주어진 형식 지정자에 따라 텍스트를 터미널에 출력하며 <code>'\n'</code> 줄바꿈이 기본적으로 보장된다.
    
    ```rust
    println!("Format: {:b}", 5);
    ```
    </td></tr></tbody></table>

* **버퍼 입력**

    지정된 형식대로 텍스트를 버퍼로 전달하도록 입력하는 데 사용된다. 유의할 점으로, 본 매크로는 콘솔창의 사용자 텍스트를 입력으로 받는 매크로가 절대 아니다. 핵심 입출력 기능 및 특성을 제공하는 [`std::io`](https://doc.rust-lang.org/std/io/)의 [`Write`](https://doc.rust-lang.org/std/io/trait.Write.html) 특성이 요구된다.

    <table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 입력 매크로</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">입력 매크로</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.write.html"><code>write!</code></a></td><td>주어진 형식 지정자 따른 텍스트를 터미널에 출력한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/macro.writeln.html"><code>writeln!</code></a></td><td>주어진 형식 지정자 따른 텍스트를 터미널에 출력하며 <code>'\n'</code> 줄바꿈이 기본적으로 보장된다.

    ```rust
    use std::io::Write;
    writeln!(&mut buffer, "Output: {:b}", 5);
    ```
    </td></tr></tbody></table>

### 탈출 문자
[탈출 문자](https://en.wikipedia.org/wiki/Escape_character)(escape character)는 백슬래시 기호 `\`를 사용하며, [문자열](#문자열)로부터 탈출하여 텍스트 데이터 내에서 특정 연산을 수행하도록 한다. 예시에서 `\n` 탈출 문자를 사용하여 문자열 줄바꿈을 구현한 것을 보여주었다.

```rust
println!("Hello,\nWorld!");
```
```terminal
Hello,
World!
```

## 자료형
[자료형](https://doc.rust-lang.org/book/ch03-02-data-types.html)(data type)은 데이터를 어떻게 표현할 지 결정하는 요소이며, 러스트에서는 다음과 같이 존재한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">러스트의 <a href="https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#scalar-types">원시 스칼라 자료형</a></caption><colgroup><col style="width: 20%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">키워드</th><th style="text-align: center;">자료형</th><th style="text-align: center;">크기 (바이트)</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>i8</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u8</code>)</i></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/numeric.html">숫자</a></td><td style="text-align: center;">1</td><td>1바이트 정수</td></tr><tr><td style="text-align: center;"><code>i16</code><br/><i>(unsigned: <code style="font-style: normal;">u16</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">2</td><td>2바이트 정수</td></tr><tr><td style="text-align: center;"><code>i32</code><br/><i>(unsigned: <code style="font-style: normal;">u32</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>4바이트 정수</td></tr><tr><td style="text-align: center;"><code>i64</code><br/><i>(unsigned: <code style="font-style: normal;">u64</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>8바이트 정수</td></tr><tr><td style="text-align: center;"><code>i128</code><br/><i>(unsigned: <code style="font-style: normal;">u128</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>16바이트 정수</td></tr><tr><td style="text-align: center;"><code>isize</code><br/><i>(unsigned: <code style="font-style: normal;">usize</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16 <sub>(최소)</sub></td><td><a href="#포인터">포인터</a> 크기의 정수; 예를 들어 x86는 32비트, 그리고 x64는 64비트 정수로 계산된다.</td></tr><tr><td style="text-align: center;"><code>f32</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>32비트 <a href="https://en.wikipedia.org/wiki/Single-precision_floating-point_format">단정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f64</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>64비트 <a href="https://en.wikipedia.org/wiki/Double-precision_floating-point_format">배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f128</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>128비트 <a href="https://en.wikipedia.org/wiki/Quadruple-precision_floating-point_format">4배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>bool</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/boolean.html">논리</a></td><td style="text-align: center;">1</td><td>참(<code>true</code>; 1) 혹은 거짓(<code>false</code>; 0)</td></tr><tr><td style="text-align: center;"><code>char</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/textual.html">텍스트</a></td><td style="text-align: center;">4</td><td>단일 <a href="https://en.wikipedia.org/wiki/UTF-32">UTF-32</a> 문자: 0x0000-0xD7FF 또는 0xE000-0x10FFFF</td></tr></tbody/></table>

> [바이트](https://ko.wikipedia.org/wiki/바이트)(byte)란, 컴퓨터에서 메모리에 저장하는 가장 기본적인 단위이다. 자료형마다 크기가 정해진 이유는 효율적인 메모리 관리 차원도 있으나 CPU 연산과도 깊은 연관성을 갖는다. 한 바이트는 여덟 개의 [비트](https://ko.wikipedia.org/wiki/비트_(단위))(bit)로 구성된다.

여기서 [unsigned](https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int)란, 자료형 중에서 [최상위 비트](https://ko.wikipedia.org/wiki/최상위_비트)를 정수의 [부호](https://ko.wikipedia.org/wiki/Signed와_unsigned)를 결정하는 요소로 사용하지 않는다. 아래의 16비트 정수형인 `i16`는 원래 최상위 비트를 제외한 나머지 15개의 비트로 정수를 표현한다. 반면 unsigned 자료형인 `u16`은 음의 정수를 나타낼 수 없지만, 16개의 비트로 양의 정수를 더 많이 표현할 수 있다.

```rust
i16             // 표현 가능 범위: -32768 ~ +32767
u16             // 표현 가능 범위:     +0 ~ +65535
```

그 외의 자료형들은 차후 러스트 프로그래밍 언어에 대한 개념을 소개하면서 함께 설명할 예정이다.

### 자료형 변환
[자료형 변환](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)(type conversion)은 데이터를 다른 자료형으로 바꾸는 작업이며, 불가피하게 데이터가 손실될 수 있으므로 유의하도록 한다. 러스트는 일반적으로 [강제 변환](https://doc.rust-lang.org/reference/type-coercions.html)(coercion)이란 암시적 자료형 변환을 지원하지 않으며, 자동 변환은 오로지 특정 위치의 코드에서 제한된 자료형에만 국한된다.

* **[자료형 캐스팅](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)**(type casting)

    명시적 자료형 변환에 해당하며, 데이터의 우변에 이진 연산자 [`as`](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)와 함께 변환 자료형을 기입하여 lvalue로 전달한다. 암시적 자료형을 시도할 경우, 러스트는 자료형 불일치라는 명목으로 컴파일 오류를 알린다.

    ```rust
    let variable = 'A';
    println!("The ascii value of the variable is {}", variable as u8);
    ```

### `size_of` 함수
[`size_of`](https://doc.rust-lang.org/std/mem/fn.size_of.html) [함수](#함수)는 자료형의 메모리 크기를 바이트로 반환한다. 정확한 설명은 "배열의 다음 요소 간 오프셋"을 반환하며, [데이터 정렬](C.md#데이터-구조-정렬)에 삽입된 패딩도 포함한다.

* [`size_of_val`](https://doc.rust-lang.org/std/mem/fn.size_of_val.html): (자료형을 알 수 없는 경우) 참조된 값의 메모리 할당 크기를 동적으로 확인하여 반환한다.

```rust
std::mem::size_of::<char>();        // 크기: 4바이트

let variable: u128 = 0;
std::mem::size_of_val(&variable);   // 크기: 16바이트
```

## 변수
[변수](https://doc.rust-lang.org/stable/book/ch03-01-variables-and-mutability.html)(variable)는 [네임 바인딩](https://en.wikipedia.org/wiki/Name_binding) 기법을 통해 지정된 [자료형](#자료형)의 데이터와 엮여 접근하는 데 사용되는 [식별자](#식별자)이며, 간단히 설명하자면 데이터에 이름을 붙인 것이다. 아래 코드는 `variable` 변수를 "선언(declaration)"한 다음 상수 3을 바인딩한다. 여기서 최초 바인딩을 "초기화(initialization)"라고 부르며, 초기화되지 않은 채 변수 호출은 허용되지 않지만 선언된 이후 나중에 바인딩될 수 있다.<sup>[<a href="https://doc.rust-lang.org/rust-by-example/variable_bindings/declare.html">출처</a>]</sup>

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

## 연산자
[연산자](https://en.wikipedia.org/wiki/Operator_(computer_programming))(operator)는 피연산 데이터를 조작할 수 있는 가장 간단한 형태의 연산 요소이다. 연산자는 피연산자의 접두부, 접미부, 혹은 두 데이터 사이에 위치시켜 사용한다. 가독성을 위해 데이터와 연산자 사이에 공백을 넣어도 연산에는 아무런 영향을 주지 않는다. 다음은 [러스트 연산자](https://doc.rust-lang.org/book/appendix-02-operators.html)들을 간략히 소개한다.

### 산술 연산자
<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#arithmetic-and-logical-binary-operators">산술 연산자</a>(arithmetic operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">산술</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Add.html"><code>+</code></a></td><td style="text-align: center;">덧셈</td><td>좌측과 우측 피연산자의 값을 더하여 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Sub.html"><code>-</code></a></td><td style="text-align: center;">뺄셈</td><td>좌측 피연산자에서 우측 피연산자를 뺀 값을 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Mul.html"><code>*</code></a></td><td style="text-align: center;">덧셈</td><td>좌측 피연산자를 우측 피연산자의 값만큼 곱하여, 즉 반복 덧셈하여 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Div.html"><code>/</code></a></td><td style="text-align: center;">나눗셈</td><td>좌측 피연산자에서 우측 피연산자를 나눈 <a href="https://en.wikipedia.org/wiki/Quotient">몫</a>을 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Rem.html"><code>%</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Modular_arithmetic">모듈러</a></td><td>좌측 피연산자에서 우측 피연산자를 나눈 <a href="https://en.wikipedia.org/wiki/Remainder">나머지</a>를 반환한다.</td></tr></tbody></table>

### 비트 연산자
<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation">비트 연산자</a>(bitwise operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">비트연산</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.BitAnd.html"><code>&</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#AND">AND</a></td><td>두 피연산자의 각 비트를 비교하여 모두 1이면 1을, 아니면 0을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.BitOr.html"><code>|</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#OR">OR</a></td><td>두 피연산자의 각 비트를 비교하여 하나라도 1이 있으면 1을, 아니면 0을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.BitXor.html"><code>^</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#XOR">XOR</a></td><td>두 피연산자의 각 비트를 비교하여 값이 같으면 0을, 다르면 1을 계산하여 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Not.html"><code>!</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operation#NOT">NOT</a></td><td>피연산자의 각 비트마다 반전시킨 값을 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Shl.html"><code>&lt;&lt;</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operations_in_C#Left_shift_%3C%3C">좌향 시프트</a></td><td>피연산자(左)의 비트를 전반적으로 일정 값(右)만큼 왼쪽으로 이동시킨다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.Shr.html"><code>&gt;&gt;</code></a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Bitwise_operations_in_C#Right_shift_%3E%3E">우향 시프트</a></td><td>피연산자(左)의 비트를 전반적으로 일정 값(右)만큼 오른쪽으로 이동시킨다.</td></tr></tbody></table>

### 할당 연산자
단순 할당 연산자를 산술 및 비트 연산자와 조합하여 코드를 더욱 간결하게 작성할 수 있으며, 아래는 다양한 할당 연산자 중 일부만 보여준다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#compound-assignment-expressions">할당 연산자</a>(assignment operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">할당</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#basic-assignments"><code>=</code></a></td><td style="text-align: center;">단순 할당</td><td>피연산자(右)가 <a href="#변수">변수</a>와 같은 피할당자(左)로 할당된 값을 반환한다.</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.AddAssign.html"><code>+=</code></a></td><td style="text-align: center;">덧셈 대입</td><td>

```rust
x += y;  // 동일: x = x + y;
```
</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.MulAssign.html"><code>*=</code></a></td><td style="text-align: center;">곱셈 대입</td><td>

```rust
x *= y;  // 동일: x = x * y;
```
</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.BitAndAssign.html"><code>&=</code></a></td><td style="text-align: center;">AND 대입</td><td>

```rust
x &= y;  // 동일: x = x & y;
```
</td></tr><tr><td style="text-align: center;"><a href="https://doc.rust-lang.org/std/ops/trait.ShlAssign.html"><code>&lt;&lt;=</code></a></td><td style="text-align: center;">좌향 시프트 대입</td><td>

```rust
x <<= y;  // 동일: x = x << y;
```
</td></tr></tbody></table>

### 비교 연산자
아래 비교 연산자의 설명은 참을 반환할 조건을 소개하며, 그 외에는 모두 `false`를 반환한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#comparison-operators">비교 연산자</a>(comparison operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">관계</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>&gt;</code></td><td style="text-align: center;">초과</td><td>좌측 피연산자가 우측 피연산자보다 크면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>&lt;</code></td><td style="text-align: center;">미만</td><td>좌측 피연산자가 우측 피연산자보다 작으면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>&gt;=</code></td><td style="text-align: center;">이상</td><td>좌측 피연산자가 우측 피연산자보다 크거나 같으면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>&lt;=</code></td><td style="text-align: center;">이하</td><td>좌측 피연산자가 우측 피연산자보다 작거나 같으면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>==</code></td><td style="text-align: center;">동일</td><td>두 피연산자의 값이 같으면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>!=</code></td><td style="text-align: center;">상이</td><td>두 피연산자의 값이 같지 않으면 <code>true</code>를 반환한다.</td></tr></tbody></table>

### 논리 연산자
(논리 부정을 제외한) 아래 논리 연산자의 설명은 참을 반환할 조건을 소개하며, 그 외에는 모두 `false`를 반환한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;"><a href="https://doc.rust-lang.org/reference/expressions/operator-expr.html#lazy-boolean-operators">논리 연산자</a>(logical operators)</caption><colgroup><col style="width: 10%;"/><col style="width: 15%;"/><col style="width: 75%;"/><col style="width: "/></colgroup><thead><tr><th style="text-align: center;">연산자</th><th style="text-align: center;">논리</th><th style="text-align: center;">설명 </th></tr></thead><tbody><tr><td style="text-align: center;"><code>&&</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Logical_conjunction">논리곱</a></td><td>좌측 그리고 우측 <a href="https://en.wikipedia.org/wiki/Proposition">명제</a>(피연산자)가 모두 참이면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>||</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Logical_disjunction">논리합</a></td><td>좌측 또는 우측 명제(피연산자)가 하나라도 참이면 <code>true</code>를 반환한다.</td></tr><tr><td style="text-align: center;"><code>!</code></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Negation">부정</a></td><td>명제(피연산자)가 참이면 거짓으로, 혹은 그 반대로 반전된 값을 반환한다.</td></tr></tbody></table>

# 모듈
> *참고: [Defining Modules to Control Scope and Privacy - The Rust Programming Language](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html)*

[모듈](https://doc.rust-lang.org/rust-by-example/mod.html)(module)은 함수, 구조체, 심지어 타 모듈을 포함한 코드 항목들을 논리 단위로 나눈 집합체이다.
