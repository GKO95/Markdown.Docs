---
category: 윈도우
title: 윈도우 API
---
# 윈도우 API
[윈도우 API](https://ko.wikipedia.org/wiki/윈도우_API)(일명 WinAPI)는 [사용자 모드](ko.Processor.md#보호-링)에서 윈도우에서 제공하는 기능들을 활용할 수 있도록 하는 [어플리케이션 프로그래밍 인터페이스](https://ko.wikipedia.org/wiki/API)이다. 이들은 마이크로소프트 공식 문서가 존재하여 개발자가 참고할 수 있으며 대표적으로 [`CreateProcess`](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessw), [`CreateFile`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilew) 등의 함수가 해당한다. 윈도우 개발 당시에 가장 널리 사용된 저급 [C](ko.C.md) 프로그래밍 언어로 작성되었으므로 윈도우 API는 타 프로그래밍 언어에서도 불러와 활용이 가능하다.

> 이전에는 32비트 윈도우 운영체제의 프로그래밍 인터페이스인 Win32 API로 알려졌으나, 마이크로소프트는 특정 아키텍처에 종속되지 않는 명칭으로 변경한 것이다. 

윈도우에서 다루는 [함수](https://ko.wikipedia.org/wiki/함수_(컴퓨터_과학))(혹은 서브루틴; subroutine)는 API 함수 외에도 다음 유형들이 존재한다:

* **루틴(routines)**

    [커널 모드](ko.Processor.md#보호-링)에서만 호출될 수 있는 윈도우 운영체제 함수이다. 마이크로소프트 공식 문서를 참고하여 루틴에 대한 설명을 확인할 수 있으나, 이들은 [장치 드라이버](https://ko.wikipedia.org/wiki/장치_드라이버) 개발자 위주로 내용을 다룬다. 대표적인 예시로 윈도우 시스템 공간에 힙 영역의 [풀 메모리](ko.Memory.md#메모리-풀)를 할당하는 [`ExAllocatePoolWithTag`](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-exallocatepoolwithtag) 루틴이 있다.

* **[시스템 서비스](https://ko.wikipedia.org/wiki/시스템_호출)(system services)**

    운영체제에서 제공하는 기능을 사용자 모드에서 호출할 수 있도록 하는 함수이다. 시스템 서비스 실행 시 [프로세서](ko.Processor.md)의 특수한 명령에 의해 [스레드](ko.Process.md#스레드)는 커널 모드에 진입하여 루틴을 처리하고, 완료되면 사용자 모드로 되돌아가는 원리로 동작한다. 윈도우 API로 `CreateProcess` 함수를 호출하면 `NtCreateUserProcess` 시스템 서비스로 이어져 [프로세스](ko.Process.md)를 생성하는 루틴들이 처리된다. 시스템 서비스에 대하여 공개된 공식 마이크로소프트 참고 문서가 존재하지 않는다.

## 컴포넌트 오브젝트 모델
> *참조: [The Component Object Model - Win32 apps &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model)*

[컴포넌트 오브젝트 모델](https://ko.wikipedia.org/wiki/컴포넌트_오브젝트_모델)(Component Object Model; COM)은 마이크로소프트가 1993년에 프로세스 간 통신에 호환성을 보장하기 위해 표준화한 [어플리케이션 "이진" 인터페이스](https://ko.wikipedia.org/wiki/응용_프로그램_이진_인터페이스)이다. 본 내용을 진행하기 전에 API와 ABI의 차이점을 간단히 소개할 필요가 있다.

<table style="width: 60%; margin: auto;">
<caption style="caption-side: top;">API와 ABI의 차이점</caption>
<colgroup><col style="width: 30%;"/><col style="width: 35%;"/><col style="width: 35%;"/></colgroup>
<thead><tr><th style="text-align: center;">비교</th><th style="text-align: center;">API</th><th style="text-align: center;">ABI</th></tr></thead>
<tbody><tr><td>라이브러리 연관성</td><td style="text-align: center;">함수는 무엇이 존재하며 인자의 개수와 순서가 어떤지 선언</td><td style="text-align: center;">함수는 어떻게 접근되며 인자는 어떻게 전달이 되는지 정의</td></tr>
<tr><td>인터페이스 영역</td><td style="text-align: center;">소스 코드</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/기계어">머신 코드</a></td></tr>
<tr><td>하드웨어 독립</td><td style="text-align: center;">⭕</td><td style="text-align: center;">❌</td></tr>
</tbody>
</table>

라이브러리에 정의된 데이터와 함수를 소스 코드로 불러올 때는 API가 활용되나, 이를 컴파일 된 이진 파일에서 접근 및 호출할 때에는 ABI가 활용된다. 이는 개발에 사용된 프로그래밍 언어와 컴파일러 상관없이, 서로 달리 제작된 어플리케이션이나 라이브러리 사이에서도 상호작용을 가능케 만든다. 단, COM 제작 및 사용을 할 수 있는 프로그래밍 언어로써는 [가상 메소드 테이블](https://ko.wikipedia.org/wiki/가상_메소드_테이블)을 활용할 수 있는 [포인터](ko.C.md#포인터)가 지원되어야 한다.

[C#](ko.Csharp.md) 프로그래밍 언어의 [인터페이스](ko.Csharp.md#인터페이스) 및 [클래스](en.Csharp.md#클래스)에 대한 지식은 개념의 유사성으로 COM을 이해하는데 도움이 된다: COM 클래스는 도입된 COM 인터페이스의 텅 빈 가상 메소드에 실질적인 함수 코드(일명 컴포넌트, 혹은 COM 오브젝트)를 정의하는데, 해당 컴포넌트를 사용하기 위해서는 오로지 COM 인터페이스로만 접근될 수 있다. 그리고 COM 오브젝트 호출하는 자를 COM 클라이언트, 그리고 제공하는 자를 COM 서버라고 지칭한다.

# 같이 보기
* [Windows API index](https://learn.microsoft.com/en-us/windows/win32/apiindex/windows-api-list)
* [COM Technical Overview](https://learn.microsoft.com/en-us/windows/win32/com/com-technical-overview)
