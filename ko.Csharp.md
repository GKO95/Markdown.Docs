---
category: 프로그래밍
title: C#
---
# C#
[C#](https://learn.microsoft.com/en-us/dotnet/csharp/)(한국어:씨샵)은 마이크로소프트에서 개발한 [객체지향 프로그래밍 언어](https://ko.wikipedia.org/wiki/객체_지향_프로그래밍)(일명 OOP)이며, [자바](https://ko.wikipedia.org/wiki/자바_(프로그래밍_언어))와 유사하게 설계되었지만 문법적으로 [C](ko.C)/[C++](ko.Cpp) 언어를 연상케 한다. [.NET](#net)의 지원으로 [데스크탑](https://ko.wikipedia.org/wiki/응용_소프트웨어), [모바일](https://ko.wikipedia.org/wiki/모바일_응용_소프트웨어), 그리고 [웹 어플리케이션](https://ko.wikipedia.org/wiki/웹_애플리케이션)에도 활용되는 범용성을 지닌다. 간단함과 편리함을 추구하는데, 만일 C 유형의 언어에 이미 익숙한 개발자들에게 C# 언어는 쉽게 배울 수 있는 장점을 지닌다. (비록 한국에서는 인지도가 낮으나) 폭넓은 커뮤니티를 보유하고 있어 관련 자료 검색에도 용이하다.

OOP 언어는 "객체"라는 데이터를 위주로 프로그램을 개발되며, 아래는 객체에 대한 관련 용어을 간략히 소개한다:

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">OOP 핵심 용어의 간략 소개</caption>
<colgroup><col style="width: 33.3%;"/><col style="width: 33.4%;"/><col style="width: 33.3%;"/></colgroup>
<thead><tr><th style="text-align: center;">객체</th><th style="text-align: center;">맴버</th><th style="text-align: center;"><a href="#클래스">클래스</a></th></tr></thead>
<tbody><tr style="vertical-align: top; text-align: center;"><td>데이터를 저장한 공간(<a href="#변수">필드</a>)과 동작을 수행하는 코드(<a href="#함수">메소드</a>)의 묶음</td><td>객체 구성원: 필드, 메소드 등</td><td>맴버들이 정의되어 객체를 생성하는 <a href="#자료형">자료형</a></td></tr>
</tbody>
</table>

## .NET
[.NET](https://ko.wikipedia.org/wiki/닷넷)은 마이크로소프트에서 개발한 [오픈소스](https://github.com/dotnet/dotnet) [소프트웨어 프레임워크](https://ko.wikipedia.org/wiki/소프트웨어_프레임워크)이며, 프로그래밍에 방대하고 다양한 기능과 지원을 제공하는 핵심 요소이다. 국제표준기구 ISO와 ECMA에서 표준으로 채택된 공통 언어 기반(Common Language Infrasturcture; CLI)이 적용되어 운영체제 및 아키텍쳐가 다르더라도 [크로스 플랫폼](https://ko.wikipedia.org/wiki/크로스_플랫폼)(cross-platform)을 지원해야 한다.

.NET 컴파일러 플랫폼, 일명 <a href="https://en.wikipedia.org/wiki/Roslyn_(compiler)">Roslyn</a>은 소스 코드에서 이진코드가 아닌 [바이트코드](https://ko.wikipedia.org/wiki/바이트코드)를 [공통 중간 언어](https://ko.wikipedia.org/wiki/공통_중간_언어)(Common Intermediate Language; CIL)로 채택한 [어셈블리](https://learn.microsoft.com/en-us/dotnet/standard/assembly/) 파일을 생성한다. 바이트코드 특성상 컴퓨터가 읽을 수 있는 기계어는 아니지만 아키텍처 독립 언어로 크로스 플랫폼 지원이 가능하여, C# 언어 이외에도 .NET은 [비주얼 베이직](https://ko.wikipedia.org/wiki/비주얼_베이직_닷넷), [F#](https://ko.wikipedia.org/wiki/F_샤프), 그리고 [파워셸](ko.PowerShell.md) 언어도 지원한다.

> 어셈블리는 .exe 혹은 .dll 파일로 컴파일되지만, <a href="https://ko.wikipedia.org/wiki/기계어">기계어</a>가 아니기 때문에 단독적으로 실행이 불가하며 .NET CLR의 추가 [JIT 컴파일](ko.Compiler.md#jit-컴파일)이 반드시 필요하다.

![.NET 공통 언어 기반(CLI)](https://upload.wikimedia.org/wikipedia/commons/8/85/Overview_of_the_Common_Language_Infrastructure.svg)

프레임워크는 FCL(혹은 CoreFX) 그리고 CLR(혹은 CoreCLR)로 구성되어 있으며 아래의 표는 이들의 역할을 간략하게 설명한다:

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">.NET 구성요소</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Framework_Class_Library">프레임워크 클래스 라이브러리</a>(FCL)</th><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/공통_언어_런타임">공통 언어 런타임</a>(CLR)</th></tr></thead>
<tbody>
<tr><td>.NET 프로그램 개발에 필요한 <a href="https://ko.wikipedia.org/wiki/표준_라이브러리">표준 라이브러리</a>를 제공한다.</td><td><a href="ko.Compiler.md#jit-컴파일">JIT 컴파일러</a>를 통해 .NET 프로그램을 컴파일 및 실행한다.</td></tr>
</tbody>
</table>

### .NET 프레임워크
[.NET 프레임워크](https://ko.wikipedia.org/wiki/닷넷_프레임워크)(.NET Framework)는 .NET 이전에 활발히 사용되던 [윈도우 NT](ko.Windows.md) 데스크탑에서만 사용할 수 있는 프레임워크이다. 비록 2020년 11월부로 [.NET Core](#net)(현재 .NET)가 주요 프레임워크로 전환되었으나, 마이크로소프트는  [누적 업데이트](ko.Update.md#waas)를 배포하는 등 .NET 프레임워크를 [윈도우 OS](ko.Windows.md)에서 제거할 계획이 없음을 [발표](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-5#net-5-doesnt-replace-net-framework)했다. 다만, 새로운 어플리케이션 개발에 있어서는 [.NET 6](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-6) 버전 이상을 사용하는 걸 적극 권장한다.

> .NET 프레임워크는 4.x 버전 번호를 유지할 것으로 보이며, 현재까지도 새로운 빌드가 [출시](https://learn.microsoft.com/en-us/dotnet/framework/whats-new/) 및 [지원](https://learn.microsoft.com/en-us/lifecycle/products/microsoft-net-framework)되고 있다.

## 프로젝트
다음은 비주얼 스튜디오 2022을 위주로 C# 프로젝트 구축에 대하여 설명한다.

![비주얼 스튜디오 C# 프로그래밍 화면](./images/visual_studio_csharp.png)

[C# 9.0](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9) 업데이트를 기점으로 비주얼 스튜디오에서 C# 콘솔 프로젝트를 생성할 때 [새로운 템플릿](https://learn.microsoft.com/en-us/dotnet/core/tutorials/top-level-templates)이 제공되며, 아래는 이전과 이후 소스 코드를 비교한다.

<table style="table-layout: fixed; width: 95%; margin: auto;">
<caption style="caption-side: top;">C# 9.0 전후 콘솔 프로젝트 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">C# 9.0 이전</th><th style="text-align: center;">C# 9.0 이후</th></tr></thead>
<tbody>
<tr style="text-align: center;"><td>초창기 .NET 5 또는 이전 버전</td><td>현재 .NET 5 또는 이후 버전</td></tr><tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```cs
using System;

namespace Experiment
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```
</td><td>

```cs
Console.WriteLine("Hello World!");
```
</td></tr><tr><td><code>Main()</code> 진입점이 C# 프로젝트명의 <a href="#네임스페이스">네임스페이스</a> 안에 위치한다. 객체지향 프로그래밍의 성격이 잘 드러나는 정석적인 코드이다.</td><td><code>Main()</code> 진입점이 컴파일러에 의해 <a href="#전역-네임스페이스">전역 네임스페이스</a>에 생성된다. <a href="https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/top-level-statements">최상위 문장</a>(top-level statements)은 코드를 간결하게 표현하지만 암묵적인 규칙들이 존재한다.</td></tr>
</tbody>
</table>

본 문서는 가독성을 위해 후자에서 선보인 최상위 문장이 작용한 코드를 예시로 활용할 예정이다.
