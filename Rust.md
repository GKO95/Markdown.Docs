# 러스트
> *출처: [The Rust Programming Language <sub>(영문)</sub>](https://doc.rust-lang.org/book/)*

[러스트](https://www.rust-lang.org/)(Rust)는 성능과 보안이 매우 강조된 프로그래밍 언어이며, 특히 자료형 및 메모리 관리 측에서 강점을 지닌다. 뿐만 아니라, 하드웨어 제어가 가능하다는 점에서 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 개발에도 활용할 수 있다. 이러한 특성들에 의해 [C++](Cpp.md) 언어와 유사한 성능을 지닌 동시에 훌륭한 보안성이 보장되어 [마이크로소프트](https://www.microsoft.com/), [구글](https://www.google.com/), [아마존](https://www.amazon.com/) 등의 주요 IT 기업의 관심과 지원을 받고 있다.

## 설치
러스트 프로그래밍 언어를 실행하는 데 필요한 [툴체인](https://ko.wikipedia.org/wiki/툴체인)은 공식 홈페이지에서 rustup_init.exe를 [다운로드](https://www.rust-lang.org/tools/install) 받아 설치한다.

> 설치 과정에서 2013 버전 이상의 [비주얼 스튜디오](https://visualstudio.microsoft.com/downloads/)이 요구되는 데, 미리 Desktop development with C++ 그리고 윈도우 10 또는 11 SDK를 준비하도록 한다.

![<code>rustup_init.exe</code> 시작 화면](./images/rustup_init_setup.png)

여기서 러스트 프로그래밍 언어의 툴체인(toolchain)이란, 소프트웨어 제품 개발에 활용되는 프로그래밍 도구들의 집합을 가리키며 대표적으로 다음 세 개의 명령어 프로그램이 있다.

1. `rustc`: 러스트 [컴파일러](Compiler.md)(compiler)
1. [`cargo`](#프로젝트): 러스트 패키지 관리자; 프로젝트를 컴파일 및 실행한다.
1. `rustup`: 러스트 툴체인 관리자, 즉 위의 `rustc` 및 `cargo` 프로그램을 다른 버전으로 설치 및 선택한다.

설치가 완료되면 `%UserProfile%` 디렉토리에 `.rustup` 및 `.cargo` 폴더가 생성된다.

<table style="width: 85%; margin: auto;"><caption style="caption-side: top;">러스트 관련 디렉토리 소개</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">디렉토리</th><th style="text-align: center;">설명</th></tr></thead><tbody>
<tr><td style="text-align: center;"><code>.rustup</code></td><td><code>rustc</code>, <code>cargo</code> 등을 포함한 러스트 툴체인이 실질적으로 위치하는 디렉토리이며, <code>toolchains</code> 하위폴더에 버전마다 나뉘어 설치된다.</td></tr><tr><td style="text-align: center;"><code>.cargo</code></td><td>설치된 러스트 툴체인에 <a href="https://en.wikipedia.org/wiki/Hard_link">링크</a>된 <a href="https://en.wikipedia.org/wiki/Binary_file">바이너리 파일</a>이 위치하여, 개발자는 러스트의 전반적인 환경 구성을 그대로 유지한 채 <code>rustup</code>에서 선택한 버전으로 변경하여 사용할 수 있다.</td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><sup>† <a href="Python.md">파이썬</a>에 빗대어 설명하자면 전자는 실제 파이썬 <a href="Interpreter.md">인터프리터</a>가, 후자는 Py Launcher가 위치한 폴더에 해당한다.</sup></caption></table>

### 통합 개발 환경
[통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)은 최소한 프로그래밍 언어의 소스 코드 편집, 프로그램 빌드, 그리고 디버깅 기능을 제공하는 소프트웨어 개발 프로그램이다. 툴체인은 러스트 코드를 컴퓨터가 실행할 수 있도록 하는 개발 도구이지만 러스트 코드 편집기는 아니다. 러스트 코드를 작성하고 프로그램으로 실행하여 문제가 발생하면 검토할 수 있는 IDE가 절대적으로 필요하다.

* [비주얼 스튜디오 코드](https://code.visualstudio.com/download), 일명 VS Code는 마이크로소프트에서 개발한 무료 소스 코드 편집기이다. 비록 기술적으로 IDE는 아니지만, [rust-analyzer 확장도구](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)를 설치하면 코드 자동완성, 자료형 정의, 구문 하이라이트 등의 기능들을 제공한다. 코드 디버깅을 하려면 아래를 함께 설치한다.

    * [마이크로소프트 C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) (윈도우 운영체제)
    * [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) (macOS 또는 리눅스 운영체제)

## 프로젝트
> *참고: [Rust with Visual Studio Code](https://code.visualstudio.com/docs/languages/rust)*

러스트 프로그래밍 언어는 [`cargo`](https://doc.rust-lang.org/rust-by-example/cargo.html)라는 공식 패키지 관리 도구를 통해 프로젝트를 관리한다.

![VS Code에서 러스트 프로그래밍의 프로젝트](./images/vscode_rust.png)

<table style="width: 70%; margin: auto;"><caption style="caption-side: top;"><code>cargo</code> 도구의 프로젝트 관리 명령</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">명령어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>cargo build</code></td><td>프로젝트 빌드</td></tr><tr><td style="text-align: center;"><code>cargo run</code></td><td>프로젝트 (빌드 및) 실행</td></tr><tr><td style="text-align: center;"><code>cargo clean</code></td><td>프로젝트 빌드 결과물 제거 및 정리</td></tr><tr><td style="text-align: center;"><code>cargo check</code></td><td>프로젝트 유효성 검사; 컴파일 생략으로 소모 시간을 단축하나 결과물 부재</td></tr></tbody></table>

VS Code 편집기에 rust-analyzer 확장도구를 사용하면 `main()` 진입점 위에 "▶ Run | Debug" CodeLens 표시가 나타난다. 이는 `cargo`를 대신하여 프로젝트를 컴파일 및 실행하는 데 활용될 수 있다. 그러나 `CTRL+F5` 또는 `F5` 단축키로 빌드 및 실행이 불가하므로, `.vscode` 폴더에 추가할 두 개의 JSON 파일을 공유한다.

```json
// filename: launch.json
// Allow VS Code to run or debug the source code with (CTRL+) F5 hotkey.
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Rust Launch",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${workspaceFolder}/target/debug/${workspaceFolderBasename}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "console": "integratedTerminal",
            "preLaunchTask": "rust: cargo build"
        }
    ]
}
```
```json
// filename: tasks.json
// Build the source code before running the source code.
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cargo",
			"command": "build",
			"problemMatcher": [
				"$rustc"
			],
			"group": "build",
            "label": "rust: cargo build"
		}
	]
}
```

러스트 프로그래밍은 기본적으로 디버그 모드에서 프로젝트를 빌드하고 실행하며, 컴파일 부산물들은 `.\target\debug` 폴더 안에 위치한다. 만일 정식 배포를 위해 최적화된 프로그램으로 빌드하기 위헤 명령어에 `--release` 플래그를 추가하면 `.\target\release` 폴더에 프로그램이 생성된다.

### 크레이트
[크레이트](https://doc.rust-lang.org/rust-by-example/crates.html)(crate; 화물상자)는 컴파일로 생성된 가장 작은 단위의 결과물이며, 간단히 말해 `.RS` 소스 파일(일명 크레이트 파일; crate file)로부터 생성된 실행 및 라이브러리 파일이다. 크레이트 파일 중에서 러스트 프로그래밍 컴파일의 근본이 되는 소스 파일을 크레이트 루트(crate root)이라고 부른다.

크레이트는 아래와 같이 두 유형으로 나뉘어진다:

<table style="width: 70%; margin: auto;"><caption style="caption-side: top;">이진 및 라이브러리 크레이트 비교</caption><colgroup><col style="width: 16%;"/><col style="width: 44%;"/><col style="width: 44%;"/></colgroup>
<thead><tr><th></th><th style="text-align: center;">이진 크레이트 (binary crate)</th><th style="text-align: center;">라이브러리 크레이트 (library crate)</th></tr></thead><tbody><td style="text-align: center;">설명</td><td style="text-align: center;"><code>.EXE</code> 실행 프로그램</td><td style="text-align: center;"><code>.RLIB</code> 라이브러리 파일</td></tbody><tbody><td style="text-align: center;">크레이트 루트</td><td style="text-align: center;"><code>src/main.rs</code></td><td style="text-align: center;"><code>src/lib.rs</code></td></tbody><tbody><td style="text-align: center;"><code>main</code> 진입점</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tbody></table>

러스트 프로그래밍에서 크레이트를 이야기하면 흔히 "라이브러리 크레이트"를 가리키며, 이는 일반 프로그래밍 언어에서의 "[라이브러리](C.md#라이브러리)"와 혼용되어 언급되기도 한다.

### 패키지
패키지(package)는 하나 이상의 [크레이트](#크레이트)를 다루는 [번들](https://en.wikipedia.org/wiki/Product_bundling#Software)(bundle)이다. 각 패키지는 여러 개의 이진 크레이트를 다룰 수 있지만, 라이브러리 크레이트는 오로지 하나만 가능하다. 패키지의 특징 중 하나는 [`Cargo.toml`](#cargotoml) 파일을 가지고 있다는 점인데, 이는 크레이트를 어떻게 빌드를 할 것인지 설명한다.

러스트 프로그래밍에서 프로젝트를 생성한다는 것이 바로 패키지를 가리키며, 새로운 패키지를 생성하려면 아래 명령어를 입력한다.

```terminal
cargo new <프로젝트명>
```

# 구문
[구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)은 프로그래밍 언어에서 문자 및 기호들의 조합이 올바른 문장 또는 표현식을 구성하였는지 정의하는 규칙이다. 각 프로그래밍 언어마다 규정하는 구문이 다르며, 이를 준수하지 않을 시 해당 프로그램은 빌드되지 않거나, 실행이 되어도 오류 및 의도치 않은 동작을 수행한다.

다음은 러스트 프로그래밍 언어에서 구문에 관여하는 요소들을 소개한다:

* **[표현식](https://ko.wikipedia.org/wiki/식_(프로그래밍))(expression)**
    
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

* **[문장](https://ko.wikipedia.org/wiki/문_(프로그래밍))(statement)**
    
    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 러스트 프로그래밍 언어는 [세미콜론](https://ko.wikipedia.org/wiki/새줄_문자)(semicolon) `;`을 기준으로 문장을 분별한다. 

    ```rust
    let variable = 2 + 3;      // 숫자 5를 "variable" 변수에 초기화
    if 2 < 3 { statement; }    // 논리가 참이면 "statement" 문장 실행
    ```

### 식별자
[식별자](https://doc.rust-lang.org/reference/identifiers.html)(identifier)는 프로그램을 구성하는 데이터들을 구별하기 위해 사용되는 명칭이다. 즉, 식별자는 개발자가 데이터에 직접 붙여준 이름이다. 러스트 프로그래밍 언어에서 식별자는 [유니코드 표준 부록 #31](https://www.unicode.org/reports/tr31/tr31-37.html)을 준수하며, 간단히 설명하면 다음과 같다.

1. 알파벳, 숫자, 밑줄 `_`만 허용 (그 외 특수문자 및 공백 사용 불가)
1. 식별자의 첫 문자는 숫자가 될 수 없음
1. 대소문자 구분 필수
1. [예약어](https://ko.wikipedia.org/wiki/예약어) 금지

### 주석
[주석](https://doc.rust-lang.org/reference/comments.html)(comment)은 프로그램의 소스 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데, 크게 비문서 주석 그리고 문서 주석로 나뉘어진다.

<table style="table-layout: fixed; width: 90%; margin: auto;">
<caption style="caption-side: top;">러스트 주석 종류</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">비문서 주석(non-doc comments)</th><th style="text-align: center;">문서 주석(doc comments)</th></tr></thead>
<tbody>
<tr><td>일반적인 <a href="C.md">C</a>/<a href="C.md">C++</a> 언어 형식의 <a href="C.md#주석">주석</a>과 동일하다.</td><td>문서 주석은 한줄(<code>///</code>) 그리고 블록(<code>/** */</code>)으로 입력할 수 있으며, 데이터에 대한 간략한 설명을 기입하는 데 사용된다.</td></tr>
<tr style="vertical-align: top; overflow-wrap: break-word;"><td>

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
</td></tr>
</tbody>
</table>

일반 주석과 달리, 사용자가 정의한 데이터에 기입된 문서 주석은 `doc`이란 특수한 속성에 저장되어 정의된 데이터에 대한 설명을 정의한 소스 코드를 찾아가지 않고서도 곧바로 내용을 살펴볼 수 있다.

![VS Code에서의 문서 주석 활용 예시](./images/rust_doccomments.png)

## 자료형
[자료형](https://ko.wikipedia.org/wiki/자료형)(data type)은 데이터를 어떻게 표현할 지 결정하는 요소이며, 러스트에서는 다음과 같이 존재한다.

<table style="width: 80%; margin: auto;"><caption style="caption-side: top;">러스트의 <a href="https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#scalar-types">원시 스칼라 자료형</a></caption><colgroup><col style="width: 20%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">키워드</th><th style="text-align: center;">자료형</th><th style="text-align: center;">크기 (바이트)</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><code>i8</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u8</code>)</i></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/numeric.html">숫자</a></td><td style="text-align: center;">1</td><td>1바이트 정수</td></tr><tr><td style="text-align: center;"><code>i16</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u16</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">2</td><td>2바이트 정수</td></tr><tr><td style="text-align: center;"><code>i32</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u32</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>4바이트 정수</td></tr><tr><td style="text-align: center;"><code>i64</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u64</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>8바이트 정수</td></tr><tr><td style="text-align: center;"><code>i128</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">u128</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>16바이트 정수</td></tr><tr><td style="text-align: center;"><code>isize</code><br/><i>(<a href="https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int">unsigned</a>: <code style="font-style: normal;">usize</code>)</i></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16 <sub>(최소)</sub></td><td><a href="#포인터">포인터</a> 크기의 정수; 예를 들어 x86는 32비트, 그리고 x64는 64비트 정수로 계산된다.</td></tr><tr><td style="text-align: center;"><code>f32</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">4</td><td>32비트 <a href="https://en.wikipedia.org/wiki/Single-precision_floating-point_format">단정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f64</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">8</td><td>64비트 <a href="https://en.wikipedia.org/wiki/Double-precision_floating-point_format">배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>f128</code></td><td style="text-align: center;">숫자</td><td style="text-align: center;">16</td><td>128비트 <a href="https://en.wikipedia.org/wiki/Quadruple-precision_floating-point_format">4배정밀도 실수</a></td></tr><tr><td style="text-align: center;"><code>bool</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/boolean.html">논리</a></td><td style="text-align: center;">1</td><td>참(<code>true</code>; 1) 혹은 거짓(<code>false</code>; 0)</td></tr><tr><td style="text-align: center;"><code>char</code></td><td style="text-align: center;"><a href="https://doc.rust-lang.org/reference/types/textual.html">텍스트</a></td><td style="text-align: center;">4</td><td>단일 <a href="https://en.wikipedia.org/wiki/UTF-32">UTF-32</a> 문자: 0x0000-0xD7FF 또는 0xE000-0x10FFFF</td></tr></tbody/></table>

> [바이트](https://ko.wikipedia.org/wiki/바이트)(byte)란, 컴퓨터에서 메모리에 저장하는 가장 기본적인 단위이다. 자료형마다 크기가 정해진 이유는 효율적인 메모리 관리 차원도 있으나 CPU 연산과도 깊은 연관성을 갖는다. 한 바이트는 여덟 개의 [비트](https://ko.wikipedia.org/wiki/비트_(단위))(bit)로 구성된다.

여기서 [unsigned](https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned_int)란, 자료형 중에서 [최상위 비트](https://ko.wikipedia.org/wiki/최상위_비트)를 정수의 [부호](https://ko.wikipedia.org/wiki/Signed와_unsigned)를 결정하는 요소로 사용하지 않는다. 아래의 16비트 정수형인 `i16`는 원래 최상위 비트를 제외한 나머지 15개의 비트로 정수를 표현한다. 반면 unsigned 자료형인 `u16`은 음의 정수를 나타낼 수 없지만, 16개의 비트로 양의 정수를 더 많이 표현할 수 있다.

```rust
i16             // 표현 가능 범위: -32768 ~ +32767
u16             // 표현 가능 범위:     +0 ~ +65535
```

그 외의 자료형들은 차후 러스트 프로그래밍 언어에 대한 개념을 소개하면서 함께 설명할 예정이다.

### 자료형 변환
자료형 변환(type conversion)은 데이터를 다른 자료형으로 바꾸는 작업이며, 불가피하게 데이터가 손실될 수 있으므로 유의하도록 한다. 러스트는 일반적인 경우에 암시적 자료형 변환, 일명 [강제 변환](https://doc.rust-lang.org/reference/type-coercions.html)(coercion)을 지원하지 않는다.

* **[자료형 캐스팅](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)**(type casting)

    명시적 자료형 변환에 해당하며, 데이터의 우변에 이진 연산자 [`as`](https://doc.rust-lang.org/reference/expressions/operator-expr.html#type-cast-expressions)와 함께 변환 자료형을 기입하여 좌변으로 해당 값을 전달한다.

    ```rust
    let variable: i16 = 13;
    let unsigned: u16 = variable as u16;
    ```

    만일 `as` 연산자가 없었더라면 암시적 자료형 변환에 해당하기 때문에 러스트는 자료형 불일치라는 명목으로 컴파일 오류를 알린다.
