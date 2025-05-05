# 프로그래밍
**[프로그래밍](https://en.wikipedia.org/wiki/Computer_programming)**(programming), 혹은 간단히 **코딩**(coding)은 컴퓨터가 수행할 작업들의 집합체인 [프로그램](https://en.wikipedia.org/wiki/Computer_program)을 설계 및 작성하는 행위를 가리킨다. 프로그램 개발에 활용될 수 있는 프로그래밍 언어는 다양하며, 각자 추구하려는 목적과 철학에 따라 설계되었기 때문에 언어마다 제각각 장단점을 지닌다. 프로그램을 생성하거나 실행하는 방식 또한 서로 다르기 때문에, 본문은 크게 두 가지 부류의 프로그래밍 언어를 소개한다.

1. [컴파일러](#컴파일러)에 의해 다른 언어의 코드로 변환하여 실행하는 "**[컴파일 언어](https://en.wikipedia.org/wiki/Compiled_language)**"
1. [인터프리터](#인터프리터)에 의해 CPU가 실행해야 한 작업을 안내하는 "**[인터프리트 언어](https://en.wikipedia.org/wiki/Interpreter_(computing))**"

## 컴파일러
**[컴파일러](https://en.wikipedia.org/wiki/Compiler)**(compiler)는 하나의 프로그래밍 언어를 다른 언어로 변환(일명 컴파일; compile)하는 언어 처리 소프트웨어이다.

컴파일러를 사용하는 대표적인 [C](C.md)/[C++](Cpp.md) 프로그래밍 언어는 [CPU](Processor.md)가 직접적으로 수행할 수 있는 [기계어](#기계어)로 컴파일되다 보니, 흔히 [고급 프로그래밍 언어](https://en.wikipedia.org/wiki/High-level_programming_language)를 [저급 프로그래밍 언어](https://en.wikipedia.org/wiki/Low-level_programming_language)로 변환하는 국한적인 작업으로 오해한다. 하지만 [파이썬](Python.md) 및 자바 프로그래밍 언어는 [바이트코드](#바이트코드)라는 [중간 언어](https://en.wikipedia.org/wiki/Intermediate_representation)로 변환하는 컴파일러가 존재하며, 심지어 [타입스크립트](TypeScript.md) 프로그래밍 언어의 컴파일러는 [자바스크립트](JavaScript.md)라는 또 다른 고급 프로그래밍 언어로 변환한다.

>  프로그래밍 언어는 설계된 당시의 철학과 기술적 한계를 효율적으로 극복하기 위해 여러 기술들을 복합적으로 작용하면서 컴파일 언어와 [인터프리트 언어](#인터프리터)의 경계를 허물고 있다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">컴파일 작업 유형의 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Ahead-of-time_compilation">AOT 컴파일</a></th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Just-in-time_compilation">JIT 컴파일</a></th></tr></thead><tbody><tr><td style="text-align: center;">Ahead-of-time compilation</td><td style="text-align: center;">Just-in-time compilation</td></tr><tr><td>프로그램을 실행하기 전에 미리 다른 언어로 변환하는 컴파일 작업이다. 컴파일을 마친 프로그램은 추가 작업이 필요없이 실행되므로 속도가 매우 빠르다는 장점을 가진다. 다만, 변경된 소스 코드를 시험하기 위해서는 새로 컴파일해야 하는 불편함이 다소 존재한다.</td><td>프로그램을 실행하는 <a href="https://en.wikipedia.org/wiki/Execution_(computing)#Runtime">런타임</a> 도중에 다른 언어로 변환하는 컴파일 작업이다. 전통적인 AOT 컴파일의 빠른 실행 속도와 인터프리터의 유연함을 모두 지니지만, 동시에 두 기술의 <a href="https://en.wikipedia.org/wiki/Overhead_(computing)">오버헤드</a>가 중첩(즉, <a href="https://en.wikipedia.org/wiki/Compile_time">컴파일 타임</a> + <a href="https://en.wikipedia.org/wiki/Link_time">링크 타임</a> + 인터프리트 오버헤드)되는 단점도 공존한다.</td></tr><tr><td><ul><li>C/C++&nbsp;<sub>(소스 코드를 <a href="https://en.wikipedia.org/wiki/Object_code">오브젝트 파일</a>로 컴파일하고 <a href="https://en.wikipedia.org/wiki/Linker_(computing)">링커</a>로 연동시켜 <a href="https://en.wikipedia.org/wiki/Software_build">빌드</a>)</sub></li><li>Rust</li></ul></td><td><ul><li>C#</li><li>Java</li></ul></td></tr></tbody></table>

아래는 C++ 프로그래밍 언어의 CPP 확장자 소스 코드(左)가 [MSVC](https://en.wikipedia.org/wiki/Microsoft_Visual_C++) 컴파일러에 의해 기계어로 구성된 EXE 실행 파일(右)로 컴파일 된 예시이다.

![C++ 프로그래밍 소스 코드, 그리고 x64 아키텍처 기계어로 컴파일된 이진 실행 파일](./images/programming_compiler_example.png)

### 기계어
**[기계어](https://en.wikipedia.org/wiki/Machine_code)**(maachine code)는 컴퓨터 [프로세서](Processor.md)(일명  CPU)의 연산 및 동작을 직접 제어할 수 있는 0과 1만으로 구성된 [이진 코드](https://en.wikipedia.org/wiki/Binary_code)이다. 비록 프로그래밍 언어는 영문으로 작성되지만, 컴퓨터가 작업을 수행하기 위해서는 결국 기계어로 변환되어야 한다. 만일 [어셈블리](Assembly.md)에 능통하면 기계어로 직접 프로그래밍이 가능하다. CPU에는 [x86](https://en.wikipedia.org/wiki/X86), [ARM](https://en.wikipedia.org/wiki/ARM_architecture_family) 등 다양한 [아키텍처](https://en.wikipedia.org/wiki/Instruction_set_architecture)가 있는데, 각각 연산 및 동작을 수행하기 위한 이진 코드가 설계상 이유로 서로 다르다. 예를 들어, 동일한 [윈도우 OS](Windows.md)라도 [x86-64](https://en.wikipedia.org/wiki/X86-64)의 [인텔 코어](https://en.wikipedia.org/wiki/Intel_Core) 및 [AMD 라이젠](https://en.wikipedia.org/wiki/Ryzen)에서 문제가 없던 프로그램이 [ARM64](https://en.wikipedia.org/wiki/AArch64)의 [퀄컴 스냅드래곤](https://en.wikipedia.org/wiki/Qualcomm_Snapdragon)에서는 실행 불가하다.

### 바이트코드
**[바이트코드](https://en.wikipedia.org/wiki/Bytecode)**(bytecode)는 소스 코드에서 기계어로 변환하는데 징검다리 역할을 하는 [중간 언어](https://en.wikipedia.org/wiki/Intermediate_representation)이다. 수행할 연산 혹은 동작을 나타내는 [명령 코드](https://en.wikipedia.org/wiki/Opcode)(opcode)가 한 [바이트](https://en.wikipedia.org/wiki/Byte)(byte) 내에서 표현되기 때문에 바이트코드("바이트" + <sub>명령</sub>"코드")라는 명칭에서 유래되었다. [기계어](#기계어)와 유사하지만 바이트코드는 CPU 아키텍처에 의존하지 않고 소프트웨어에서 처리된다.
        
> 바이트코드도 전부 동일한 건 아니다: [파이썬](Python.md)과 자바는 바이트코드를 생성하는 대표적인 언어이지만, 명령 코드와 인자 여부 등이 달라 서로 호환되지 않는다. 즉, 바이트코드는 이를 처리하는 소프트웨어에 의존한다.

## 인터프리터
**[인터프리터](https://en.wikipedia.org/wiki/Interpreter_(computing))**(interpreter)는 소스 코드를 [CPU](Processor.md)가 직접 수행할 수 있는 [기계어](#기계어)로 [컴파일](#컴파일러)하지 않고서도 곧바로 실행할 수 있도록 하는 소프트웨어이다. 이러한 특성은 소스 코드가 어떤 시스템에서도 동일하게 실행될 수 있는 [크로스 플랫폼](https://en.wikipedia.org/wiki/Cross-platform_software) 성질을 보여준다. 소스 코드로부터 수행되어야 할 동작을 인터프리터가 대신 실행하기 때문에 기계어로의 컴파일이 불필요하다.

> 소스 코드는 크로스 플랫폼이겠지만, 이를 실행하는 인터프리터는 시스템 아키텍처에 따라 별도로 [빌드](https://en.wikipedia.org/wiki/Software_build)되어야 한다.

아래는 일부 프로그래밍 언어의 인터프리터가 무엇으로부터 빌드되었는지 나열한다.

<table style="table-layout: fixed; width: 50%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">프로그래밍 언어의 인터프리터 및 빌드에 사용된 언어</caption><thead><tr><th style="text-align: center;">프로그래밍 언어</th><th style="text-align: center;">인터프리터</th><th style="text-align: center;">인터프리터 빌드 언어</th></tr></thead><tbody><tr><td style="text-align: center;">자바</td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Java_virtual_machine">Java Virtual Machine</a></td><td style="text-align: center;"><a href="Cpp.md">C++</a></td></tr><tr><td style="text-align: center;"><a href="Python.md">파이썬</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/CPython">CPython</a></td><td style="text-align: center;"><a href="C.md">C</a></td></tr><tr><td style="text-align: center;"><a href="Python.md">파이썬</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Jython">Jython</a></td><td style="text-align: center;">자바</td></tr></tbody></table>

[런타임](https://en.wikipedia.org/wiki/Execution_(computing)#Runtime) 도중에 인터프리터는 실행할 코드를 분석하여 해당하는 동작을 수행하는데, 이 과정에서 발생하는 [오버헤드](https://en.wikipedia.org/wiki/Overhead_(computing))는 컴파일러로 빌드된 실행 파일보다 저하된 실행 속도를 보여준다. 하지만 [컴파일 타임](https://en.wikipedia.org/wiki/Compile_time)까지 모두 고려하면 오히려 컴파일러보다 소모되는 시간이 더 짧으므로, 인터프리터의 전체적인 시간 효율은 소스 코드가 자주 변경되는 개발 단계에서 유리하게 작용한다.

인터프리터는 소스 코드를 실행하는 세 가지 전략 중 하나를 구사한다:

1. 소스 코드의 [문장](https://en.wikipedia.org/wiki/Statement_(computer_science))을 있는 그대로 하나씩 분석하고 수행한다.
2. 소스 코드를 보다 효율적인 [중간 언어](https://en.wikipedia.org/wiki/Intermediate_representation) 혹은 [오브젝트 코드](https://en.wikipedia.org/wiki/Object_code)로 변환(혹은 컴파일)을 마친 즉시 이를 실행한다.
3. 컴파일러로부터 미리 생성된 [바이트코드](#바이트코드)를 이에 부합하는 인터프리터 [가상 머신](#프로세스-가상-머신)을 통해 실행합니다.

아래는 인터프리트의 2번 전략을 활용하는 [파이썬](Python.md) 프로그래밍 언어의 PY 스크립트 파일(左)에 작성된 소스 코드(右上)와 인터프리터로 실행된 [프로세스](Process.md)(右下) 예시이다.

![파이썬 스크립트 파일의 소스 코드와 런타임 프로세스](./images/programming_interpreter_example.png)

### 프로세스 가상 머신
**[프로세스 가상 머신](https://en.wikipedia.org/wiki/Virtual_machine#Process_virtual_machines)**(process virtual machine; 프로세스 VM)은 비록 명칭이 가상 "머신(장치)"이지만 시스템 운영체제에서 단일 [프로세스](Process.md#프로세스) 실행을 지원하는 어플리케이션이다. 하드웨어나 운영체제 상관없이 플랫폼 독립적인 프로그래밍 환경을 제공하여 프로세스가 동일하게 실행될 수 있도록 보장하는 게 목적이다. 흔히 인터프리터와 성질이 비슷하여 혼용되어 일컫는 경우가 자자하지만, 프로세스를 실행하는 인터프리터는 프로세스 실행 환경을 제공하는 프로세스 VM의 구성요소 중 하나로 간주된다.

대표적인 프로세스 VM으로 [자바 플랫폼](https://en.wikipedia.org/wiki/Java_(software_platform))의 자바 가상 머신(Java virtual machine; JVM) 그리고 [.NET](Csharp.md#net)의 [공통 언어 런타임](https://en.wikipedia.org/wiki/Common_Language_Runtime)(Common Language Runtime; CLR)이 있다. 이들은 한 프로그래밍 언어에 국한되지 않고 다양한 언어를 지원할 수 있다.

* JVM: 자바, 코틀린, 그루비 등
* CLR: [C#](Csharp.md), F#, VB.NET 등

위의 프로세스 VM들은 인터프리터 뿐만 아니라 지원하는 각 프로그래밍 언어의 소스 코드를 중간 언어로 변환하는 컴파일러를 가지고 있기 때문에 포괄적인 "가상 머신"이라고 언급된다. 파이썬 프로그래밍 언어도 마찬가지로 프로세스 VM이 존재하지만 한 가지 언어만을 지원하고 인터프리터의 성질이 상당히 부각되는 점에서 오히려 "인터프리터"라고 불린다.
