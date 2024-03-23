# 인터프리터
[인터프리터](https://ko.wikipedia.org/wiki/인터프리터)(interpreter)는 소스 코드를 [CPU](Processor.md)가 직접 수행할 수 있는 [기계어](Compiler.md#기계어)로 [컴파일](Compiler.md)하지 않고서도 곧바로 실행할 수 있도록 하는 소프트웨어이다. 이러한 특성은 소스 코드가 어떤 시스템에서도 동일하게 실행될 수 있는 [크로스 플랫폼](https://ko.wikipedia.org/wiki/크로스_플랫폼)(cross platform) 성질을 보여준다. 소스 코드로부터 수행되어야 할 동작을 인터프리터가 대신 실행하기 때문에 기계어로의 컴파일이 불필요하다.

> 소스 코드는 크로스 플랫폼이겠지만, 이를 실행하는 인터프리터는 시스템 아키텍처에 따라 별도로 [빌드](https://ko.wikipedia.org/wiki/소프트웨어_빌드)되어야 한다.

아래는 일부 프로그래밍 언어의 인터프리터가 무엇으로부터 빌드되었는지 나열한다.

<table style="table-layout: fixed; width: 50%; margin-left: auto; margin-right: auto;">
<caption style="caption-side: top;">프로그래밍 언어의 인터프리터 및 빌드에 사용된 언어</caption>
<thead><tr><th style="text-align: center;">프로그래밍 언어</th><th style="text-align: center;">인터프리터</th><th style="text-align: center;">인터프리터 빌드 언어</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><a href="https://www.java.com/">자바</a></td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/자바_가상_머신">Java Virtual Machine</a></td><td style="text-align: center;"><a href="Cpp.md">C++</a></td></tr>
<tr><td style="text-align: center;"><a href="Python.md">파이썬</a></td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/C파이썬">CPython</a></td><td style="text-align: center;"><a href="C.md">C</a></td></tr>
<tr><td style="text-align: center;"><a href="Python.md">파이썬</a></td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/자이썬">Jython</a></td><td style="text-align: center;"><a href="https://www.java.com/">자바</a></td></tr>
</tbody>
</table>

[런타임](https://ko.wikipedia.org/wiki/런타임) 도중에 인터프리터는 실행할 코드를 분석하여 해당하는 동작을 수행하는데, 이 과정에서 발생하는 [오버헤드](https://ko.wikipedia.org/wiki/오버헤드)는 컴파일러로 빌드된 실행 파일보다 저하된 실행 속도를 보여준다. 하지만 [컴파일 타임](https://ko.wikipedia.org/wiki/컴파일_타임)까지 모두 고려하면 오히려 컴파일러보다 소모되는 시간이 더 짧으므로, 인터프리터의 전체적인 시간 효율은 소스 코드가 자주 변경되는 개발 단계에서 유리하게 작용한다.

인터프리터는 소스 코드를 실행하는 세 가지 전략 중 하나를 구사한다:

1. 소스 코드의 [문장](https://ko.wikipedia.org/wiki/문_(프로그래밍))(statement)을 있는 그대로 하나씩 분석하고 수행한다.
2. 소스 코드를 보다 효율적인 [중간 언어](https://ko.wikipedia.org/wiki/중간_표현) 혹은 [오브젝트 코드](https://ko.wikipedia.org/wiki/목적_파일)로 변환(혹은 컴파일)을 마친 즉시 이를 실행한다.
3. 컴파일러로부터 미리 생성된 [바이트코드](#바이트코드)를 이에 부합하는 인터프리터 [가상 머신](#프로세스-가상-머신)(virtual machine)을 통해 실행합니다.

아래는 인터프리트의 2번 전략을 활용하는 [파이썬](Python.md) 프로그래밍 언어의 `.PY` 확장자 스크립트 파일(左)에 작성된 소스 코드(右上)와 인터프리터로 실행된 [프로세스](Process.md)(右下) 예시이다.

![파이썬 스크립트 파일의 소스 코드와 런타임 프로세스](./images/programming_interpreter_example.png)

### 바이트코드
[바이트코드](https://ko.wikipedia.org/wiki/바이트코드)(bytecode)는 소스 코드에서 기계어로 변환하는데 징검다리 역할을 하는 [중간 언어](https://ko.wikipedia.org/wiki/중간_표현)(intermediate language)이다. 수행할 연산 혹은 동작을 나타내는 [명령 코드](https://ko.wikipedia.org/wiki/명령_코드)(opcode)가 한 [바이트](https://ko.wikipedia.org/wiki/바이트)(byte) 내에서 표현되기 때문에 바이트코드("바이트" + <sub>명령</sub>"코드")라는 명칭에서 유래되었다. 기계어와 유사하지만 바이트코드는 CPU 아키텍처에 의존하지 않고 소프트웨어에서 처리된다.
        
> 바이트코드도 전부 동일한 건 아니다: 파이썬과 자바는 바이트코드를 생성하는 대표적인 언어이지만, 명령 코드와 인자 여부 등이 달라 서로 호환되지 않는다. 즉, 바이트코드는 이를 처리하는 소프트웨어에 의존한다.

## 프로세스 가상 머신
[프로세스 가상 머신](https://ko.wikipedia.org/wiki/가상_머신#프로세스_가상_머신[1])(process virtual machine; 프로세스 VM)은 비록 명칭이 가상 "머신(장치)"이지만 시스템 운영체제에서 단일 [프로세스](Process.md#프로세스) 실행을 지원하는 어플리케이션이다. 하드웨어나 운영체제 상관없이 플랫폼 독립적인 프로그래밍 환경을 제공하여 프로세스가 동일하게 실행될 수 있도록 보장하는 게 목적이다. 흔히 인터프리터와 성질이 비슷하여 혼용되어 일컫는 경우가 자자하지만, 프로세스를 실행하는 인터프리터는 프로세스 실행 환경을 제공하는 프로세스 VM의 구성요소 중 하나로 간주된다.

대표적인 프로세스 VM으로 [자바 플랫폼](https://ko.wikipedia.org/wiki/자바_(소프트웨어_플랫폼))의 자바 가상 머신(Java virtual machine; JVM) 그리고 [.NET](Csharp.md#net)의 [공통 언어 런타임](https://ko.wikipedia.org/wiki/공통_언어_런타임)(Common Language Runtime; CLR)이 있다. 이들은 한 프로그래밍 언어에 국한되지 않고 다양한 언어를 지원할 수 있다.

* JVM: [자바](https://www.java.com/), 코틀린, 그루비 등
* CLR: [C#](Csharp.md), F#, VB.NET 등

위의 프로세스 VM들은 인터프리터 뿐만 아니라 지원하는 각 프로그래밍 언어의 소스 코드를 중간 언어로 변환하는 컴파일러를 가지고 있기 때문에 포괄적인 "가상 머신"이라고 언급된다. 파이썬 프로그래밍 언어도 마찬가지로 프로세스 VM이 존재하지만 한 가지 언어만을 지원하고 인터프리터의 성질이 상당히 부각되는 점에서 오히려 "인터프리터"라고 불린다.

# 같이 보기
* [컴파일러](Compiler.md)
