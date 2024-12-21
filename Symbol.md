# 디버그 심볼
**[디버그 심볼](https://en.wikipedia.org/wiki/Debug_symbol)**(debug symbol)은 이진 파일(대표적으로 실행 파일, 라이브러리 등)의 기계어를 소스 코드로 변환하여 나타낼 수 있는 정보이며, 이를 통해 컴파일된 원본 스크립트가 필요없이 내부를 들여다 볼 수 있다. 비록 심볼은 원본과 같은 완전한 소스 코드를 재구성하지 못하지만, 변수와 함수의 식별자 및 메모리상 위치하는 주소를 알아낼 수 있다.

## 윈도우 심볼
[윈도우 OS](Windows.md)를 관측할 수 있는 정보의 유형과 제약에 따라 심볼을 세 가지로 나눈다.

<table style="table-layout: fixed; width: 60%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">윈도우 운영체제 <a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/public-and-private-symbols">심볼 종류</a></caption><colgroup><col style="width: 30%;"/></col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">심볼</th><th style="text-align: center;">관측 가능한 데이터</th></tr></thead><tbody><tr><td style="text-align: center;">Export</td><td><ul><li>함수 (export 테이블 한정)</li></ul><sub>서버: N/A</sub></td></tr><tr><td style="text-align: center;">Public</td><td><ul><li>함수 (비정적 한정)</li><li>전역 변수 (<code>extern</code> 한정)</li></ul><sub>서버: <code>https://msdl.microsoft.com/download/symbols</code></sub></td></tr><tr><td style="text-align: center;">Private</td><td><ul><li>함수</li><li>전역 변수</li><li>지역 변수</li><li>사용자 정의 구조체, 클래스, 그리고 자료형</li><li>소스 파일 정보: 파일명, 줄번호, 기타 등등</li></ul></td></tr></tbody></table>

### DbgHelp 라이브러리
**[디버그 지원 라이브러리](https://learn.microsoft.com/en-us/windows/win32/debug/debug-help-library)**(debug help library), 일명 DbgHelp 라이브러리는 윈도우 NT 운영체제에서 구동되는 .exe 확장자의 실행 파일을 디버깅하는 데 도움을 주는 [함수](WinAPI.md)들을 제공한다. [Sysinternals](Sysinternals.md)를 포함한 일부 프로그램에서 [호출 스택](WinDbg.md#호출-스택) 정보를 보기위해 필요할 수 있으며 [윈도우 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)와 함께 설치된다.

만일 64비트의 NT 버전 10 운영체제(즉, 윈도우 10 및 11)를 위한 DbgHelp 라이브러리는 다음 경로에 위치한다.

```terminal
%ProgramFiles(x86)%\Windows Kits\10\Debuggers\x64\dbghelp.dll
```

윈도우에 기본적으로 설치된 `%windir%\System32\dbghelp.dll` 파일이 존재하지만, 이는 호환성을 위할 뿐이며 SDK로부터 설치한 버전과 달리 디버깅에 사용할 수 없다.

### 프로그램 데이터베이스
**[프로그램 데이터베이스](https://en.wikipedia.org/wiki/Program_database)**, 일명 .pdb 확장자 파일은 해당 모듈의 디버깅 정보가 들어있는 마이크로소프트 형식이며, 그 안에는 심볼도 포함된다.<sup>[<a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/symbols-portable-pdb">참고</a>]</sup> 비주얼 스튜디오(Visual Studio)로 소스 코드를 빌드하면 이진 파일 외에 함께 생성되는 파일 중 하나이다. 만일 트러블슈팅 과정에서 자체 개발 어플리케이션 등을 함께 분석할 필요가 있으면 해당하는 .pdb 파일이 필요할 수 있다.

## 심볼 경로
> *출처: [Symbol path for Windows debuggers - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/symbol-path)*

사용자 및 커널 모드 어플리케이션을 디버깅하는데 심볼 파일이 위치한 경로를 지정하는 작업은 매우 중요하다. 디버거가 모듈 내부를 살펴보기 위해 필요한 심볼을 불러올 수 있도록 한다.

심볼을 매번 불러오는 오버헤드를 최소화하기 위해 로컬에 캐싱하는 걸 권장한다.

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">심볼 캐싱 설정 구문</caption><colgroup><col style="width: 40%"/><col style="width: 60%"/></colgroup><thead><tr><th style="text-align: center;">구문</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td><code>cache*;</code></td><td>세미콜론 우측에 기입된 주소로부터 불러온 심볼을 기본 로컬 캐시 경로에 저장한다.</td></tr><tr><td><code>cache*C:\Symbols;</code></td><td>세미콜론 우측에 기입된 주소로부터 불러온 심볼을 <code>C:\Symbols</code> 경로에 저장한다.</td></tr></tbody></table>

인터넷이나 사내망으로부터 심볼을 불러올 서버 및 캐시 디렉토리를 지정할 수 있다.

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">심볼 서버 설정 구문</caption><colgroup><col style="width: 40%"/><col style="width: 60%"/></colgroup><thead><tr><th style="text-align: center;">구문</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td><code>srv*</code></td><td>기본 심볼 서버로부터 필요한 심볼을 획득한다.</td></tr><tr><td><code>srv*https://example.com</code></td><td><code>https://example.com</code> 서버로부터 필요한 심볼을 획득한다.</td></tr><tr><td><code>srv*C:\Symbols*https://example.com</code></td><td><code>https://example.com</code> 서버로부터 필요한 심볼을 획득하여 <code>C:\Symbols</code> 경로에 캐싱한다.</td></tr></tbody></table>

시스템 환경 변수 `_NT_SYMBOL_PATH`는 하나의 심볼 경로를 다른 디버거 프로그램에 동일하게 적용하는데 매우 유용하게 사용된다. 아래는 환경 변수에 [윈도우 심볼](#윈도우-심볼)을 GUI 혹은 CLI로 지정하는 예시를 보여준다.

![환경 변수 <code>_NT_SYMBOL_PATH</code>의 예시](./images/windbg_environment_symbol.png)

```
SETX _NT_SYMBOL_PATH SRV*C:\Symbols*https://msdl.microsoft.com/download/symbols
```

## 심볼 검증
> *출처: [SymChk - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/symchk)*

**[SymChk](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/symchk)** 프로그램은 이진 파일과 심볼 파일을 비교하여 올바른지 여부를 확인한다. [윈도우 SDK](https://aka.ms/windowssdk)의 *Debugging Tools for Windows*에 포함되며, 해당 프로그램의 64비트 버전은 다음 디렉토리에 위치한다.

* `C:\Program Files (x86)\Windows Kits\10\Debuggers\x64`

아래는 [ntoskrnl.exe](Kernel.md#nt-커널) 실행 파일의 심볼을 다운로드 및 검사한다.

```cmd
symchk "C:\Windows\System32\ntoskrnl.exe" /s SRV*C:\Symbols*https://msdl.microsoft.com/download/symbols
```
