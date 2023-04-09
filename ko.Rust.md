---
category: 프로그래밍
title: 러스트
---
# 러스트
> *출처: [The Rust Programming Language <sub>(영문)</sub>](https://doc.rust-lang.org/book/)*

[러스트](https://www.rust-lang.org/)(Rust)는 성능과 보안이 매우 강조된 프로그래밍 언어이며, 특히 자료형 및 메모리 관리 측에서 강점을 지닌다. 뿐만 아니라, 하드웨어 제어가 가능하다는 점에서 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 개발에도 활용할 수 있다. 이러한 특성들에 의해 [C++](ko.Cpp.md) 언어와 유사한 성능을 지닌 동시에 훌륭한 보안성이 보장되어 [마이크로소프트](https://www.microsoft.com/), [구글](https://www.google.com/), [아마존](https://www.amazon.com/) 등의 주요 IT 기업의 관심과 지원을 받고 있다.

## 설치
러스트 프로그래밍 언어를 실행하는 데 필요한 [툴체인](https://ko.wikipedia.org/wiki/툴체인)은 공식 홈페이지에서 `rustup_init.exe`을 [다운로드](https://www.rust-lang.org/tools/install) 받아 설치한다.

> 설치 과정에서 2013 버전 이상의 [비주얼 스튜디오](https://visualstudio.microsoft.com/downloads/)이 요구되는 데, 미리 Desktop development with C++ 그리고 윈도우 10 또는 11 SDK를 준비하도록 한다.

![<code>rustup_init.exe</code> 시작 화면](./images/rustup_init_setup.png)

여기서 러스트 프로그래밍 언어의 툴체인(toolchain)이란, 소프트웨어 제품 개발에 활용되는 프로그래밍 도구들의 집합을 가리키며 대표적으로 다음 세 개의 프로그램이 있다.

* `rustc`: 러스트 [컴파일러](ko.Compiler.md)(compiler)
* [`cargo`](#프로젝트): 러스트 패키지 관리자; 프로젝트를 컴파일 및 실행한다.
* `rustup`: 러스트 툴체인 관리자, 즉 위의 `rustc` 및 `cargo` 프로그램을 다른 버전으로 설치 및 선택한다.

설치가 완료되면 `%UserProfile%`에 두 경로가 생성된다: `.rustup` 그리고 `.cargo` 폴더이다. 전자는 러스트 툴체인이 실질적으로 설치되는 공간으로, 설치된 툴체인은 `toolchains` 하위폴더에 저장된다. 이렇게 설치된 여러 툴체인들 중에서 원하는 버전으로 경로를 바꿔가며 사용하는 번거로움을 해소하기 위해, 후자 폴더에 위치한 `rustc` 또는 `cargo` 등 명령어 프로그램들은 `rustup`에서 선택한 버전으로 연동된다. 그러므로 콘솔창은 `.cargo` 폴더에서 벗어나지 않고서도 다양한 버전의 툴체인을 활용할 수 있게 된다.

### 통합 개발 환경
[통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)은 최소한 프로그래밍 언어의 소스 코드 편집, 프로그램 빌드, 그리고 디버깅 기능을 제공하는 소프트웨어 개발 프로그램이다. 툴체인은 러스트 코드를 컴퓨터가 실행할 수 있도록 하는 개발 도구이지만 러스트 코드 편집기는 아니다. 러스트 코드를 작성하고 프로그램으로 실행하여 문제가 발생하면 검토할 수 있는 IDE가 절대적으로 필요하다.

* [비주얼 스튜디오 코드](https://ko.wikipedia.org/wiki/비주얼_스튜디오_코드)<sub>([다운로드](https://code.visualstudio.com/download))</sub>, 일명 VS Code는 마이크로소프트에서 개발한 무료 소스 코드 편집기이다. 비록 기술적으로 IDE는 아니지만, rust-analyzer 확장도구<sub>([다운로드](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer))</sub>를 설치하면 코드 자동완성, 자료형 정의, 구문 하이라이트 등의 기능들을 제공한다. 코드 디버깅을 하려면 아래를 함께 설치한다.

    * [마이크로소프트 C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) (윈도우 운영체제)
    * [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) (macOS 또는 리눅스 운영체제)

## 프로젝트
러스트 프로그래밍 언어는 [`cargo`](https://doc.rust-lang.org/rust-by-example/cargo.html)라는 공식 패키지 관리 도구를 통해 프로젝트를 관리한다.

![VS Code에서 러스트 프로그래밍의 프로젝트](./images/vscode_rust.png)

<table style="width: 70%; margin: auto;">
<caption style="caption-side: top;"><code>cargo</code> 도구의 프로젝트 관리 명령</caption>
<colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup>
<thead><tr><th style="text-align: center;">명령어</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><code>cargo build</code></td><td>프로젝트 빌드</td></tr>
<tr><td style="text-align: center;"><code>cargo run</code></td><td>프로젝트 (빌드 및) 실행</td></tr>
<tr><td style="text-align: center;"><code>cargo clean</code></td><td>프로젝트 빌드 결과물 제거 및 정리</td></tr>
<tr><td style="text-align: center;"><code>cargo check</code></td><td>프로젝트 유효성 검사; 컴파일 생략으로 소모 시간을 단축하나 결과물 부재</td></tr>
</tbody>
</table>

VS Code 편집기에 rust-analyzer 확장도구를 사용하면 `main()` 진입점 위에 "▶ Run | Debug" CodeLens 표시가 나타난다. 이는 `cargo`를 대신하여 프로젝트를 컴파일 및 실행하는 데 활용될 수 있다. 그러나 `CTRL+F5` 또는 `F5` 단축키로 빌드 및 실행이 불가하므로, `.vscode` 폴더에 추가할 두 개의 JSON 파일을 공유한다.

```json
// 파일명: launch.json
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
            "preLaunchTask": "Rust: rustc.exe build active file"
        }
    ]
}
```
```json
// 파일명: tasks.json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cargo",
			"command": "run",
			"problemMatcher": [
				"$rustc"
			],
			"label": "Rust: rustc.exe build active file",
			"group": {
				"kind": "build",
				"isDefault": true
			}
		}
	]
}
```

러스트 프로그래밍은 기본적으로 디버그 모드에서 프로젝트를 빌드하고 실행하며, 컴파일 부산물들은 `.\target\debug` 폴더 안에 위치한다. 만일 정식 배포를 위해 최적화된 프로그램으로 빌드하기 위헤 명령어에 `--release` 플래그를 추가하면 `.\target\release` 폴더에 프로그램이 생성된다.

### 크레이트
[크레이트](https://doc.rust-lang.org/rust-by-example/crates.html)(crate; 화물상자)는 컴파일로 생성된 가장 작은 단위의 결과물이며, 간단히 말해 `.RS` 소스 파일(일명 크레이트 파일; crate file)로부터 생성된 실행 및 라이브러리 파일이다. 크레이트 파일 중에서 러스트 프로그래밍 컴파일의 근본이 되는 소스 파일을 크레이트 루트(crate root)이라고 부른다.

크레이트는 아래와 같이 두 유형으로 나뉘어진다:

<table style="width: 70%; margin: auto;">
<caption style="caption-side: top;">이진 및 라이브러리 크레이트 비교</caption>
<colgroup><col style="width: 16%;"/><col style="width: 44%;"/><col style="width: 44%;"/></colgroup>
<thead><tr><th></th><th style="text-align: center;">이진 크레이트 (binary crate)</th><th style="text-align: center;">라이브러리 크레이트 (library crate)</th></tr></thead>
<tbody><td style="text-align: center;">설명</td><td style="text-align: center;"><code>.EXE</code> 실행 프로그램</td><td style="text-align: center;"><code>.RLIB</code> 라이브러리 파일</td></tbody>
<tbody><td style="text-align: center;">크레이트 루트</td><td style="text-align: center;"><code>src/main.rs</code></td><td style="text-align: center;"><code>src/lib.rs</code></td></tbody>
<tbody><td style="text-align: center;"><code>main</code> 진입점</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tbody>
</table>

러스트 프로그래밍에서 크레이트를 이야기하면 흔히 "라이브러리 크레이트"를 가리키며, 이는 일반 프로그래밍 언어에서의 "[라이브러리](ko.C.md#라이브러리)"와 혼용되어 언급되기도 한다.

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

### 주석
[주석](https://doc.rust-lang.org/reference/comments.html)(comment)은 프로그램의 소스 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데, 크게 비문서 주석(non-doc comments) 그리고 문서 주석(doc comments)로 나뉘어진다.

<table style="table-layout: fixed; width: 90%; margin: auto;">
<caption style="caption-side: top;">러스트 주석 종류</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">비문서 주석(non-doc comments)</th><th style="text-align: center;">문서 주석(doc comments)</th></tr></thead>
<tbody>
<tr><td>일반적인 <a href="ko.C.md">C</a>/<a href="ko.C.md">C++</a> 언어 형식의 <a href="ko.C.md#주석">주석</a>과 동일하다.</td><td>문서 주석은 한줄(<code>///</code>) 그리고 블록(<code>/** */</code>)으로 입력할 수 있으며, 데이터에 대한 간략한 설명을 기입하는 데 사용된다.</td></tr>
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
