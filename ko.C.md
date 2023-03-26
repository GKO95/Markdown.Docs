---
category: 프로그래밍
title: C
---
# C
> *참조: [Microsoft Docs C 언어 설명서](https://learn.microsoft.com/en-us/cpp/c-language/)*

C 언어는 [유닉스](https://ko.wikipedia.org/wiki/유닉스)(UNIX) 컴퓨터를 위한 소프트웨어 제작을 위해 개발된 [B](https://ko.wikipedia.org/wiki/B_(프로그래밍_언어)) 언어의 후속작이다. 현재 C 언어는 가장 널리 사용되고 있는 프로그래밍 언어로 [C++](ko.Cpp.md), [C#](ko.Csharp.md), [파이썬](ko.Python.md), 자바 등 여러 프로그래밍 언어에 영향을 주었다. C 언어는 다른 프로그래밍 언어에 비해 매우 빠른 처리 속도와 훌륭한 호환성을 가지고 있어 소프트웨어 및 펌웨어 개발에 여전히 사용되고 있다.

## 컴파일 언어
C 프로그래밍 언어는 [컴파일 언어](ko.Compiler.md)(compiled language)이다. C 컴파일러는 [국제 표준화 기구](https://www.iso.org/home.html)(International Organization for Standardization; ISO)에서 표준을 발표한 년도에 따라 버전이 나뉘어진다. 가장 널리 사용되고 있는 버전으로는 ANSI C(일명 C89)와 C99가 있다. 본 문서는 <span style="color: red;">*ANSI C 컴파일러 기준*</span>으로 C 프로그래밍 언어를 설명한다.

### 전처리기 지시문
[전처리기 지시문](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/preprocessor-directives)(preprocessor directive)는 소스 코드가 컴파일이 되기 전에 처리되는 명령을 가리킨다. 전처리기 지시문은 C 프로그래밍 언어 문법을 준수하지 않으며, 실질적인 코드에 전혀 관여하지 않는다. 비록 필수요소가 아니지만 다음과 같은 프로그래밍 편의성 및 컴파일러 설정을 제공한다.

<table style="width: 75%; margin: auto;">
<caption style="caption-side: top;">C 언어의 전처리기 지시문 예시</caption>
<colgroup><col style="width: 20%;"/><col style="width: 30%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">전처리기 지시문</th><th style="text-align: center;">예시</th><th style="text-align: center;">설명</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><code>#include</code></td><td><code>#include &lt;stdio.h&gt;</code></td><td>스크립트에 헤더 파일을 추가한다.</td></tr>
<tr><td style="text-align: center;"><code>#define</code></td><td><code>#define SQUARE</code></td><td>스크립트 내에서 사용할 수 있는 매크로를 정의한다.</td></tr>
<tr><td style="text-align: center;"><code>#pragma</code></td><td><code>#pragma once</code></td><td>컴파일러에 추가적 설정을 제공한다.</td></tr>
</tbody>
</table>

자세한 내용은 본문의 [전처리기](#전처리기) 장에서 다룰 예정이다.
