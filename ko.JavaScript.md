---
category: 프로그래밍
title: 자바스크립트
---
# 소개
> *참조: [JavaScript &#124; MDN Web Docs (영문)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)*

[자바스크립트](https://ko.wikipedia.org/wiki/자바스크립트)(JavaScript)는 웹디자인에 사용된 [HTML](#html-웹프로그래밍-언어) & [CSS](https://ko.wikipedia.org/wiki/CSS)만으로는 제한적인 [동적 웹페이지](https://ko.wikipedia.org/wiki/동적_웹페이지)를 구현하기 위해 개발된 스크립트 언어였다. 자바스크립트가 [웹 브라우저](https://ko.wikipedia.org/wiki/웹_브라우저)에서만 실행될 수 있던 당시에는 활용도가 클라이언트 측에서 웹페이지 상호작용을 위한 [프론트엔드](https://ko.wikipedia.org/wiki/프론트엔드와_백엔드)(front-end)에 한정되었다. 현재는 데이터 처리 및 어플리케이션 제작 등 서버 측에서 [백엔드](https://ko.wikipedia.org/wiki/프론트엔드와_백엔드)(back-end)로도 활발히 사용되고 있다.

## 인터프리터
자바스크립트 프로그래밍 언어는 [인터프리트 언어](ko.Interpreter.md)(interpreted language)이다. 초창기 자바스크립트 코드를 실행하는 [자바스크립트 엔진](https://ko.wikipedia.org/wiki/자바스크립트_엔진)(JavaScript engine)은 단순 인터프리터였으나, 현대에는 성능 향상을 위해 [JIT 컴파일](ko.Compiler.md#jit-컴파일)(just-in-time compile)을 활용한다.

# 설치
자바스크립트를 실행하기 위해서는 두 가지 프로그램이 필요하다: (1) 자바스크립트 엔진 그리고 (2) 통합 개발 환경이다. 자바스크립트 엔진은 기본적으로 웹 브라우저에 탑재되어 있으나, 본문은 자바스크립트를 최대한 프로그래밍 언어적인 측면에서 설명하기 위해 백엔드 관점에서 바라본다.

## Node.js
[Node.js](https://ko.wikipedia.org/wiki/Node.js)<sub>([다운로드](https://nodejs.org/ko/))</sub>은 구글에서 개발한 [크롬](https://ko.wikipedia.org/wiki/구글_크롬) 및 [크로미엄](https://en.wikipedia.org/wiki/크로미엄_(웹_브라우저)) 웹 브라우저에 탑재된 [V8](https://ko.wikipedia.org/wiki/V8_(자바스크립트_엔진)) 자바스크립트 엔진을 활용하는 런타임 환경(runtime environment)이다.

> 자바스크립트는 [인터프리트 언어](#인터프리트-언어)이기 때문에 런타임 환경만 있으면 프로그램을 실행하는데 충분하다.

![Node.js 설치 화면](./images/nodejs_install_startup.png)

Node.js는 "JavaScript everywhere(어디서든 자바스크립트)" 패러다임을 반영하여 웹 브라우저(즉, 클라이언트)로 한정되었던 자바스크립트의 활용도를 서버에서도 사용할 수 있도록 한다. 이는 웹 어플리케이션을 개발하는데 인터페이스와 알고리즘을 동일한 자바스크립트 언어로 구현할 수 있다는 점에서 강력한 장점 중 하나로 작용한다.

### 런타임 환경
> *참고: [What the heck is the event loop anyway? &#124; Philip Roberts &#124; JSConf EU - YouTube](https://www.youtube.com/watch?v=8aGhZQkoFbQ)*

Node.js에서 가장 주목할 특징은 [비동기 입출력](https://ko.wikipedia.org/wiki/비동기_입출력)(asynchronous I/O)이 가능한 [이벤트 기반 아키텍처](https://ko.wikipedia.org/wiki/이벤트_기반_아키텍처)(event-driven architecture)라는 점이다. 비록 아래 이미지는 웹 브라우저의 런타임 환경을 시각화하였으나, 브라우저가 자체적으로 제공하는 [Web APIs](https://en.wikipedia.org/wiki/Web_API) 대신에 C 언어로 제작된 [libuv](https://en.wikipedia.org/wiki/Libuv) 라이브러리로 대체하면 바로 Node.js의 런타임 환경이다.

![웹 브라우저 런타임 환경<sub><i>출처: <a href="https://medium.com/@lizfaria/the-javascript-engine-and-runtime-environment-e0ed86fea903">Medium @Liz Faria</a></i></sub>](./images/js_runtime_environment.png)

좌측은 자바스크립트 엔진을 의미하며, Node.js 런타임 환경에서는 V8 엔진이 해당한다. 엔진 내부에는 데이터를 저장할 [힙](https://ko.wikipedia.org/wiki/동적_메모리_할당#힙_영역)(heap) 영역 메모리와 자바스크립트 코드를 실행하는 [스택](https://ko.wikipedia.org/wiki/스택) 메모리, 일명 [호출 스택](https://ko.wikipedia.org/wiki/콜_스택)(call stack)을 갖는다. [파이썬](ko.Python.md)이나 [C](ko.C.md)/[C++](ko.Cpp.md) 등의 일반적인 타 프로그래밍 언어와 마찬가지로 자바스크립트는 단일 [스레드](ko.Process.md#스레드)(thread)만을 사용하기 때문에 하나의 호출 스택만을 활용하는데, 스택 구조로 인해 한 코드가 완전히 끝날 때까지 다음 코드로 실행되지 않는 블로킹(blocking)이 작용한다. 허나 이러한 구조적 문제가 유난히 자바스크립트에서 지적되는 이유는 웹페이지 상호작용이나 마우스/키보드 이벤트 등이 자주 동시다발적으로 나타날 수 있는 환경에서 이를 최대한 막힘없이 처리할 수 있는 능력이 어느 프로그래밍 언어보다 요구되기 때문이다.

자바스크립트는 웹 브라우저에 탑재된 웹 API를 호출하여 함수 및 기능을 사용할 수 있다. 만일 웹 API 중에서 [DOM](https://ko.wikipedia.org/wiki/문서_객체_모델)으로부터 `onClick`, `onLoad`, `onDone` 등의 이벤트가 동작하면 웹 API의 인자로써 전달된 [콜백](#콜백-함수)(callback)이 [큐](https://ko.wikipedia.org/wiki/큐_(자료_구조))(queue)에 대기한다. 자바스크립트 엔진의 호출 스택이 비었으면 대기 중인 콜백을 불러와 실행하는데, 이를 처리하는 요소가 바로 [이벤트 루프](https://ko.wikipedia.org/wiki/이벤트_루프)(event loop)이다. 정리하자면, 자바스크립트는 콜백과 이를 지원하는 런타임 환경 덕분에 비동기 입출력이 가능해져 여러 작업을 동시에 처리할 수 있는 것이다.

## 통합 개발 환경
[통합 개발 환경](https://ko.wikipedia.org/wiki/통합_개발_환경)(integrated development environment; IDE)은 최소한 프로그래밍 언어의 소스 코드 편집, 프로그램 빌드, 그리고 디버깅 기능을 제공하는 소프트웨어 개발 프로그램이다. 자바스크립트 엔진은 자바스크립트 코드를 실행하는 소프트웨어이지만, 자바스크립트 코드 편집기가 아니다. 그러므로 자바스크립트 코드를 편집하고 곧바로 프로그램으로 실행하여 문제가 발생하면 검토할 수 있는 IDE가 절대적으로 필요하다.

### 비주얼 스튜디오
[비주얼 스튜디오](https://ko.wikipedia.org/wiki/마이크로소프트_비주얼_스튜디오)<sub>([다운로드](https://visualstudio.microsoft.com/downloads/))</sub>는 마이크로소프트에서 개발한 IDE이다. 비주얼 스튜디오의 세 가지 에디션 중에서 무료 버전인 커뮤니티 에디션으로도 충분하다. 자바스크립트 언어 개발을 위해서면 "Node.js development"를 선택한다. 단, Node.js은 포함되지 않으므로 별도 설치가 필요하다. 한국어 지원을 원하면 "Language packs" 탭에서 한국어도 함께 선택한다.

![비주얼 스튜디오 자바스크립트 프로그래밍을 위한 구성요소](/images/visual_studio_javascript.png)

비주얼 스튜디오를 실행하면 아래와 같은 시작화면이 나타난다. 새로운 프로젝트를 생성하려면 오른쪽 하단의 "Create a new project" 버튼을 클릭한다.

![비주얼 스튜디오 시작화면](./images/visual_studio_startup.png)

자바스크립트 프로그래밍 언어로 다양할 어플리케이션을 만들 수 있어, 비주얼 스튜디오에서도 다양한 종류의 프로젝트 생성 선택지를 제공한다. 그 중에서 간단한 콘솔 어플리케이션을 위한 자바스크립트 프로그래밍 언어 프로젝트 생성은 아래의 절차를 따른다:

1. 프로그래밍 언어를 JavaScript로 선택하여 "Blank Node.js Console Application"을 클릭한다.

2. 프로젝트 및 솔루션 이름을 선정한다. 여기서 프로젝트란, 소스 코드와 컴파일러 설정 등의 실질적인 코딩 내용을 관리하는 `.njsproj` 확장자 파일이며, 솔루션은 여러 프로젝트 파일을 하나의 폴더처럼 관리하는 `.sln` 확장자 파일이다. 비주얼 스튜디오에서 프로젝트는 `.sln` 파일로 열기를 권장한다.

3. 비주얼 스튜디오에서 알아서 준비한 프로젝트를 그대로 사용한다.

비주얼 스튜디오는 두 가지의 실행 방법이 있다: 일반 실행 모드(`Ctrl+F5`)와 디버그 모드(`F5`)이다. [디버그](https://ko.wikipedia.org/wiki/디버그)(debug)는 프로그램에 발생한 문제를 해결하는 행위로, IDE에서 각 줄의 코드마다 어떠한 변화가 생겼는지 혹은 얼만큼의 시스템 리소스를 소모하는지 등을 확인할 수 있는 정보를 제공한다. 디버깅 목적이 아니면 일반 실행 모드를 사용하는 것을 권장한다.

### 비주얼 스튜디오 코드
[비주얼 스튜디오 코드](https://ko.wikipedia.org/wiki/비주얼_스튜디오_코드)<sub>([다운로드](https://code.visualstudio.com/download))</sub>, 일명 VS Code는 마이크로소프트에서 개발한 무료 소스 코드 편집기이다. 비록 기술적으로 IDE는 아니지만, 확장도구를 설치하여 컴파일러 혹은 인터프리터를 불러오면 코드 실행 및 디버깅이 모두 가능한 IDE 역할을 수행한다. 자바스립트의 경우에는 자동 코드 완성(일명 IntelliSense), 코드 탐색기, 디버거 등이 이미 탑재되어 있어 다른 프로그래밍 언어처럼 별도의 확장도구 설치가 불필요하다.

![VS Code에서 Node.js 작업 환경 예시](/images/js_vscode.png)

VS Code는 두 가지의 실행 방법이 있다: 일반 실행 모드(`Ctrl+F5`)와 디버그 모드(`F5`)이다. [디버그](https://ko.wikipedia.org/wiki/디버그)(debug)는 프로그램에 발생한 문제를 해결하는 행위로, IDE에서 각 줄의 코드마다 어떠한 변화가 생겼는지 혹은 얼만큼의 시스템 리소스를 소모하는지 등을 확인할 수 있는 정보를 제공한다. 디버깅 목적이 아니면 일반 실행 모드를 사용하는 것을 권장한다. 최초 실행 당시에는 상단에 `Select Environment` 문구가 나타나면 Node.js을 선택하도록 한다.

# 기초
각 프로그래밍 언어마다 준수되어야 할 규칙(일명 [구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어)); syntax)과 기반이 되는 데이터들이 존재한다. 이를 어길 시에는 프로그램에 오류가 발생하거나 정상적인 동작을 보장할 수 없다. 실질적인 프로그래밍에 있어, 본 장에서는 자바스크립트 코딩에 기초적인 정보 제공을 목표로 한다.

> 본문은 [ECMAScript 2015](https://ko.wikipedia.org/wiki/ECMA스크립트), 일명 ES6 스크립트 언어 표준으로부터 소개된 자바스크립트 데이터 및 구문을 기준으로 소개한다.

## 주석
[주석](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#comments)(comment)은 프로그래밍에 있어 실행되지 않는 부분이며, 흔히 어떠한 정보를 간략히 스크립트 내에 입력하는데 사용된다. 자바스크립트에는 두 가지의 주석이 존재하며, 이들은 각각 한줄 주석과 블록 주석이라 부른다.

* **한줄 주석 (line comment)**: 코드 한 줄을 차지하는 주석이며, 두 개의 슬래시 `//`로 표시된다.
* **범위 주석 (delimited comment)**: 코드 여러 줄을 차지하는 주석이며, 한 쌍의 슬래시와 별표 `/* */`로 표시된다.

```js
/*
    블록 주석:
    코드 여러 줄을 차지하는 주석이다.
*/
// 한줄 주석: 코드 한 줄을 차지하는 주석이다.
```

## 표현식
프로그래밍에서는 표현식과 문장이 있다.

* **[표현식](https://ko.wikipedia.org/wiki/식_(프로그래밍))(expression)**

    값을 반환하는 구문적 존재를 가리킨다. 표현식에 대한 결과를 도출하는 것을 평가(evaluate)라고 부른다.
    
    ```js
  2 + 3       // 숫자 5를 반환
  2 < 3       // 논리 참을 반환
    ```

* **[문장](https://ko.wikipedia.org/wiki/문_(프로그래밍))(statement)**

    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 자바스크립트에서 모든 문장은 문장 종단자(statement terminator)인 세미콜론 `;`으로 마무리 되어야 한다.

    ```js
  variable = 2 + 3;          // 숫자 5를 "variable" 변수에 초기화
  if (2 < 3) statement;      // 논리가 참이면 "statement" 문장 실행
    ```

* **[블록](https://ko.wikipedia.org/wiki/블록_(프로그래밍))(block)**

    소스 코드를 중괄호 `{}`로 그룹화시키는 프로그래밍 언어의 어휘적 구조이다. 블록의 두 가지 의의는 (1) 여러 문장들을 하나의 문장처럼 다루거나 (2) 데이터가 취급되는 [영역](#유효범위)을 구분 짓는다.

    ```js
  {
      variable = 2 + 3;
      if (2 < 3) statement;  
  }
    ```

## 입력 및 출력
자바스크립트는 본래 동적 웹페이지를 제공하기 위해 설계된 스크립트 언어라는 점을 감안해야 한다. 출력의 경우에는 진단을 위한 [로그](https://ko.wikipedia.org/wiki/로그파일)(log) 형식으로 출력이 가능하나, 일반적인 프로그래밍 언어가 갖는 콘솔 입력을 기대할 수 없다. 그러므로 예외적으로 자바스크립트 입력은 Node.js 모듈을 소개한다.

* **입력 함수 `PromptSync.Prompt()`**
    
    입력 함수가 실행될 시, `PromptSync.Prompt()`의 소괄호 `()` 안에 있는 텍스트가 터미널에 나타나며 Enter/Return을 누를 때까지 대기한다.

    > 본 함수를 사용하기 위해서는 npm으로부터 [`prompt-sync`](https://www.npmjs.com/package/prompt-sync) 모듈을 설치해야 한다. 자세한 내용은 [모듈](#모듈)(module)에서 설명한다.
    >
    > ```bash
    > npm install prompt-sync
    > ```

* **출력 함수 [`console.log()`](https://developer.mozilla.org/en-US/docs/Web/API/console/log)**

    출력 함수가 실행될 시, `console.log()`의 소괄호 `()` 안에 있는 데이터가 터미널에 나타난다.

```js
/* prompt-sync 모듈 불러오기 */
const prompt = require('prompt-sync')();

variable = prompt("입력: ");
console.log("출력:", variable);
```
```terminal
입력: Hello World!
출력: Hello World!
```

하나의 `console.log()` 함수에서 두 개 이상의 데이터를 한꺼번에 출력하려면 쉼표 `,`를 사용하여 연속적으로 데이터를 나열할 수 있다. 단, 각 쉼표가 위치한 곳에는 항상 공백이 놓여진다. 그 외의 다른 방법은 [문자열 자료형](#문자열-자료형)을 설명하는 부분에서 소개한다.

```js
A = 10.0;
B = "자바스크립트";

// 텍스트와 숫자의 혼합된 데이터를 쉼표(,)를 사용해 나열한다.
console.log("A는", A , ",\n그리고 B는", B, "이다.");
```
```terminal
A는 10.0 ,
그리고 B는 자바스크립트 이다.
```

## 식별자
[식별자](https://ko.wikipedia.org/wiki/식별자#컴퓨터_언어)(identifier), 일명 네임(name)은 프로그램을 구성하는 데이터들을 구별하기 위해 사용되는 명칭이다. 즉, 식별자는 개발자가 데이터에 직접 붙여준 이름이다. 파이썬에서 식별자를 선정하는데 아래의 규칙을 지켜야 한다.

* 오직 영문, 숫자, 밑줄 `_`, 그리고 달러 표시 `$`만 허용된다.
* 첫 문자는 숫자로 시작할 수 없다.
* 공백은 허용되지 않는다.
* 대소문자를 구분한다.

## 변수
변수(variable)는 할당 기호 `=`를 사용하여 데이터를 할당(assignment)받을 수 있는 저장공간이다. 아래 예시는 `variable`이란 식별자를 갖는 변수에 숫자 3을 할당한다. 시스템적 관점에서 바라보면 `variable`이란 이름에 숫자 3이란 데이터를 엮는 절차를 [네임 바인딩](https://ko.wikipedia.org/wiki/네임_바인딩)(name binding)이라고 하며, 비로서 해당 식별자가 변수로 "정의(definition)"되었다고 한다.

```js
/* 변수 "variable"의 정의 */
variable = 3;
```

자바스크립트의 변수는 반드시 네임 바인딩으로부터 정의되어야 한다. 즉, 아무런 데이터 할당이 없으면 인터프리터는 식별자를 변수로 간주하지 않아 오류가 발생한다.

```js
variable;
console.log(variable);
```
```terminal
Uncaught ReferenceError: variable is not defined
```

> 본 내용은 가장 근본된 자바스크립트의 변수 정의이다. 이후 ES6에서는 새로운 변수 유형 세 개를 소개하였으며, 이들과 구분짓기 위해 해당 변수를 "선언 생략형"이라 칭한다.

거의 모든 프로그래밍 언어는 할당 기호를 기준으로 왼쪽에는 피할당자(변수), 오른쪽에는 할당자(데이터 혹은 변수)가 위치한다. 반대로 놓여질 경우, 오류가 발생하거나 원치 않는 결과가 도출될 수 있다.

자바스크립트의 변수는 데이터 종류와 상관없이 새로운 데이터를 언제든지 할당받을 수 있다.

```js
variable = 3;
variable = "Hello World!";
console.log(variable);
```
```terminal
Hello World!
```

### 유효범위
[유효범위](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_scope)(scope)란, 현재 실행되는 코드로부터 값 또는 표현식이 가시적이거나 참조가 가능한 문맥을 가리킨다. 간단히 말해, 변수는 유효범위 내에서만 접근이 가능하며 벗어날 시에는 사용불가하다. 자바스크립트에는 총 세 가지의 유효범위가 존재한다. ES6 이전의 자바스크립트는 함수와 전역 범위만 존재하였으며, 비록 소스 코드 문장을 그룹화시키는 요소인 블록이 있어도 별도의 범위로 구분하지 않았다.

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">자바스크립트 유효범위</caption>
<colgroup><col style="width: 10%;"/><col style="width: 10%;"/><col style="width: 80%;"/></colgroup>
<thead><tr><th style="text-align: center;">유효범위</th><th style="text-align: center;">ES6 추가</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;">블록</td><td style="text-align: center;">✔️</td><td><a href="#표현식">블록</a> <code>{}</code> 내부에 선언된 변수는 해당 블록 및 네스티드 블록(즉, 안에 있는 블록)으로 접근이 제한된다.</td></tr><tr><td style="text-align: center;">함수</td><td style="text-align: center;">-</td><td><a href="#함수">함수</a> 블록 내부에 선언된 변수는 해당 함수 안에서 네스티드 여부 상관없이 어디서든 사용될 수 있다.</td></tr><tr><td style="text-align: center;">전역</td><td style="text-align: center;">-</td><td>블록 및 함수 외부에 선언된 변수는 어플리케이션 내에서 접근이 자유롭다.</td></tr></tbody>
</table>

자바스크립트의 선언 생략형 변수는 항상 전역 범위(global scope)이다.

* **웹 브라우저**

    현 웹페이지에 연관된 모든 자바스크립트가 브라우저 창에 대한 [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window) [전역 객체](https://developer.mozilla.org/en-US/docs/Glossary/Global_object)를 통해 공유된다.

    ```js
  /* 웹 브라우저 */
  variable = "Hello World!";
  console.log(window.variable);    // 출력: Hello World!
    ```

* **Node.js**

    실행되고 있는 자바스크립트 파일, 일명 [모듈](#모듈)(module)에서만 [`global`](https://nodejs.org/api/globals.html#globals_global) [전역 객체](https://developer.mozilla.org/en-US/docs/Glossary/Global_object)를 통해 모듈 내에서만 데이터가 공유된다.

    ```js
  /* Node.js */
  variable = "Hello World!";
  console.log(global.variable);    // 출력: Hello World!
    ```

이러한 차이점은 브라우저의 경우에 웹페이지 HTML이 중심이 되어 여러 자바스크립트 파일들을 [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) 태그로 불러와 마치 하나처럼 실행하는 반면, Node.js는 하나의 자바스크립트 파일이 중심이 되어 실행되는 게 원인으로 볼 수 있다.

> 이러한 런타임 환경마다 상이한 전역 객체 호출 방식의 통일을 위해 자바스크립트는 [`globalThis`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) 키워드를 제공한다.
>
> ```js
> /* 웹 브라우저 */
> console.log(globalThis == window);    // 출력: true
>
> /* Node.js */
> console.log(globalThis == glabal);    // 출력: true
> ```

타 변수와 중복된 이름을 가진다면 예상치 못한 결과와 오류가 발생할 수 있어 가급적 전역 변수의 사용은 피하도록 한다.

### 변수 선언문
ES6부터는 새로운 방식의 바인딩이 소개되어 식별자 앞단에 특정 키워드를 기입하는 것만으로 변수로 "선언(declaration)"한다. 선언된 변수는 자동으로 [`undefined`](https://developer.mozilla.org/en-US/docs/Glossary/undefined)(즉, 아직 정의되지 않음)으로 네임 바인딩되지만, 선언 당시에 개발자가 직접 값을 할당하는 작업을 "초기화(initialization)"라고 일컫는다. 아래는 ES6에서 추가된 세 가지 변수 유형을 지정하는 선언문이다.

* **[`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) 선언문**

    블록 유효범위의 지역 변수(local variable)로 작용하며, 코드 블록 외부에서는 바인딩된 데이터가 소멸되어 더 이상 변수로써 사용할 수 없다. 지역 변수의 특징을 활용하면 코드 블록 외부에서 선언된 변수 이름을 그대로 가져와 코드 블록 내부에서 동일한 이름이지만 전혀 다른 존재의 변수를 새롭게 선언할 수 있다.

    ```js
  let x = 3;
  let y;

  /* 코드 블록 내부 */
  {
      x = 7;
      let y = "Hello World!";
      let z = true;
      
      console.log("x:", x);    // x: 7
      console.log("y:", y);    // y: Hello World!
      console.log("z:", z);    // z: true
  }

  console.log("x:", x);        // x: 7
  console.log("y:", y);        // y: undefined
  console.log("z:", z);        // ReferenceError: z is not defined
    ```

* **[`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 선언문**

    블록 유효범위의 지역 상수(local constant)로 작용하며, 코드 블록 외부에서는 바인딩된 데이터가 소멸되어 더 이상 상수로써 사용할 수 없다. 여기서 상수는 한 번 초기화된 이후에는 변경이 불가하므로, 선언과 동시에 반드시 초기화를 해주어야 한다.

    ```js
  /* "const" 변수의 잘못된 사용법 */
  const x;                     // SyntaxError: Missing initializer in const declaration
  x = "Hello World!";          // TypeError: Assignment to constant variable
    ```

* **[`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var) 선언문**

    해당 선언문은 두 가지 경우의 수가 있다:

    1. 함수 내부에 선언될 시 함수 유효범위의 지역 변수(local variable)
    2. 함수 외부에 선언될 시 전역 유효범위의 전역 변수(global variable)
    
    > [유효범위](#유효범위)(scope)에서 언급한 바에 따르면 Node.js의 전역 변수는 실행되는 자바스크립트 모듈 내에서 한정된다. 심지어 Node.js의 전역 객체 [`global`](https://nodejs.org/api/globals.html#globals_global)의 속성과 연동되지 않는 점에서, 오히려 모듈 유효범위의 지역 변수라고 표현하는 게 적합하다.
    >
    > ```js
    > /* Node.js */
    > var variable = "Hello World!";
    > console.log(global.variable);    // 출력: undefined
    > ```

    변수 중에서 유일하게 선언 [호이스팅](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting#variable_hoisting)(hoisting)을 지원한다. 즉, 변수를 선언하기 전에 미리 할당 및 호출되어 사용될 수 있다는 의미이다.

    ```js
  /* 변수 호이스팅 */
  variable = "Hello World!";
  var variable;

  /* 동일:
      var variable;
      variable = "Hello World!";
  */
    ```

### `delete` 연산자
[`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete) 연산자는 식별자에 바인딩된 데이터를 해제, 즉 변수의 정의를 무효화할 때 사용한다. 차후 동일한 식별자로 변수를 다시 정의할 수 있다.

> 주의해야 할 점으로 `var` 선언문의 변수만이 유일하게 적용되지 않으며, 그 이유로 변수 호이스팅과 연관이 있는 것으로 판단된다.

```js
/* 변수 "variable"의 정의 */
variable = "Hello World!";
console.log(variable);

/* 변수 "variable" 네임 바인딩 해제 */
delete variable;
console.log(variable);
```
```terminal
Hello World!
Uncaught ReferenceError: variable is not defined
```

## 자료형
[자료형](https://ko.wikipedia.org/wiki/자료형)(data type)은 데이터의 내용물이 어떻게 표현되는지 결정하는 요소이며, 자바스크립트이 갖는 총 일곱 개의 [원시 자료형](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) 중에서 가장 중요한 몇 가지를 살펴본다.

> 원시(primitive)는 데이터 자체에 기능성이나 하위 데이터를 내포하지 않는 순수한 데이터를 가리킨다. 이와 반대되는 개념으로 [객체](#객체)(object)가 존재한다.

### 숫자 자료형
[숫자 자료형](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#number_type)(number type)은 소수점을 포함한 64비트 실수를 표현하는 원시 자료형이다. 자바스크립트 숫자 자료형들은 산술 연산이 가능하다: 가장 기본적인 `+`, `-`, `*`, `/` 사칙 연산자부터 나눗셈의 나머지 `%` 그리고 제곱 `**`을 구할 수 있다. 산술 연산을 쉽게 읽을 수 있도록 숫자와 산술 연산자 사이에 공백을 넣어도 연산에는 아무런 영향을 주지 않으므로 무관한다.

[할당 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#assignment_operators)(assignment operator)는 할당 기호 `=`와 조합하여 산술 연산 코드를 더욱 간결하게 작성할 수 있도록 한다.

| 연산자 | 예시     | 동일                                          |
| :----: |--------| --------------------------------------------- |
|  `=`   | `x = y`  | `x = y`; `x` 변수에 `y` 변수의 값을 할당한다. |
|  `+=`  | `x += y` | `x = x + y`                                   |
|  `-=`  | `x -= y` | `x = x - y`                                   |
|  `*=`  | `x *= y` | `x = x * y`                                   |
|  `/=`  | `x /= y` | `x = x / y`                                   |
|  `%=`  | `x %= y` | `x = x % y`                                   |
|  `**=` | `x **= y`| `x = x ** y`                                  |

> 여기서 `=` 연산자가 할당된 값을 반환한다는 것을 통해 다음과 같은 표현식을 구현할 수 있다.
>
> ```js
> var variable;
> console.log(variable = 3);
> ```
> ```terminal
> 3
> ```

[증가 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Increment)(increment operator) `++` 및 [감소 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Decrement)(decrement operator) `--`는 데이터를 1만큼 증가 혹은 감소하는데 간략하게 한 줄로 표현할 수 있도록 한다.

* **접두부**

    해당 변수를 1만큼 증가/감소시킨 다음에 표현식을 평가한다.

    ```js
  x = ++y;    // 동일: y = y + 1; x = y;
  x = --y;    // 동일: y = y - 1; x = y; 
    ```

* **접미부**

    표현식을 평가한 다음에 해당 변수를 1만큼 증가/감소시킨다.

    ```js
  x = y++;    // 동일: x = y; y = y + 1;
  x = y--;    // 동일: x = y; y = y - 1;
    ```

### 논리 자료형
[논리 자료형](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#boolean_type)(boolean type)은 문장이 참인지 거짓인지 판별하는 논리 조건에 사용되는 원시 자료형이다.

* **논리적 참 `true`**: 논리가 참일 때 반환된다; 숫자 0이 아닌 정수로 대체 가능하다.
* **논리적 거짓 `false`**: 논리가 거짓일 때 반환된다; 숫자 0으로 대체 가능하다.

대표적인 논리 조건으로써 두 데이터를 대조하는 [비교 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#comparison_operators)(comparison operator)가 있다: 초과 `>`와 미만 `<`, 이상 `>=`과 이하 `<=`, 그리고 동일 `==`와 다름 `!=` 관계 부합 여부에 따라 논리적 참 혹은 거짓이 반환된다. 자바스크립트는 `===` 연산자를 갖는 소수의 프로그래밍 언어 중 하나이며, 값 뿐만 아니라 자료형의 일치 여부도 확인한다.

[논리 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#logical_operators)(logical operator)는 논리 자료형의 조합이 논리적으로 참인지 거짓인지 판별한다.

| 연산자  | 논리  | 설명                                        |
|:----:|-----|-------------------------------------------|
| `&&` | 논리곱 | 모든 데이터가 참이면 `true`를 반환하고, 그렇지 않으면 `false`를 반환한다.     |
| `||` | 논리합 | 하나 이상의 데이터가 참이면 `true`를 반환하고, 그렇지 않으면 `false`를 반환한다. |
| `!`  | 보수  | 참을 거짓으로, 또는 거짓을 참으로 변경한다.                 |

### 문자열 자료형
[문자열 자료형](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#string_type)(string type)은 한 쌍의 작은 따옴표 `''`, 큰 따옴표 `""`, 혹은 [템플릿 리터럴](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)(template literal)을 의미하는 [억음 부호](https://ko.wikipedia.org/wiki/억음_부호) <code>&grave;&grave;</code>로 구별되는 텍스트 자료형이다.

문자열 자료형은 다른 데이터와 더하기 기호 `+`를 통해 공백없이 하나의 문자열로 연결할 수 있다. 원래는 문자열 간에만 사용할 수 있으나, 자바스크립트가 자동으로 숫자 및 논리 자료형 등을 문자열로 변환해 준다. 그 외에도 ES6부터 추가된 한 쌍의 억음 부호로 표시되는 템플릿 리터럴 <code>&grave;&grave;</code>에서 `${}` 연산자를 통해 원하는 위치에 곧바로 데이터 삽입이 가능하다.

```js
A = 10.0;
B = "자바스크립트";

/* 문자열 연결 */
console.log("A는", A + ",\n그리고 B는", B + "이다.");

/* 템플릿 리터럴 */
console.log(`A는 ${A},\n그리고 B는 ${B}이다.`);
```
```terminal
A는 10.0,
그리고 B는 자바스크립트이다.
```

텍스트에 따옴표를 넣으려면 해당 따옴표 앞에 백슬래시 `\`를 배치하여 문자열이 도중이 끊기는 현상을 방지한다.

```js
/* 문자열 작성의 부적절한 예시와 적절한 예시의 비교. */
console.log('Where's my "Cat in the Hat" book?');
console.log('Where\'s my "Cat in the Hat" book?');
```
```terminal
Where
Where's my "Cat in the Hat" book?
```

### `null` 키워드
[`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) 키워드는 자료형과 관계없이 아무런 데이터가 없음을 의도적으로 나타내는 [원시값](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)이다.

> [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)도 마찬가지로 아무런 데이터가 없다는 것을 나타내는 원시 자료형이지만 값이 아니란 차이점이 있다. 이 때문에 `undefined`은 초기화되지 않은 변수에 나타나는 것이다.
> 
> ```js
> typeof null;         // 결과: object
> typeof undefined;    // 결과: undefined
> ```

비록 `null`과 `false`는 개념적으로 완전히 다른 존재이지만, 논리 조건에서는 `null`을 `false`로 사용할 수 있다. 이러한 특징을 보이는 값을 [거짓스러운 값](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)(falsy)이라고 부른다.

## 자료형 변환
자바스크립트는는 데이터를 타 자료형으로 [변환](https://developer.mozilla.org/en-US/docs/Glossary/Type_Conversion)(type conversion)할 수 있다.

* **숫자 자료형 변환 [`Number()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/Number)**

    문자열이나 논리값 등을 숫자 자료형으로 변환하며, 불가한 경우에는 [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)을 반환한다. [이진수](https://ko.wikipedia.org/wiki/이진법) 또는 [십육진수](https://ko.wikipedia.org/wiki/십육진법)를 표현하는 문자열을 [십진수](https://ko.wikipedia.org/wiki/십진법)로 표현할 수 있다.

    ```js
  Number("3.14");
    ```

    > 유사한 역할로 [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 및 [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) 함수가 있는데, 이는 문자열에서 정수 혹은 실수를 추출한다. `Number()`보다 느리지만 숫자가 아닌 텍스트를 거를 수 있다.
    >
    > ```js
    > parseFloat("3.14 < Pi");    // 결과: 3.14
    > ```

* **문자열 자료형 변환 [`String()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/String)**

    숫자나 논리값 등을 문자열 자료형으로 변환한다.

    ```js
  String(3.14);
    ```

    > 타 자료형을 문자열로 반환하는 또 다른 방법으로는 객체의 [`Object.toString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) 메소드를 이용할 수 있다. 여기서 원시 자료형은 객체와 반대되는 개념이지만 [오토박싱](https://developer.mozilla.org/en-US/docs/Glossary/Primitive#autoboxing_primitive_wrapper_objects_in_javascript)(autoboxing)에 의해 마치 객체처럼 사용할 수 있도록 한다.
    >
    > ```js
    > (3.14).toString();          // 결과: "3.14"
    > ```

### `typeof` 연산자
[`typeof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof) 연산자는 변수 혹은 상수 데이터의 자료형을 문자열로 알려준다. 

```js
typeof 3.14;              // 결과: number
typeof "Hello World!";    // 결과: string
```

## 탈출 문자
[탈출 문자](https://ko.wikipedia.org/wiki/이스케이프_문자)(escape character)는 백슬래시 기호 `\`를 사용하며, 문자열로부터 탈출하여 텍스트 데이터 내에서 특정 연산을 수행하도록 한다. 이전에 문자열 자료형을 소개할 때, `\n` 탈출 문자를 사용하여 문자열 줄바꿈을 구현한 것을 보여주었다.

```js
print("Hello\nWorld!")
```
```terminal
Hello
World!
```

## 엄격 모드
[엄격 모드](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)(strict mode)은 ECMAScript 5부터 소개되었으며 일반적인 자바스크립트에 비해 제약적인 자바스크립트 모드이다. 엄격 모드를 활성화하기 위해 `"use strict";`(혹은 `'use strict';`)을 (1) 스크립트 최상단에 기입하여 전체에 적용하거나 (2) [함수](#함수)의 최상단에 기입하여 국부적으로 적용할 수 있지만, 일반 블록에는 적용이 불가하다.

> 엄격 모드의 의의는 보다 "안전한" 자바스크립트를 작성할 수 있도록 하는 것으로, 이에 대한 내용은 차후에 설명할 [`this`](#this-키워드) 키워드와 밀접한 관련이 있다.

엄격 모드는 일반 자바스크립트에 비해 다음과 같이 몇 가지 차이점을 가진다.

1. 단순히 실수로 간주되었던 자바스크립트 코드를 오류로 치부한다.

    ```js
   'use strict';

   /* 설명: 식별자 오타로 의도치 않은 변수를 새로 정의하는 것을 방지하기 차원에서 전역 변수를 생성하는 것을 금지한다. */
   var variable;
   varible = "Hello World!";
    ```
    ```terminal
   Uncaught ReferenceError: varible is not defined
    ```

2. 자바스크립트 엔진이 최적화를 수행하는데 어려워 했던 부분들을 해결하였다. 그러므로 일부 동일한 코드에서 엄격 모드가 상대적으로 더 빨리 실행되기도 한다.

3. 차후 ECMAScript 버전에 정의될 수 있는 구문 사용을 금지한다.

    ```js
   'use strict';
   
   /* 설명: 차후 ECMAScript 버전을 위해 예약된 키워드 중 하나인 interface로 변수를 선언할 수 없다. */
   var interface = "Hello World!";
    ```
    ```terminal
   Uncaught SyntaxError: var interface = "Hello World!";
                             ^^^^^^^^^
    ```

# 조건 및 루프
조건문(conditional statement) 및 반복문(loop statement)은 프로그래밍에 가장 흔히 사용되는 코드 문장(statement) 중 하나이다. 여기서 문장이란, 실질적으로 무언가를 실행하는 코드를 의미한다. 본 장에서는 자바스크립트 프로그래밍의 조건에 따라 실행하는 조건문과 반복적으로 실행하는 반복문을 소개한다.

## `if` 조건문
[`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 조건문은 조건 혹은 논리가 참(`true`)일 경우 코드를 실행하며, 거짓(`false`)일 경우에는 코드를 실행하지 않는다.

```js
if (condition) {
    statements;
}

// 간략화된 문장
if (condition) statement;
```

### `else` 조건문
`else` 조건문은 단독으로 사용될 수 없으며 반드시 `if` 조건문 이후에 사용되어야 한다. 조건부가 거짓(`false`)으로 판정되면 실행할 코드를 포함한다.

```js
if (condition) {
    statements;
}
else {
    statements; 
}
```

### `else if` 조건문
`else if` 조건문은 `else`와 `if` 조건문의 조합으로 이전 조건이 거짓(`false`)일 때 새로운 조건을 제시한다.

```js
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
[조건 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)(ternary operator) `?:`는 세 가지 인수만을 사용하여 조건문을 아래와 같이 간략하게 표현한다. 조건 연산자는 가독성을 감소시키므로 과용해서는 안되지만 변수 할당에 유용하다.

```js
condition ? true_return : false_return;
```

## `switch` 조건문
[`switch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) 조건문은 전달받은 인자를 `case`의 상수와 동일한지 비교하여 논리가 참일 경우 해당 지점부터 코드를 실행하며, 거짓일 경우에는 다음 `case`로 넘어간다. 선택사항으로 `default` 키워드를 통해 어떠한 `case` 조건에도 부합하지 않으면 실행될 지점을 지정한다.

```js
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
        break;
}
```

`switch` 조건문이 어느 `case` 코드를 실행할지 결정하는 것이라고 쉽사리 착각할 수 있으나, 이는 사실상 `break` 탈출문 덕분이다. 탈출문이 없었더라면 아래 예시 코드처럼 해당 조건의 `case` 코드 실행을 마쳤어도 다음 `case` 코드로 계속 진행하는 걸 확인할 수 있다. 즉, `case` 키워드는 코드 실행 영역을 분별하는 것이 아니라 진입 포인트 역할을 한다.

```js
var variable = 2;

// switch 조건문의 동작 예시
switch (variable) {
    case 1:
        console.log("Statement 1");
    
    case 2:
        console.log("Statement 2");
    
    case 3:
        console.log("Statement 3");
     
    default:
        console.log("Statement 4");
}
```
```terminal
Statement 2
Statement 3
Statement 4
```

## `while` 반복문
[`while`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while) 반복문은 조건 혹은 논리가 참(`true`)일 동안 코드를 반복적으로 실행하며, 거짓(`false`)일 경우에는 반복문을 종료한다.

```js
while (condition) {
    statements;
}

// 간략화된 문장
while (condition) statement;
```

### `do` 반복문
[`do`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while) 반복문은 코드를 우선 실행하고 조건 혹은 논리가 참(`true`)일 경우 코드를 반복하며, 거짓(`false`)일 경우에는 반복문을 종료한다.

```js
do {
    statements;
} while (condition);
```

### `break` 문
[`break`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break) 문(일명 탈출문)은 반복문을 조기 종료시키는데 사용된다. 반복 실행 도중에 탈출문을 마주치는 즉시 가장 인접한 반복문으로부터 탈출한다.

### `continue` 문
[`continue`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue) 문은 반복문의 나머지 실행문을 전부 건너뛰어 다시 반복문의 조건부로 돌아간다. `break`와 달리 반복문은 종료되지 않고 여전히 살아있다.

## `for` 반복문
[`for`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) 반복문은 조건 혹은 논리가 참(`true`)일 동안 코드를 반복적으로 실행하며, 거짓(`false`)일 경우에는 반복문을 종료한다. `for` 반복문은 조건 평가 외에도 지역 변수를 초기화 및 증감할 수 있는 인자가 있다.

```js
for (initialize; condition; increment) {
    statements;
}

// 간략화된 문장
for (initialize; condition; increment) statement;
```

`for` 반복문의 우선 `initialize`에서 반복문 지역 변수를 선언하거나 외부 변수를 불러와 반복문을 위한 초기값을 할당한 다음 `condition`에서 조건을 평가한다. 논리가 참이면 코드를 반복적으로 실행하며, 거짓일 경우에는 반복문을 종료한다. 블록 내의 코드가 마무리되었거나 `continue` 문을 마주하면 `increment`에서 변수를 증감하고, `condition`으로 돌아가 절차를 반복한다.

### `for...of` 반복문
[`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) 반복문은 문자열이나 배열과 같은 [이터러블](#이터러블)(iterable)의 요소를 순서대로 하나씩 반환한다.

```js
for (const element of "ES6") {
    console.log(element);
}
```
```terminal
E
S
6
```

### `for...in` 반복문
[`for...in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) 반복문은 [객체](#객체)(object)에 속한 열거 가능한 속성들(enumerable properties)의 이름을 하나씩 반환한다.

```js
for (const property in "ES6") {
    console.log(property);
}
```
```terminal
0
1
2
```

# 이터러블
[이터러블](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)(iterable; 반복 가능한)은 저장된 여러 데이터 항목을 하나씩 반환할 수 있는 [컨테이너 객체](#객체)를 가리킨다. 이터러블의 특징인 `@@iterator` 메소드는 이터레이터(iterator) 객체를 반환하고, 그리고 이터레이터는 `next()` 메소드를 통해 [`for...of`](#forof-반복문) 반복문에 전달될 다음 데이터 항목을 반환한다. 다시 말해 이터러블 객체의 핵심은 순차적으로 데이터를 반환할 수 있다는 점이며, 다수의 데이터를 하나의 변수로 저장하는 성질은 이를 구현하기 위한 일환이다. 대표적인 시퀀스 객체 중 하나로 [문자열](#문자열-자료형)이 있다.

## 배열
[배열](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)(array) 이터러블 객체는 자료형과 관계없이 데이터를 나열한 순서대로 인덱스(index) 위치에 저장한다. 배열의 데이터 할당은 (1) 대괄호 `[]` 혹은 (2) [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) [클래스](#클래스)의 [생성자](#생성자) 안에 항목을 순서대로 쉼표로 나누어 나열한다. 아무런 데이터를 입력하지 않으면 빈 배열을 생성한다. 대괄호는 0번부터 시작하는 인덱스 위치의 요소(element)를 호출할 때에도 사용된다.

```js
var variable = [value1, value2, value3, value4, ...];
/* 동일:
    var variable = new Array(value1, value2, value3, value4, ...);
*/

console.log(variable);       // 출력: [ value1, value2, value3, value4, ... ]
console.log(variable[0]);    // 출력: value1
```

> [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) 연산자는 [클래스](#클래스)(`Array` 클래스)로부터 [객체](#객체)(배열 이터러블 객체)를 생성하는 연산자이다.

개별 요소를 재할당하여 데이터를 변경할 수 있다. 배열 범위를 벗어난 요소를 호출하면 `undefined`, 즉 초기화되지 않은 요소라고 반환되는데 차후에 데이터를 할당할 수 있다.

```js
var variable = [];
console.log(variable);        // 출력: []

variable[2] = "Hello World!";
console.log(variable);        // 출력: [ <2 empty items>, 'Hello World!' ]
```

### 전개 연산자
[전개 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)(spread operator) `...`는 배열에서 다음과 같이 활용된다.

* 배열 접두부에 위치할 경우, 배열의 모든 요소들을 전개하여 한꺼번에 반환한다.

    ```js
  var variable = [value1, value2, value3, value4, value5];

  console.log(variable);       // 출력: [ value1, value2, value3, value4, value5 ]
  console.log(...variable);    // 출력: value1 value2 value3 value4 value5
    ```

* [배열 구조 분해](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)(array destructuring), 즉 배열의 요소를 각 변수마다 나누어서 할당하는 과정에서 나머지 요소들을 한꺼번에 받을 변수 앞단에 기입된다.

    ```js
  var [batch1,  , ...batch2] = [value1, value2, value3, value4, value5];

  console.log(batch1);         // 출력: value1
  console.log(batch2);         // 출력: [ value3, value4, value5 ]
    ```

## 연관 배열
자바스크립트는 배열의 요소를 문자열로 호출하는 [연관 배열](https://ko.wikipedia.org/wiki/연관_배열)(associative array)을 지원하지 않는다. 설령 빈 배열에 요소 확장을 할 때 문자열을 입력하는 방법을 사용하여도, 이때부터는 배열이 아닌 [객체](#객체)(object)가 된다. 자바스크립트는 이러한 경우에 오히려 객체를 사용하는 것을 권고한다.

```js
var variable = [];

/* 비록 배열을 선언하였으나, 아래의 코드로 인해 배열이 아닌 일반 객체로 변환 */
variable['property1'] = value1;
variable['property2'] = value2;

console.log(variable);            // 출력: [ property1: value1, property2: value2 ]
console.log(variable.length);     // 출력: 0
```

## 형식화 배열
[형식화 배열](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)(typed array)은 요소의 데이터 형식이 고정된 이터러블 객체이다. 여기서 언급된 "데이터 형식"은 [자료형](#자료형)이 아니라 배열의 요소가 비가공된 이진 데이터를 어떻게 표현할 것인지를 가리킨다. 예를 들어 `Uint8Array` 형식화 배열은 데이터를 0 ~ 255 범위의 정수만으로 표현한다. 미디어 편집이나 비가공 데이터 접근 등과 같이 강력한 기능을 제공하는 현대 웹 어플리케이션의 요구에 대응하여 ES6부터 추가되었다. 단, 형식화 배열은 "배열"과 다른 존재이며 크기 확장이 불가하다.

> 형식화 배열은 [`ArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 클래스와 함께 사용되기도 하는데, 해당 클래스는 데이터 변경이 불가하지만 [바이트](https://ko.wikipedia.org/wiki/바이트)(byte) 단위의 [버퍼](https://ko.wikipedia.org/wiki/버퍼_(컴퓨터_과학))(buffer) 크기를 제공하는 용도로 사용된다.

```js
var buffer   = new ArrayBuffer(16);
var variable = new Int32Array(buffer);

/* 동일:
    var variable = new Int32Array(4);
*/
```

# 함수
[함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)(function)는 독립적인 코드 블록으로써 데이터를 처리하며, 재사용이 가능하고 호출 시 처리된 데이터를 보여주어 유동적인 프로그램 코딩을 가능하게 한다. 함수는 이름 뒤에 소괄호가 있는 `function()` 형식으로 구별된다.

```js
var variable = "3.14159";
console.log(parseInt(variable));
// 소수점을 반올림하는 "parseInt()" 함수
```
```terminal
3
```

함수의 기능을 정의(definition)하는 방법으로 두 가지가 있는데, 이 둘의 차이점은 차후 [`this`](#this-키워드) 키워드와 함께 설명할 예정이다.

1. **[`function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function) 키워드**

    자바스크립트에서 함수를 정의하는 전통적인 방식으로, [함수 호이스팅](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting#function_hoisting)(function hoisting)을 지원하여 함수가 정의되기 전에 호출하여 사용할 수 있다.

    > 함수의 식별자는 생략될 수 있는데, 이를 이름이 없는 [익명 함수](https://en.wikipedia.org/wiki/Anonymous_function)(anonymous function)라고 부르며 흔히 일회용으로 사용된다.

    ```js
   /* 기존 함수 구문 */
   function func() {
       
   }
    ```

2. **[화살표 함수 표현식](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)(arrow function expression)**

    ES6부터 새로 추가된 (익명) 함수를 정의하는 대안 방식으로, 기존 `function` 키워드의 일부 결함을 해소하기 위해 고안되었다.

    > 화살표 함수 표현식은 절대로 `function` 키워드를 대체하기 위한 것이 아니다! 이를 반영하는 예시로 화살표 함수 표현식은 함수 호이스팅을 지원하지 않는다.

    ```js
   /* 화살표 함수 표현식 */
   () => {
   
   }
    ```

자바스크립트는 이례적으로 함수 블록 안에 또 다른 함수를 정의하는 것이 허용되며, [함수 유효범위](#유효범위)에 해당하여 정의된 함수 내에서만 사용할 수 있다. 그리고 정의된 함수는 익명 여부를 불문하고 식별자에 네임 바인딩이 될 수 있다.

함수명 뒤에 소괄호 `()` 기입여부에 따라 의미하는 바가 다르다.

* `function()`은 함수에 정의된 코드를 실행한다.

    ```js
  function func() {
      console.log("Hello World!");
  }

  const variable = func();
  console.log(variable);
    ```
    ```terminal
  Hello World!
  undefined
    ```

* `function`은 함수 자체를 가리킨다.

    ```js
  function func() {
      console.log("Hello World!");
  }

  const variable = func;
  console.log(variable);
    ```
    ```terminal
  [Function: func]
    ```

### `return` 반환문
[`return`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return) 반환문은 함수로부터 데이터를 반환하는 함수 전용 문장이다. 반환문이 실행되면 하단에 코드가 남아 있음에도 불구하고 함수는 즉시 종료된다. 함수는 반환문을 반드시 필요로 하지 않으며, 이러한 경우에는 `undefined` 원시값이 반환되어 변수에 전달된다.

```js
// return 반환문이 있는 사용자 정의 함수
function func() {
    console.log("Hello World!");
    return 1 + 2;
}

console.log(func());
```
```terminal
Hello World!
3
```

### 매개변수 및 전달인자
다음은 함수에 대해 논의할 때 중요하게 언급되는 매개변수와 전달인자의 차이에 대하여 설명한다.

* **전달인자 (argument)**: 전달인자, 혹은 간략하게 "인자"는 함수로 전달되는 데이터이다.
* **매개변수 (parameter)**: 전달인자를 할당받는 함수 내의 지역 변수이다. 그러므로 매개변수는 함수 외부에서 호출이 불가능하다. 매개변수 선언은 함수의 소괄호 `()` 내에서 이루어진다.

> 매개변수와 전달인자는 개념적으로 다른 존재이지만, 동일한 데이터를 가지고 있는 관계로 흔히 두 용어는 혼용되어 사용하는 경우가 많다.

다음은 매개변수에 사용되는 연산자로 전달인자을 받는데 유연성을 제공한다. 이들은 프로그래밍 구문상 명확한 구별이 가능해야 하므로 반드시 일반 매개변수 뒤에 위치해야 한다.

| 연산자 | 구문          | 설명                                                            |
|:---:|:-----------:|---------------------------------------------------------------|
| `=` | `arg=value` | 전달인자가 없으면 기본값 `value`가 대신 매개변수에 할당된다. |
| `...` | `...arg` | 여러 개의 전달인자들을 하나의 배열로 전달받는다. |

```js
/* 예시. arg = value */
function func(arg1, arg2 = 2) {
    return arg1 + arg2;
}

func(3);            // 출력: 5 (= 3 + 2)
func(3, 4);         // 출력: 7 (= 3 + 4)
```
----
```js
/* 예시. ...arg */
function func(arg1, ...arg2) {
    console.log(arg1);
    console.log(arg2);
}

func(1, 2, 3, 4);   // 출력: 1
                    // 출력: [ 2, 3, 4 ]
```

## `this` 키워드
[`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) 키워드는 런타임 도중에 현재 실행되고 있는 자바스크립트 코드가 어느 데이터에 종속(일명 바인딩; binding)되어 있는지를 가리킨다. 자바스크립트에서 상당히 난해한 개념 중 하나인데, 이는 `this` 키워드가 기입된 위치와 [엄격 모드](#엄격-모드) 여부에 따라 가리키는 객체가 달라지기 때문이다. 그러한 동시에 활용도가 높아 널리 사용되고 있으므로 불가피한 내용이다.

1. **[전역 문맥](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this#global_context)(global context)**

    * **[웹 브라우저](#런타임-환경)**

        `this` 키워드는 엄격 모드와 무관하게 `window` 전역 객체를 반환된다.

        ```js
      console.log(this === globalThis ? true : this);    // 출력: true
        ```

    * **[Node.js](#런타임-환경)**

        모듈 형식의 Node.js에서 `this` 키워드는 [`module.exports`](#commonjs)를 반환하며, 이는 `global` 전역 객체와는 전혀 다른 존재이다.

        ```js
      console.log(this === globalThis ? true : this);    // 출력: {}
        ```

2. **[함수 문맥](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this#function_context)(function context)**

    * **[`function`](#함수) 키워드**

        일반적인 자바스크립트에서 함수는 `globalThis.func()`처럼 전역 객체에 종속된 함수로 받아들여져, 결과적으로 `this` 키워드는 `globalThis` 전역 객체를 가리킨다. 엄격 모드에서는 [`call()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 혹은 [`apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 등으로 `this` 키워드의 문맥을 직접 지정해야 하며, 만일 이러한 설정이 없으면 `undefined`를 반환한다.

        ```js
      /* 런타임 환경: Node.js (비엄격 모드) */
      
      function func() {
          console.log(this === globalThis ? true : this);
      }

      func();    // 출력: true
        ```
        ```js
      /* 런타임 환경: Node.js (엄격 모드) */
      
      function func() {
          "use strict";
          console.log(this === globalThis ? true : this);
      }

      func();    // 출력: undefined
        ```

    * **[화살표 함수 표현식](#함수)**

        화살표 함수 표현식은 `this` 키워드가 바인딩에 관여하지 않는다. 그러므로 `this` 키워드는 해당 함수를 내포하는 (혹은 정의하는) 객체를 가리킨다.

        ```js
      /* 런타임 환경: Node.js */
      
      const func = () => {
          console.log(this === globalThis ? true : this);
      };

      func();    // 출력: {}
        ```

## 콜백 함수
[콜백 함수](https://ko.wikipedia.org/wiki/콜백)(callback function)는 인자로 전달되는 함수이다. 콜백 함수를 전달받는 함수, 일명 호출 함수(calling function)는 블록 내에서 매개변수 호출을 통해 콜백 함수를 실행한다.

> 여기서 콜백이란, 전달인자로 전달된 함수가 다른 함수에서 언젠가 다시 호출(call back)되어 실행된다는 의미에서 붙여진 용어이다.

콜백 함수는 자바스크립트에서 매우 중요한 개념 중 하나이며 [비동기 입출력](#런타임-환경)을 가능케 하는 요인이다. 아래는 콜백 함수를 어떻게 인자로써 전달하고, 그리고 전달받은 호출 함수는 콜백 함수를 어떻게 활용하는지 보여준다. 여기서 [`function`](#함수) 키워드와 [화살표 함수 표현식](#함수) 중 무엇을 사용할 지는 [`this`](#this-키워드) 키워드가 가리키고자 하는 객체에 따라 결정한다.

```js
/* 호출 함수 */
function calling(func, arg) {

    // 콜백 함수의 호출
    return func(arg, 3.14159);
}

/* 콜백 함수 */
function callback(arg1, arg2) {
    return arg1 + arg2;
}

console.log(
    calling(callback, 1)
);
```
```terminal
4.14159
```

## 재귀 함수 
[재귀 함수](https://ko.wikipedia.org/wiki/재귀_(컴퓨터_과학))(recursive function)는 스스로를 호출하는 함수이다. 재귀 함수는 반드시 스스로를 호출하는 반복으로부터 탈출하는 기저 조건(base case)이 필요하다. 기저 조건이 없으면 무한 재귀가 발생하는데 프로그램 실행에 기여하는 [메모리](/docs/ko.C#스택-영역)가 부족하여 런타임 오류가 발생한다.

```js
/* 예제: 펙토리얼 "!" */
function factorial(arg) {
    // 기저 조건: 재귀로부터 탈출하는 조건
    if (arg == 1)
        return 1;
    else
        return arg * factorial(arg - 1);
}
```

# 객체
[객체](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)(object 혹은 instance)는 데이터를 저장할 수 있는 변수와 처리할 수 있는 함수를 하나로 묶은 데이터이다. 데이터나 함수를 바인딩할 수 있는 객체 내부의 식별자들을 속성(property)라고 하는데, 이들은 다음과 같이 [속성 접근자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors)로 접근한다. 함수가 바인딩된 속성을 메소드(method)라고도 부르는데, 반드시 [`function`](#함수) 키워드 혹은 ES6부터 소개된 [메소드 정의](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)를 사용한다.

> 반면 메소드는 절대 [화살표 함수 표현식](#함수)로 정의되지 말아야 한다! 이는 화살표 함수 표현식의 성질에 의해 [`this`](#this-키워드) 키워드가 객체를 가리키지 않기 때문이다.

| 객체 속성 | 속성 접근자                                    |
|:-------:|-----------------------------------------------|
| 변수 | `instance.property` 또는 `instance["property"]` |
| 함수 | `instance.method()` 또는 `instance["method"]()` |

현재까지 다룬 내용 중에서 객체에 해당되는 데이터로는 [전역 객체](#유효범위)와 [이터러블 객체](#이터러블)가 있다.

```js
var variable = [0, 3, 5, 9];
console.log(variable.indexOf(5));
// "variable" 배열 이터러블 객체의 "indexOf()" 메소드를 사용하여 값 5에 대한 위치를 반환한다.
```
```terminal
2
```

자바스크립트의 객체는 속성을 자유롭게 추가할 수 있는 특징을 가진다: 정의된 "선언 생략형" [변수](#변수)가 전역 객체의 속성처럼 접근될 수 있다는 점은 객체의 유연한 속성 확장을 잘 보여주는 [예시](#유효범위)이다. 다시 말해, 객체는 없던 속성도 언제든지 간단히 만들어 사용할 수 있다. 이러한 성질을 기반으로 자바스크립트는 아래의 방법으로 사용자 정의 객체를 생성한다.

* **[객체 초기자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)(object initializer)**

    객체 리터럴(object literal) `{}` 안에 속성을 `property: definition` 구문으로 초기화 (또는 정의)하여 객체를 생성한다. 초기자는 엄밀히 말해 객체가 아니기 때문에, `this` 키워드는 객체가 아닌 전역 문맥을 가리키는 점을 유의한다. 초기자로부터 객체가 생성된 이후에는 `this` 키워드로 객체를 불러 속성을 접근해야 하므로 메소드 안에 `this` 키워드가 사용된 것이다.

    ```js
  const instance = {
      
      // 속성 정의
      property1: 2,
      property2: 3.14,

      // 메소드 정의
      property3: function(arg) {
          return this.property1 + this.property2 - arg;
      }

      /* 동일:
          property3(arg) {
              return this.property1 + this.property2 - arg;
          }
      */
  };
    ```

* **[생성자 함수](https://developer.mozilla.org/en-US/docs/Glossary/Constructor)(constructor function)**

    [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) 연산자에 의해 함수는 객체를 만드는 [생성자](#생성자) 역할을 한다. 생성자는 객체를 만드는 과정에서 자동으로 실행되는 특수한 메소드인데, 이를 외부 함수로 구현한 것이라 볼 수 있다. 생성자 함수는 이미 객체의 일부로 간주되고 있기 때문에 속성과 메소드는 객체를 가리키는 `this` 키워드를 통해 정의되어야 한다.

    ```js  
  const instance = new func(2, 3.14);

  function func(arg1, arg2) {
      
      // 속성 정의
      this.property1 = arg1;
      this.property2 = arg2;

      // 메소드 정의
      this.property3 = function(arg) {
          return this.property1 + this.property2 - arg;
      }
  }
    ```

### 프로토타입
[프로토타입](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)(prototype)은 객체에 기본으로 내장된 속성 및 메소드를 일컬으며, 모든 자바스크립트 객체는 프로토타입을 갖는다. 프로토타입 또한 객체이므로 자신만의 프로토타입이 있는데 이러한 연속을 프로토타입 연쇄(prototype chain)라 부르고, 프로토타입이 `null`일 때까지 연쇄는 계속 이어진다. [`Object.create()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 메소드는 인자로 전달받은 객체를 프로토타입으로 갖는 새로운 객체를 생성한다.

> 객체의 프로토타입은 [`Object.getPrototypeOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) 메소드로 반환한다.

```js
const prototype = {

    // 속성 정의
    property1: value1,
    property2: value2,

    // 메소드 정의
    property3(arg) {
        return this.property1 + this.property2 - arg;
    }
};

const instance = Object.create(prototype);
```

## 클래스
[클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)(class)는 객체를 생성하는데 사용된다. ES6부터 소개된 클래스는 `class` 키워드를 사용하여 속성 및 메소드와 함께 정의되나 [클래스 호이스팅](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting#class_hoisting)(class hoisting)을 지원하지 않는다. 클래스로부터 객체를 생성하는 것을 "객체화(instantiation)"라 부르는데, 이때 클래스에 정의된 속성과 메소드들은 [캡슐화](https://ko.wikipedia.org/wiki/캡슐화)(encapsulation)되어 다음 특징을 갖는다:

1. 변수와 함수가 하나의 객체로 결합된다.
2. 우연치 않은 수정을 방지하기 위해 변수 및 함수에 대한 직접적인 접근을 외부로부터 제한할 수 있다.

```js
/* 클래스 정의 */
class CLASS {

    // 속성 정의
    property1 = 2;
    property2 = 3.14;

    // 메소드 정의
    method(arg) {
        return this.property1 + this.property2 - arg;
    }
}

/* 클래스 객체화 */
const instance = new CLASS();
```

함수의 `function` 키워드와 유사하게 식별자를 생략하므로써 익명 클래스로 활용할 수 있다. `new` 연산자 없이 간편히 객체를 생성할 수 있으나, 식별자가 없어 재활용이 불가능하다는 단점이 있다.

```js
/* 클래스 표현식 */
const instance = class {

    // 속성 정의
    property1 = 2;
    property2 = 3.14;

    // 메소드 정의
    method(arg) {
        return this.property1 + this.property2 - arg;
    }
};
```

### 생성자
[생성자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)(constructor)는 객체화마다 자동으로 실행되는 데이터 반환이 없는 `constructor()` 메소드이다. 비록 생성자는 선택사항이지만, 선언한다면 반드시 클래스명과 동일해야 한다. 흔히 객체화 단계에서 맴버들을 초기화하는 용도로 사용된다.

> 생성자를 별도로 정의하지 않으면 아래의 기본 생성자(default constructor)가 암묵적으로 클래스에 적용된다.
>
> ```js
> constructor() {}
> ```

```js
/* 클래스 정의 */
class CLASS
{
    /* 생성자 정의 */
    constructor(arg1, arg2) {
        this.property1 = arg1;
        this.property2 = arg2;

        statements;
    }
}

const instance = new CLASS(2, 3.14);
```

### 정적 속성 및 메소드
[정적 속성 및 메소드](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)(static property & method)는 클래스에서만 직접 접근이 가능한 속성 및 메소드이며 `static` 키워드로 정의된다. 이와 반대로 객체를 통해 접근되는 일반적인 속성 및 메소드와 별개의 영역으로 간주되므로 `static` 키워드만 확실히 표시해주면 동일한 식별자를 가질 수 있다.

> 다시 말해, 정적 속성 및 메소드는 단순히 클래스 자료형에 종속되어 있는 일반 변수 및 함수와 같다.

```js
/* 클래스 정의 */
class CLASS
{
    /* 정적 필드 정의 */
    static property = "Hello World!";

    /* 정적 메소드 정의 */
    static method(arg) {
        return this.property + arg;
    }
}

console.log(CLASS.method(7));            // 출력: Hello World!7

/* 클래스 객체화 */
const instance = new CLASS(2, 3.14);
console.log(instance.method(7));         // Uncaught TypeError: instance.method is not a function
```

### Setter 및 Getter
클래스는 하나의 속성을 getter와 setter 영역으로 나누어 [데이터 숨기기](https://en.wikipedia.org/wiki/Information_hiding)(data hiding)을 지원한다.

| 접근자 | 키워드 | 설명                                     |
|:--------:| ------- | ----------------------------------------------- |
| Getter   | [`get`][accessor-get]   | 속성으로부터 값을 반환받는 맴버이다. |
| Setter   | [`set`][accessor-set]   | 속성으로부터 값을 설정하는 맴버이다. |

[accessor-get]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get
[accessor-set]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set

속성을 나누므로써 수정되어서는 안될 민감한 코드를 setter 영역에 숨기고 getter 영역만을 통해서 데이터를 반환한다. 정의된 형태는 메소드와 유사하지만 실제로 사용할 때는 소괄호 `()` 없이 필드처럼 사용된다. Setter를 정의할 때 반드시 인자를 전달받을 매개변수 하나가 필요하며, 반대로 getter은 매개변수가 없지만 데이터를 반환할 [`return`](#return-반환문) 문이 있어야 한다.


```js
/* 클래스 정의 */
class CLASS {
    
    // Setter 정의
    set setter(arg) {
        this.property = arg + 1;
    }

    // Getter 정의
    get getter() {
        return this.property ** 2;
    }
}

/* 클래스 객체화 */
const instance = new CLASS();

instance.setter = 3;
console.log(instance.getter);    // 출력: 16
```

### Private 필드
속성은 클래스 및 객체 외부에서 접근이 가능한 [public](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields) 필드이며, 반대로 외부로부터 접근이 불가한 [private](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields) 필드도 존재한다. Private 필드는 식별자 앞에 해시 기호 `#`가 있는 게 특징이며, 속성(일명 public 필드)와 달리 `this` 키워드나 차후 객체화 이후에 추가될 수 없다. 단, 2021년 4월 기준으로 private 맴버는 아직 ECMAScript 실험 단계에 있어 본문의 예시 코드에서는 알 수 없는 구문으로 치부한다.

```js
/* 클래스 정의 */
class CLASS
{
    /* 생성자 정의 */
    public   = 2;
    #private = 3.14;
}

const instance = new CLASS();

console.log(instance.public);      // 출력: 2
console.log(instance.#private);    // SyntaxError: Private field '#private' must be declared in an enclosing class
```

## 상속
[상속](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)(inheritance)은 기반 클래스(base class)가 파생 클래스(derived class)에게 필드 및 메소드 맴버를 제공하는 행위이다. 파생 클래스는 [`extends`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends) 키워드로 하나의 기반 클래스로부터만 상속받을 수 있다. 기반 클래스와 파생 클래스에 동일한 이름의 속성과 메소드가 존재할 경우,

* **속성**: 기반 클래스의 속성은 파생 클래스에 명시된 데이터로 재할당된다.
* **메소드**: 기반 클래스의 메소드는 파생 클래스에 의해 묻혀진다.

```js
/* 기반 클래스 정의 */
class BASECLASS
{
    property1 = 3;
    property2 = "JavaScript";

    method(arg1, arg2) {
        return arg1 + arg2;
    }
}

/* 파생 클래스 정의 */
class DERIVEDCLASS extends BASECLASS
{
    property2 = "Hello World!";
    property3 = true;

    method(arg1, arg2) {
        return arg1 * arg2;
    }
}

/* 클래스 객체화 */
const instance = new DERIVEDCLASS();

console.log(`${instance.property1}, ${instance.property2}, ${instance.property3}`);
console.log(instance.method(2, 3));
```
```terminal
3 Hello World! true
6
```

### `super` 키워드
[`super`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super) 키워드는 파생 클래스로부터 기반 클래스의 객체 및 정적 메소드를 접근하기 위해 사용되며, 다음과 같은 용도로 활용된다.

* **`super.` 표현식**

    파생 클래스로부터 원하는 기반 클래스 맴버를 호출한다.

    ```js
  /* 기반 클래스 정의 */
  class BASECLASS {

      method() {
          return "Hello";
      }
  }

  /* 파생 클래스 정의 */
  class DERIVEDCLASS extends BASECLASS {

      method() {
          // 기반 클래스 메소드 호출
          let variable = super.method();
          console.log(`${variable} World!`);
      }
  }

  const instance = new DERIVEDCLASS();
  instance.method();                    // 출력: Hello World!
    ```

* **`super()` 표현식**

    파생 클래스의 생성자가 정의되어 있으면, 반드시 `this` 키워드가 사용되기 전에 `super()` 표현식으로 기반 클래스의 생성자를 호출해야 한다. 이는 객체화 과정에서 각 클래스가 갖는 생성자가 이중으로 실행되는 것을 방지한다. 만일 파생 클래스에 별도의 생성자가 정의되지 않으면 생략해도 무방하다.

    ```js
  /* 기반 클래스 정의 */
  class BASECLASS {
      constructor(arg1, arg2) {
          
      }
  }

  /* 파생 클래스 정의 */
  class DERIVEDCLASS extends BASECLASS {
      constructor(arg) {
          super(arg, arg);
      }
  }
    ```

# 모듈
[모듈](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)(module)은 부가적인 기능 및 데이터를 제공하는 자바스크립트 소스 코드이며, 일반 스크립트와 마찬가지로 `.JS` 혹은 일부 런타임에서 모듈로 인식할 수 있는 `.MJS` 확장자를 갖는다. 모듈은 정의된 변수, 함수, 클래스 및 객체 등을 제공하는 건 물론이고 타 모듈을 불러올 수 있다. 단, 모듈은 무조건 [엄격 모드](#엄격-모드)라는 점을 유의한다.

## ECMAScript 모듈
[ECMAScript 모듈](https://nodejs.org/api/esm.html) 규격은 자바스크립트 코드를 재활용할 수 있도록 패키징하는 공식 표준 형식이며, `import` 및 `export` 문을 활용하는 게 특징이다. [Node.js](#nodejs)은 ECMAScript 모듈을 지원하지만 기본 규격으로 채택되어 있지 않다. 해당 규격을 활성화하기 위해서는 프로젝트 경로에 `package.json` 파일을 생성하여 아래 코드를 기입한다.

```json
{
    "type": "module"
}
```

이러한 설정을 하지 않으면 Node.js에서 채택한 자바스크립트 패키징 규격인 [CommonJS](#commonjs)이 작용하여 구문 요류가 발생한다.

![Node.js 런타임 환경에서 ECMAScript 모듈 설정](./images/nodejs_package_ecmascript.png)

### `export` 문
[`export`](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) 문은 모듈이 제공하고자 하는 데이터를 지정하는데 사용된다.

* **유명 내보내기(named export)**

    모듈에서 데이터를 불러올 때 식별자를 명시해야만 접근할 수 있다. 중복된 식별자로 충돌이 우려되거나 너무 길면 `as` 키워드로 별칭을 지정할 수 있다.

    ```js
  /* 단일 유명 내보내기 */
  export object1;

  /* 다중 유명 내보내기 + 별칭 */
  export { object2, object3 as aliased };
    ```

* **기본 내보내기(default export)**

    모듈에서 데이터를 불러올 때 스크립트 측에서 식별자가 아니라 별칭을 지정하여 접근한다. 모듈 당 한 개의 데이터만 기본 내보내기로 지정될 수 있다.

    ```js
  export default object4;
    ```

이에 대한 자세한 내용을 이해하려면 모듈을 불러오는 [`import`](#import-문) 문을 함께 참고하도록 한다.

> 위의 유명 및 기본 내보내기는 하나의 문장으로 표현될 수 있다.
>
> ```js
> export { object4 as default, object1, object2, object3 as aliased };
> ```

데이터는 `export` 문에서 곧바로 정의될 수 있다. 특히 기본 내보내기는 익명의 함수 및 클래스 표현식을 사용하여 전달할 수 있다.

```js
/* 예시. 유명 내보내기 */
export function func() { }

/* 예시. 기본 내보내기 */
export default class { }
```

타 모듈에서 내보낸 데이터를 아무런 변동없이 있는 그대로 다시 내보낼 수 있다. 이러한 행위는 여러 모듈을 [`export from`](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#re-exporting_aggregating) 구문으로 집합(aggregate)시켜 하나의 모듈로 불러올 수 있도록 한다.

```js
export { object1, object2 } from "./module1.js";
export { object3, object4 } from "./module2.js";
```

### `import` 문
[`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) 문은 모듈에서 내보낸 데이터를 불러오는데 사용된다. 모듈로부터 데이터를 불러오는 방법은 크게 두 가지가 있다.

* **유명 불러오기(named import)**

    모듈에서 유명 내보내기로 전달된 데이터를 불러온다. 중복된 식별자로 충돌이 우려되거나 너무 길면 `as` 키워드로 별칭을 지정할 수 있다.

    ```js
  import { object1, object2, aliased as aka } from "./module.js";
    ```

* **기본 불러오기(default import)**

    모듈에서 기본 내보내기로 전달된 데이터를 원하는 별칭으로 불러온다.

    ```js
  import defaultExport from "./module.js";
    ```

모듈에서 내보낸 데이터는 [`export`](#export-문) 문을 참고한다.

> 위의 유명 및 기본 불러오기는 하나의 문장으로 표현될 수 있다.
>
> ```js
> import defaultExport, { object1, object2, aliased as aka } from "./module.js";
> ```

네임스페이스 불러오기(namespace import)는 모듈로부터 내보내진 데이터 전체를 [네임스페이스](https://ko.wikipedia.org/wiki/이름공간)(namespace)란 하나의 그룹으로 묶어서 불러온다.

```js
/* 네임스페이스 불러오기 */
import * as namespace from "./module.js";
```

데이터를 호출할 때에는 마치 객체의 속성을 접근하는 것처럼 `namespace.object` 구문을 사용한다. 단, 기본 내보내기로 전달된 데이터는 `namespace.default`로 호출한다.

## CommonJS
[CommonJS](https://nodejs.org/api/modules.html)은 본래 [Node.js](#nodejs)에서 사용하던 자바스크립트 코드 패키징 방식이다. Node.js은 각 파일을 하나의 모듈로 간주하는데, [`module`](https://nodejs.org/api/modules.html#the-module-object) 모듈 객체는 개별 모듈을 참조하는 것으로 국한된다. 해당 내용을 간단히 풀어 설명하면 다음 성질을 나타낸다.

1. 두 파일 간 `module` 객체는 서로 다른 존재이다.
2. `module` 객체는 모듈에 국한된 반면, [`global`](#유효범위) 객체는 프로젝트 전역을 참조하므로 서로 다른 존재이다.

### `exports` 객체
[`module.exports`](https://nodejs.org/api/modules.html#moduleexports) 객체 혹은 [`exports`](https://nodejs.org/api/modules.html#exports) 변수는 모듈에서 내보낼 데이터를 할당받는다.

* **`exports` 변수**

    간략화된 `exports` 변수는 속성을 추가하여 데이터를 할당받는 방식으로 동작한다. `exports` 변수 자체에 데이터를 할당하려는 시도는 아무런 영향이 없어 무의미하다.

    ```js
  exports.property = 3.14;
  exports.method = function(arg) {
      return this.property * arg;
  };
    ```

* **`module.exports` 객체**

    간략화된 `exports` 변수와 달리 함수, 객체, 클래스 등을 `module.exports`에 곧바로 할당할 수 있다.

    ```js
  module.exports = class {
      property = 3.14;
      method(arg) {
          return this.property * arg;
      }
  };
    ```

### `require()` 함수
[`module.require()`](https://nodejs.org/api/modules.html#modulerequireid) 메소드 혹은 [`require()`](https://nodejs.org/api/modules.html#requireid) 함수는 CommonJS에서 모듈뿐만 아니라 JSON, 로컬 파일 등을 불러온다. [`module.exports`](#export-객체)를 어떻게 사용하였는지에 따라 활용할 수 있는 예시를 아래에서 소개한다.

* 모듈이 `exports` 변수에서 속성을 확장하여 내보내었을 경우
 
    ```js
  const namespace = require("./module.js");

  cossole.log(namespace.method(2));    // 출력: 6.28
    ```

* 모듈이 `module.exports` 객체를 사용하여 내보낼 클래스를 정의한 경우
 
    ```js
  const CLASS = require("./module.js");
  const instance = new CLASS();

  console.log(instance.method(2));     // 출력: 6.28
    ```

# DOM
자바스크립트는 HTML 및 CSS와 함께 사용하여 다양한 기능을 제공하는데 기여한다. 선언형 언어인 HTML은 아래와 같이 [트리 구조](https://ko.wikipedia.org/wiki/트리_구조)의 [문서 객체 모델](https://ko.wikipedia.org/wiki/문서_객체_모델)(Document Object Model; DOM)로 문서를 표현한다.

![문서 객체 모델 <sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:DOM-model.svg">위키미디어</a></i></sub>](./images/js_html_dom.png)

자바스크립트는 HTML의 DOM에 접근하여 요소를 추가, 제거, 그리고 변경할 수 있어 동적이고 유연한 웹사이트를 제작할 수 있다. 트리 구조의 블록, 즉 `<html>`, `<head>`, `<h1>`과 같은 HTML 요소는 DOM에서 [노드](https://developer.mozilla.org/en-US/docs/Web/API/Node)(node)라고 부른다. 노드는 타 노드와 상하 관계가 존재하며, 이는 가족 구성에 빗대어 부모(parent), 자식(child), 그리고 형제(sibling)이라 부른다.

* **부모(parent)**: 해당 노드를 포함하고 있는 상위 노드
* **자식(child)**: 해당 노드가 포함하고 있는 하위 노드
* **형제(sibling)**: 해당 노드와 동일한 부모를 가지는 노드

본 장은 웹 브라우저를 기준으로 자바스크립트가 HTML 및 CSS와 상호작용하는 방법을 간략하게 설명한다.

## `document` 객체
[`window.document`](https://developer.mozilla.org/en-US/docs/Web/API/Document) 혹은 간단히 `document` 객체는 DOM의 노드를 최상위 권한으로 접근하는 데 사용된다. 노드 생성, 스타일 변경, 이벤트 설정 등은 전부 `document` 객체를 통해서 이루어진다.

### 요소 선택
DOM에서 원하는 요소 선택은 다음과 같은 `document` 객체 메소드를 통해 실현된다. 단, 여기서 말하는 속성(attribute)은 자바스크립트의 속성(property)과 다른 존재이다.

* **HTML 요소 태그**

    ```js
  // "DIV" 태그를 가진 index 번째 요소를 접근
  document.getElementsByTagName("DIV")[index];
    ```

* **HTML 요소의 [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) 속성**

    ```js
  // "CLASS"란 클래스를 가진 index 번째 요소를 접근
  document.getElementsByClassName("CLASS")[index];
    ```

* **HTML 요소의 [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) 속성**

    타 요소 접근 방식과 달리 배열이 사용되지 않았는데, `id` 속성은 DOM 내에서 하나의 요소에만 할당할 수 있는 유일성을 지녔기 때문이다.

    ```js
  // "ID"란 아이디를 가진 요소를 접근
  document.getElementById("ID");
    ```

이렇게 선택된 요소는 아래의 속성을 통해서 해당 요소의 부모, 자식, 혹은 형제 노드를 선택할 수도 있다.

| 속성                | 설명                                            |
|-------------------|------------------------|-----------------------------------------------|
| [`Node.parentNode`](https://developer.mozilla.org/en-US/docs/Web/API/Node/parentNode)      |  `Node`의 부모 노드를 호출한다.                          |
| [`Node.childNodes[index]`](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes)      |  `Node`의 `index` 번째 자식 노드를 호출한다.                     |
| [`Node.firstChild`](https://developer.mozilla.org/en-US/docs/Web/API/Node/firstChild)      |  `Node`의 첫 자식 노드를 호출한다.                        |
| [`Node.lastChild`](https://developer.mozilla.org/en-US/docs/Web/API/Node/lastChild)       |  `Node`의 마지막 자식 노드를 호출한다.                      |
| [`Node.hasChildNodes()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/hasChildNodes)   |  `Node`가 자식 노드를 가지면 `true`, 아니면 `false`를 반환한다. |
| [`Node.nextSibling`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nextSibling)     |  `Node`의 다음 형제를 호출한다.                          |
| [`Node.previousSibling`](https://developer.mozilla.org/en-US/docs/Web/API/Node/previousSibling) |  `Node`의 이전 형제를 호출한다.                          |

### 요소 스타일 변경
DOM은 각 요소를 개별 객체로 간주한다. 즉, 요소의 [속성](https://ko.wikipedia.org/wiki/HTML_속성)(attribute)들은 객체의 [속성](#객체)처럼 접근할 수 있다.

```html
<!-- 예시 HTML -->
<div id="SAMPLE">
    <img src="path/to/image1.png" style="width:400px; height:300px;">
    <span>그림 1. 예시 이미지</span>
</div>

<!-- 자바스크립트 -->
<script>
    /* "SAMPLE" 아이디를 가진 요소의 자식 노드 배열 접근 */
    const node = document.getElementById("SAMPLE").ChildNodes;
    
    /* 0 번째 자식 노드, 즉 <IMG> 요소 접근 및 스타일 변경 */
    node[0].src = "path/to/image2.png";
    node[0].style.width = "800px";
    node[0].style.height = "600px";
</script>
```

### 요소 생성
HTML이 아닌 자바스크립트를 사용해서 새로운 요소를 생성할 수 있다. 그러나 생성된 요소는 도표에서 설명한 대로 아직 DOM 어딘가에도 속하지 않은 상태이므로 웹사이트에 표시되지 않는다. 웹사이트에 나타나게 하기 위해서는 DOM에 추가해야 한다.

* **HTML 요소 생성**
    
    ```js
  // "DIV" 요소를 생성하지만, 아직 DOM 내에는 위치하지 않은 상태이다.
  document.createElement("DIV");
    ```

* **텍스트 요소 생성**

    ```js
  // "String" 텍스트를 가진 노드를 생성하지만, 아직 DOM 내에는 위치하지 않은 상태이다.
  document.createTextNode("String");
    ```


### 요소 추가 및 제거
DOM에 요소를 추가하는 방법은 다음과 같다.

| 메소드                         | 설명                                                |
|-----------------------------|---------------------------------------------------|
| [`Node.appendChild(node)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)         | 현재 노드에서 인자로 전달된 `node` 노드를 현시점 마지막 자식으로 추가한다. |
| [`Node.insertBefore(node,child)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore) | 현재 노드에서 인자로 전달된 `node` 노드를 `child` 자식의 다음 순서로 추가한다. |

DOM에 요소를 제거하는 방법은 다음과 같다.

| 메소드                         | 설명                                                |
|-----------------------------|---------------------------------------------------|
| [`NELement.remove()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/remove)         | 현재 노드를 제거한다. |
| [`Node.removeChild()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild) | 현재 노드에서 인자로 전달된 자식 노드를 제거한다. |

```html
<!-- 예시 HTML -->
<div>
    <p id="P1">첫 번째 문장.</p>
    <p id="P2">두 번째 문장.</p>
</div>

<!-- 자바스크립트 -->
<script>
    /* 텍스트 노드를 새로 생성된 <P> 요소에 삽입 */
    const textNode = document.createTextNode("JS로 생성된 텍스트.");
    const paraNode = document.createElement("p");
    paraNode.appendChild(textNode);
    
    /* 새로 생성된 <P> 요소를 <DIV> 자식 노드의 맨 끝에 추가 */
    document.getElementsByTagName("div")[0].appendChild(paraNode);
</script>
```

## 이벤트
자바스크립트는 클릭이나 키보드 입력, 혹은 입력창 데이터 제출 등과 같은 [이벤트](https://developer.mozilla.org/en-US/docs/Web/API/Event)(event)가 발생할 시 코드가 실행되도록 할 수 있다. 비록 이벤트는 HTML에서 인식하나, 해당 이벤트에 대한 동작(일명 [이벤트 처리자](https://developer.mozilla.org/en-US/docs/Web/Events/Event_handlers); event handler)은 자바스크립트에서 정의된다.

* HTML 소스 코드에 요소들이 어떠한 이벤트를 인식할지 미리 지정한다.

    ```html
  <div>
      <!-- <BUTTON> 요소는 클릭 이벤트 발생 시 "functionName()" 이벤트 처리자   실행 -->
      <button onclick="functionName()">클릭</button>
  </div>
  
  <!-- 자바스크립트 -->
  <script>
      /* "functionName()" 이벤트 처리자 */
      const functionName = () => {
          statements;
      }
  </script>
    ```

* 자바스크립트에서 직접 이벤트를 동적으로 할당한다.

    ```html
  <div>
      <!-- DOM으로 이벤트 지정 예정 -->
      <button>클릭</button>
  </div>
  
  <!-- 자바스크립트 -->
  <script>
      /* DOM을 통한 <BUTTON> 요소 이벤트 지정 및 이벤트 처리자 정의 */
      const variable = document.getElementsByTagName("BUTTON")[0];
  
      /* "functionName()" 이벤트 처리자 */
      variable.onclick = () => {
          statements;
      }
  </script>
    ```

* [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) 인터페이스를 활용하여 이벤트를 지정하는 동시에 이벤터 처리자를 함께 정의한다.



    ```html
  <div>
      <!-- DOM에서 "addEventListener()" 메소드로 이벤트 지정 예정 -->
      <button>클릭</button>
  </div>
  
  <!-- 자바스크립트 -->
  <script>
      /* "addEventListener()"로 <BUTTON> 요소 이벤트 지정 및 이벤트 처리자 정의   */
      const variable = document.getElementsByTagName("BUTTON")[0];
      variable.addEventListener("click", functionName);
      
      /* "functionName()" 이벤트 처리자 */
      function functionName() {
          statements;
          
          // 이벤트 실행 시, statements 이후 마지막에 이벤트 할당 해제: 일회용   이벤트
          variable.removeEventListener("click", functionName);
      }
  </script>
    ```

    여기서 주의할 점은 이벤트 이름이 이전 이벤트 관련 예시 코드와 다르다는 것이다.

| 메소드                  | 설명                                    |
| ------------------------------------------------------------------- | ---------------------------------------------- |
| [`EventTarget.addEventListener("click",func)`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)   | `func` 함수를 실행하는 `click` 이벤트를 해당 요소에 추가한다.   |
| [`EventTarget.removeEventListener("click",func)`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener) | `func` 함수를 실행하는 `click` 이벤트를 해당 요소로부터 제거한다. |

### 이벤트 전파
[이벤트 전파](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#example_5_event_propagation)(event propagation)은 이벤트 처리자의 실행 순서를 결정한다. 아래의 HTML 소스 코드를 예시로 들어본다.

```html
<div onclick="functionDIV()">
    <span onclick="functionSPAN()">
        Hello World!
    </span>
</div>
```

만일 "Hello World!" 텍스트를 클릭하였을 시, 어떤 요소의 이벤트 처리자가 우선적으로 실행되는가: `<DIV>` 아니면 `<SPAN>`인가?

* **캡쳐링 (capturing)**: DOM 트리 구조에서 상위 노드에서 하위 노드 순서로 내려간다 (우선 `<DIV>`, 이후 `<SPAN>`).
* **버블링 (bubbling)**: DOM 트리 구조에서 하위 노드에서 상위 노드 순서로 올라간다 (우선 `<SPAN>`, 이후 `<DIV>`).

이러한 이벤트 전파는 `addEventListener()` 메소드에서 `useCapture` 논리형 매개변수를 통해 설정할 수 있다. 기본적으로 `useCapture = false`로 버블링이 설정되어 있다.

| 예시                                                                   |
|:--------------------------------------------------------------------:|
| `EventTarget.addEventListener("click",func,useCapture)`                 |
| `useCapture`는 논리 자료형을 받는 매개변수로, `true`이면 캡쳐링이고 `false`이면 버블링으로 설정된다. |

## 반복 실행
자바스크립트는 하나의 함수를 일정 주기에 맞춰 반복적으로 실행하도록 할 수 있으며, 이는 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) 및 [`clearInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval) 메소드 쌍으로 구현할 수 있다.

| 메소드                               | 설명                                                   |
|----------------------------------|------------------------------------------------------|
| `setInterval(func,millisec)` | `func` 함수가 `millisec` [밀리초](https://ko.wikipedia.org/wiki/밀리초) 지연시간을 가지며 실행된다. |
| `clearInterval(setInterval)`     | `setInterval` 객체를 비활성화한다.                      |

```html
<!-- 예시 HTML -->
<div>
    <span>Hello World!</span>
</div>

<!-- 자바스크립트 -->
<script>
    /* "functionName()" 함수를 매 0.5초 동안 반복 실행 */
    let variable = setInterval(functionName, 500);

    var index = 0;    // 전역 변수
    const functionName = () => {
        /* 세 번 반복하도록 설정 */
        if (index == 3)
        {
            /* 반복 실행 해제 */
            clearInterval(variable);
        }
        else
        {
            statements;
            index++;    // 전역 변수이기에 값이 유지된다.
        }
    }
</script>
```

이러한 반복 실행은 결국 HTML 요소의 위치 이동 및 스타일이 시간에 따라 서서히 변하는 애니메이션 동작을 구현하는데 활용된다.

# jQuery
jQuery는 자바스크립트 라이브러리 중 하나로 HTML DOM 구조 이동, 제어, 그리고 이벤트 처리 등을 간략화하는데 목적을 둔다. 해당 특징들은 많은 웹사이트에서는 jQuery 라이브러리 사용을 독려하여, [W3Techs](https://w3techs.com/technologies/overview/javascript_library) 통계에 의하면 75% 이상의 전세계 웹사이트에서 jQuery를 사용하는 매우 유명한 라이브러리이다. 본 장에서는 jQuery를 사용하는 방법에 대해서 소개한다.

라이브러리 파일은 `jquery-3.x.y.min.js`과 `jquery-3.x.y.js`로 나뉘어지는데, 전자는 압축된 배포용이며 후자는 개발용으로 사용된다. jQuery 라이브러리를 스크립트로 불러오는 방법은 두 가지가 있다:

* 첫 번째로는 jQuery 자바스크립트 파일을 [다운로드](https://jquery.com/download/)하여 사용하는 것이다. 만일 jQuery 3.5.1 배포용을 불러오고 싶으면 다음과 같이 코드를 `<head>` 태그 아래에 입력한다.

  ```html
  <head>
      <!-- 로컬 JQUERY 라이브러리 불러오기 -->
      <script src="jquery-3.5.1.min.js"></script>
  <head>
  ```

* 두 번째 방법으로는 온라인에서 jQuery 라이브러리만 전달하는 CDN을 통해 불러올 수 있다. 다음과 같이 코드를 `<head>` 태그 아래에 입력하여 jQuery 3.5.1 배포용 라이브러리를 Google 서버에서 직접 불러온다.

  ```html
  <head>
      <!-- CDN JQUERY 라이브러리 불러오기 -->
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  </head>
  ```
