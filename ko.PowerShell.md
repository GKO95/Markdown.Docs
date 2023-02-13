---
layout: docs
category: Script
title: 파워셸
slug: ko.PowerShell
icon: icon-powershell.png
order: null
---
# 파워셸
[파워셸](https://learn.microsoft.com/en-us/powershell/scripting/overview)(PowerShell)은 [마이크로소프트](https://www.microsoft.com)에서 개발한 [크로스 플랫폼](https://ko.wikipedia.org/wiki/크로스_플랫폼)을 지원하는 [.NET](https://ko.wikipedia.org/wiki/닷넷) 기반의 [명령 줄](https://ko.wikipedia.org/wiki/명령_줄_인터페이스) [셸](https://ko.wikipedia.org/wiki/셸)이자 [스크립트 언어](https://ko.wikipedia.org/wiki/스크립트_언어)이다. 작업 자동화 및 구성 관리에 특화되었으며, [윈도우](https://learn.microsoft.com/en-us/powershell/windows/get-started)나 [애저](https://learn.microsoft.com/en-us/powershell/azure/)와 같은 마이크로소프트 제품 이외에도 [AWS](https://aws.amazon.com/powershell/), [VMware](https://core.vmware.com/vmware-powercli), [구글 클라우드](https://cloud.google.com/powershell/) 등의 [서드 파티](https://ko.wikipedia.org/wiki/서드_파티_개발자#서드파티) 제품에도 활용이 가능하다. [윈도우 8](ko.WindowsNT) 이상의 운영체제에서는 파워셸이 기본적으로 설치되어 있어 곧바로 실행될 수 있다.

> 기본적으로 설치된 "윈도우 파워셸(Windows PowerShell)"은 윈도우 운영체제에서만 지원되는 [.NET 프레임워크](ko.Csharp#net-프레임워크)로 개발된 파워셸 `5.x` 빌드를 가리킨다.

![<a href="https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701">윈도우 터미널</a>의 파워셸 7.3.1에서 <a href="#cmdlet">cmdlet</a>을 실행하는 예제](./images/powershell_example.png)

### 명령 프롬프트
[명령 프롬프트](https://ko.wikipedia.org/wiki/Cmd.exe), 일명 cmd.exe은 윈도우 운영체제에 오래 전부터 존재하던 또 다른 셸 환경으로 `.BAT` 혹은 `.CMD` 확장자의 [배치 파일](https://ko.wikipedia.org/wiki/배치_파일)(batch file) 스크립트를 실행하여 작업을 수행할 수 있다. 비록 파워셸이란 보다 강력한 스크립트 도구가 있어도 윈도우를 구성하는 수많은 자동화 시스템이 오랜 시간동안 누적된 cmd.exe 스크립트의 결과물이기 때문에 절대로 대체될 수 없는 존재이다.

자세한 내용은 마이크로소프트 [Windows Command Line](https://devblogs.microsoft.com/commandline/rumors-of-cmds-death-have-been-greatly-exaggerated/) 게시글을 참고한다.

## 객체 지향
파워셸은 [객체 지향](https://ko.wikipedia.org/wiki/객체_지향_프로그래밍)(object-oriented) 성격이 짙은 스크립트 언어이며, 처리되는 입출력 데이터가 전부 [객체](ko.Csharp#클래스)(object)이다. 그 예시로 파워셸 버전을 확인할 수 있는 [`$PSVersionTable`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_powershell_editions#edition-in-psversiontable) 자동 변수를 불러오면 `PSVersion`, `PSEdition` 등의 다양한 정보가 출력되는 데, 이들은 [C#](ko.Csharp) 프로그래밍 언어에서 객체의 맴버를 접근하듯이 `.` 기호를 통해 정보를 선택적으로 호출할 수 있다.

> [자동 변수](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables)(automatic variables)란, 파워셸에 대한 상태 정보가 저장된 읽기 전용 변수이며 파워셸에서 자체적으로 생성하고 관리한다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption>파워셸의 객체 지향 데이터와 이에 속한 맴버</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead style="text-align: left;">
<tr><th>

```powershell
$PSVersionTable
```
</th>
<th>

```powershell
$PSVersionTable.PSVersion
```
</th></tr>
</thead>
<tbody>
<tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```terminal
Name                           Value
----                           -----
PSVersion                      5.1.22621.963
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.22621.963
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```
</td>
<td>

```terminal
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      22621  963
```
</td></tr>
</tbody>
</table>

반환된 객체의 유형 및 메소드나 이벤트 등의 맴버를 확인하려면 [`Get-Memeber`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-member)를 통해 확인을 해야 하며, 이에 대한 내용은 [파이프라인](#파이프라인)을 참고하도록 한다.

## Cmdlet
파워셸은 [cmdlet](https://learn.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview)(발음: command-let)이란 수많은 명령어들을 제공한다. Cmdlet은 런타임에 빌드된 .NET 클래스의 객체인데, 이러한 특성으로부터 일관된 [파이프라인](#파이프라인) 동작 등이 보장된다.

> 반면, 다른 셸 환경의 명령어(예. [`tasklist`](https://ko.wikipedia.org/wiki/Tasklist))는 그에 해당하는 프로그램(예. `%Windir%\System32\tasklist.exe`)이 실행되는 방식으로 동작한다.

Cmdlet은 다음과 같이 동사(verb)와 명사(noun)의 조합으로 구성되며 [`Get-Commnad`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-command)으로 목록을 찾아볼 수 있다. 한편, 파워셸에서 사용 가능한 동사들은 [`Get-Verb`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-verb)으로 확인할 수 있다. 파워셸은 대문자와 소문자를 구분하지 않는다.

```powershell
Get-Command -Verb Add -Noun Appx*
```
```terminal
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-AppxPackage                                    2.0.1.0    Appx
Cmdlet          Add-AppxProvisionedPackage                         3.0        Dism
Cmdlet          Add-AppxVolume                                     2.0.1.0    Appx
```

파워셸 cmdlet에 대한 도움말은 [`Get-Help`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help)로 해당 명령어에 대한 설명, 매개변수 및 사용 예시 등을 확인할 수 있다. 그 외에 alias 및 매개변수로 도움말을 찾아보는 방법도 존재한다. 허나, 최신 버전의 파워셸은 도움말 파일이 기본적으로 없는 관계로 별도 설치가 필요할 수 있다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption>Cmdlet 도움말 호출 방식</caption>
<thead><tr><th style="text-align: center;"><code>Get-Help</code> Cmdlet</th><th style="text-align: center;"><code>Help</code> Alias</th><th style="text-align: center;"><code>-?</code> 매개변수</th></tr></thead>
<tbody>
<tr style="vertical-align: top; overflow-wrap: break-word;"><td>

```powershell
Get-Help Get-Process
```
</td>
<td>

```powershell
Help Get-Process
```
</td>
<td>

```powershell
Get-Process -?
```
</td></tr>
</tbody>
</table>

Cmdlet은 사용자가 파워셸 또는 .NET을 활용하여 직접 제작하거나 온라인 커뮤니티에서 공유되고 있는 것을 가져와서 사용할 수도 있다.

### 파이프라인
[파이프라인](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines)(pipeline) 파워셸의 여러 cmdlet가 [파이프](https://ko.wikipedia.org/wiki/수직선_(기호)) 기호 `|`로 연계된 조합을 가리킨다. 파워셸 명령어는 맨 좌측에서 시작하여 반환된 객체를 파이프 건너에 있는 다음 명령어의 입력으로 전달한다. Cmdlet 이외에도 함수, 스크립트 파일, 혹은 실행 프로그램이 연계될 수 있다.

> 비록 파이프라인 기능은 [UNIX](https://ko.wikipedia.org/wiki/유닉스)에서 비롯되었으나 윈도우 등의 다른 운영체제에서도 이를 채용하였으며, 파워셸에서는 .NET 런타임과 객체 지향을 통합하여 작업 효율성을 더욱 높였다.

아래 파이프라인 예시는 `Get-Process` cmdlet가 반환하는 현재 시스템에서 실행 중인 [프로세스](ko.Process)들을 반영하는 객체의 유형 및 맴버들을 `Get-Member` cmdlet으로 나열한다.

```powershell
Get-Process | Get-Member
```
```terminal
   TypeName: System.Diagnostics.Process

Name                       MemberType     Definition
----                       ----------     ----------
Handles                    AliasProperty  Handles = Handlecount
Name                       AliasProperty  Name = ProcessName
NPM                        AliasProperty  NPM = NonpagedSystemMemorySize64
PM                         AliasProperty  PM = PagedMemorySize64
SI                         AliasProperty  SI = SessionId
VM                         AliasProperty  VM = VirtualMemorySize64
WS                         AliasProperty  WS = WorkingSet64
…
```

파워셸에서 파이프라인을 활용할 때 "좌(左)필터 우(右)서식" 규칙을 준수하기를 당부한다:

* **필터링(filtering)**

    파워셸에서 처리되어야 할 자료 중에서 가능한 필요한 데이터만 불러오도록 신경을 쓰는 것을 가리킨다. 이는 특히 방대한 데이터 용량을 자랑하거나, 혹은 네트워크를 통해 데이터를 불러오는 경우에 파이프라인 처리 속도와 효율을 증대시키는 효과를 가져올 수 있다. 그러므로, "좌필터"는 필터링 작업은 최대한 좌측 cmdlet에서 이루어져야 한다는 의미이다.

* **서식화(formatting)**

    파워셸로부터 원하던 자료들을 말끔하게 정리하여 보여주기 위해 데이터 서식화를 사용한다. 그러나 서식화 과정에서 본래 데이터들이 속하던 객체 속성들이 훼손되면서 더 이상의 데이터 추출이 어려워지게 되므로, "우서식"은 서식화 작업을 최대한 파이프라인의 마지막에 이루어져야 한다는 것을 의미한다.

# 스크립트
파워셸 스크립트는 `.PS1` 확장자 파일에 작성된다. [마이크로소프트](https://www.microsoft.com)는 스크립트를 작성하는 도구로 [파워셸 확장도구](https://code.visualstudio.com/docs/languages/powershell)가 설치된 [비주얼 스튜디오 코드](https://ko.wikipedia.org/wiki/비주얼_스튜디오_코드)<sub>([다운로드](https://code.visualstudio.com/download))</sub>, 일명 VS Code를 권장한다.

> 그 외에 [파워셸 통합 스크립팅 환경](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise)(Integrated Scripting Environment; ISE)도 있지만 [.NET 프레임워크](ko.Csharp#net-프레임워크)로 빌드된 [윈도우 파워셸](#파워셸)(Windows PowerShell)만 지원한다.

본 장은 파워셸로 스크립트를 작성하기 위해 알아야 할 구문들과 개념들을 위주로 설명한다. 만일 작성한 `sample.ps1` 스크립트를 파워셸에서 실행하려면 아래와 같이 [온점 연산자](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators#dot-sourcing-operator-)(dot sourcing operator) `.`를 함께 입력하는 걸 권장하며, 자세한 내용은 [범위](#범위)(scope)에서 설명할 예정이다.

```powershell
. ./sample.ps1
```

## 구문
다음은 파워셸 스크립트 언어를 작성하는 데 준수되어야 할 기본적인 문법적 규칙, 일명 [구문](https://ko.wikipedia.org/wiki/구문_(프로그래밍_언어))(syntax)에 관여하는 요소들을 소개한다:

* **[토큰](https://learn.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-02#23-tokens)(token)**

    줄바꿈, [주석](#주석), 빈칸, 혹은 이들의 조합에 의해 구분되는 파워셸에서 가장 작은 단위의 어휘 요소이다. 대표적으로 [키워드](https://learn.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-02#231-keywords), [변수](#변수), [리터럴](https://ko.wikipedia.org/wiki/리터럴) 등이 있다.

    ```powershell
  $variable       # 변수
  2               # 정수 리터럴
    ```

* **[표현식](https://learn.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-07)(expression)**
    
    값을 반환하는 구문적 존재를 가리킨다. 표현식에 대한 결과를 도출하는 것을 평가(evaluate)라고 부른다.
    
    ```powershell
  2 + 3           # 숫자 5를 반환
  2 -lt 3         # 논리 참을 반환
    ```

* **[문장](https://learn.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-08)(statement)**
    
    실질적으로 무언가를 실행하는 구문적 존재를 가리킨다: 흔히 하나 이상의 표현식으로 구성되지만, [`break`](#break-문) 및 [`continue`](#continue-문)와 같이 독립적으로 사용되는 문장도 있다. 파워셸 스크립트 언어는 기본적으로 [줄바꿈](https://ko.wikipedia.org/wiki/새줄_문자)(newline)을 기준으로 문장을 분별한다. 아래 기호는 파워셸의 스크립팅 문장을 유연하게 활용할 수 있도록 도와준다.

    * 세미콜론 `;`: 여려 문장을 하나의 줄에 한꺼번에 기입하기 위해 사용된다.

    ```powershell
  $variable = 2 + 3          # 숫자 5를 "variable" 변수에 초기화
  if (2 -lt 3) { statement } # 논리가 참이면 "statement" 문장 실행
    ```

### 주석
[주석](https://peps.python.org/pep-0008/#comments)(comment)은 스크립트의 코드로 취급하지 않아 실행되지 않는 영역이다. 흔히 코드에 대한 간단한 정보를 기입하기 위해 사용되는 데, 파워셸에는 이를 해시 기호 `#`로 표기한다.

```powershell
# Comment section here.
Write-Output "Hello World!"
```

## 콘솔 출력
본 부문에서는 파워셸 콘솔에서 출력을 처리하는 대표적인 두 cmdlet을 소개한다:

* **[`Write-Output`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output)** (별칭: `echo`)

    출력되는 데이터를 콘솔로 표시하지 않는 대신, 다음 [파이프라인](#파이프라인) 명령의 입력 객체로 전달한다. 단, `Write-Output` 이후로 연계될 명령이 없을 경우, 즉 파이프라인의 마지막 명령이거나 단일 cmdlet 구성이면 콘솔로 출력된다. 데이터를 출력할 때 가장 흔히 사용되는 cmdlet이다.

    아래 예시는 `"Hello World!"` 텍스트 출력이 [`Get-Memeber`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-member)의 입력으로 전달되어 텍스트의 객체 유형 및 속성이 나열된 것이다.

    ```powershell
  Write-Output "Hello World!" | Get-Member
    ```
    ```terminal
     TypeName: System.String
  
  Name                 MemberType
  ----                 ----------
  Clone                Method
  CompareTo            Method
  Contains             Method
  CopyTo               Method
  EndsWith             Method
  EnumerateRunes       Method
  Equals               Method
  …
    ```

    파워셸에서 기본적으로 텍스트 및 객체 출력을 콘솔로 표시하는 cmdlet들은 `Write-Output`을 명시할 필요가 없다. 아래는 일부 cmdlet 및 텍스트만을 입력하는 것이 파워셸에서는 구문적으로 어떻게 간주되는지 보여준다.

    <table style="width: 50%; margin: auto;"><caption><code>Write-Output</code> cmdlet의 암묵적 활용</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead style="text-align: center;"><tr><th style="text-align: center;">Cmdlet</th><th style="text-align: center;">동일 구문</th></tr></thead><tbody><tr><td><code>Get-Process</code></td><td><code>Get-Process | Write-Output</code></td></tr><tr><td><code>"Hello World!"</code></td><td><code>echo "Hello World!"</code></td></tr></tbody></table>

* **[`Write-Host`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-host)**

    여기에서 호스트(host)는 해당 cmdlet이 실행되고 있는 파워셸 콘솔 터미널을 가리킨다. 즉, `Write-Host` cmdlet은 무조건 데이터를 콘솔로 출력한다. 객체 출력보다는 텍스트 표시에 더욱 최적화된 cmdlet이며, `Write-Output`과 전혀 다른 결과물을 보여주는 점에 유의해야 한다.

    아래 예시는 `"Hello World!"` 텍스트가 콘솔로 그대로 출력되면서 [`Get-Memeber`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-member)로 전달된 입력으로 없으면서 나타나는 현상이다.

    ```powershell
  Write-Host "Hello World!" | Get-Member
    ```
    ```terminal
  Hello World!
  Get-Member: You must specify an object for the Get-Member cmdlet.
    ```

## 콘솔 입력
본 부문에서는 파워셸 콘솔에서 입력을 처리하는 대표적인 cmdlet을 소개한다:

* **[`Read-Host`](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/read-host)**

    콘솔에 입력된 한 줄의 텍스트를 읽는다. `-Prompt` 매개변수를 통해 사용자로부터 콘솔 입력을 대기할 때 문구를 표시한다.

    ```powershell
  Read-Host -Prompt "Write anything here" | Write-Output
    ```
    ```terminal
  Write anything here: Hello World!
  Hello World!
    ```

> [탈출 문자](https://ko.wikipedia.org/wiki/이스케이프_문자)(escape character)는 [억음 부호](https://ko.wikipedia.org/wiki/억음_부호) <code>&grave;</code>를 사용하여 문자열로부터 탈출해 텍스트 내에서 특정 연산을 수행한다: <code>&grave;n</code> 탈출 문자를 사용하여 문자열 줄바꿈을 구현한다.
