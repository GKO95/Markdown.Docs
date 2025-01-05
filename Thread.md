# 스레드
**[스레드](https://en.wikipedia.org/wiki/Thread_(computing))**(thread)는 [프로세스](Process.md)에 로드된 프로그램 이미지의 코드를 실행하기 위해 [프로세서](Processor.md)에서 처리하는 작업 흐름의 단위이다. 프로세스는 두 개 이상의 스레드를 활용할 수 있으며, 동일한 [가상 주소 공간](Process.md#가상-주소-공간)에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 단, 스레드 추가 여부는 프로그램 개발 당시 설계에 따라 결정되고 배포된 이후로 임의 추가할 수 있는 게 아니다.

### 기본 스레드
프로세스가 생성되면 코드 실행을 위해 기본적으로 한 개의 스레드가 함께 생성되는데, 이를 **기본 스레드**(primary thread)라고 부른다. 기본 스레드는 프로그램을 본격적으로 시작하는 [진입 함수](C.md#진입점)를 실행하기 때문에, 정상적인 기본 스레드의 종료는 결과적으로 프로세스 종료를 초래한다.

## 스레드 컨텍스트
**스레드 컨텍스트**(thread context)

## 스레드 로컬 스토리지
**[스레드 로컬 스토리지](https://en.wikipedia.org/wiki/Thread-local_storage)**(thread local storage; TLS)

# 스레드 동기화

# 스레드의 생성 및 종료
본 장은 [C](C.md) 언어로 개발된 [윈도우](Windows.md) 어플리케이션을 위주로 설명한다. 즉, [Win32 API](WinAPI.md) 이외에 [C 런타임 라이브러리](C.md#c-런타임-라이브러리)(CRT)가 함께 언급되어 개념을 설명한다.

## 스레드 생성
본래 [C 표준 라이브러리](https://en.wikipedia.org/wiki/C_standard_library)는 단일 스레드 프로그램을 위해 설계되어 다중 스레딩을 지원하는 매커니즘이 결여된다. [C 런타임 라이브러리](C.md#c-런타임-라이브러리)(일명 CRT)는 이를 보완하기 위해 [스레드 컨텍스트](#스레드-컨텍스트)를 저장할 `_tiddata` [구조체](C.md#구조체)를 [TLS](#스레드-로컬-스토리지)에 초기화하여 제공한다. CRT의 [_beginthread](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex) (혹은 [_beginthreadex](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex)) 함수는  Win32의 [CreateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread)를 내포하는 동시에 `_tiddata` 구조체를 초기화하는 등 다중 스레딩에 필요한 추가 리소스를 확보한다.

CreateThread가 실행되면 먼저 스레드 [커널 개체](Kernel.md#커널-개체)의 (대부분) 맴버들을 초기화하고 [스레드 스택](#스레드-스택)을 위한 [메모리](Memory.md) 영역을 할당한다. 스레드 스택의 최상단 주소에는 매개변수 `lpParameter` 및 `lpStartAddress` 인자를 순서대로 푸쉬하며, 다음은 이들 인자들이 무엇인지 소개한다.

* `lpParameter`: 스레드로 전달할 변수의 포인터
* `lpStartAddress`: 스레드가 실행할 프로그램 정의 함수의 포인터

마지막으로 스레드 커널 개체의 맴버 중 컨텍스트 관련 맴버들을 초기화한다: (1) [SP](Assembly.md#포인터-레지스터)에는 스택상 `lpStartAddress` 인자가 위치한 포인터, 그리고 (2) [IP](Assembly.md#명령어-포인터-레지스터)에는 스레드를 시작할 [RtlUserThreadStart](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/xperf/thread-start-functions) 함수의 포인터로 할당된다. 이대로 [스케줄링](Processor.md#스케줄링)되면 [프로세서](Processor.md#프로세서)는 스레드에 할애된 프로그램 정의 함수를 실행하게 된다.

## 스레드 종료
스레드를 종료하려면 대응하는 CRT의 [_endthread](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/endthread-endthreadex) (혹은 [_endthreadex](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/endthread-endthreadex)) 함수를 호출하거나, 또는 프로그램 정의 함수가 반환하여 종료될 시 자동적으로 호출된다. CRT 리소스 정리를 마친 다음, 내포된 Win32의 [ExitThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread) 함수 실행을 마지막으로 스택 메모리 해제 및 스레드 상태 코드를 반환한다. 이때 스레드 커널 개체의 참조 카운트가 차감되어, 만일 카운트가 0에 달하면 개체는 OS에 의해 자동 소멸된다.

### 잘못된 스레드 종료 방법
스레드를 종료하는 가장 올바른 방법은 프로그램 정의 함수가 [반환문](C.md#return-반환문) 혹은 CRT의 _endthread (혹은 _endthreadex) 함수를 호출하여 종료되는 것이다. 그 외에 스레드를 종료하는 방법이 몇 가지 존재하며 이들의 문제점을 함께 설명한다.

1. *스레드에서 Win32의 [ExitThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread) 함수 호출* : 프로그램을 C/C++ 코드로 작성하고 있다면 리소스 정리를 위해 가급적 CRT 함수를 호출한다.

1. *본 프로세스 (혹은 타 프로세스)에서 Win32의 [TerminateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminatethread) 함수 호출* : 해당 스레드의 프로세스가 종료되지 않는 이상, 시스템은 스레드 스택을 제거하지 않는다. 이는 마이크로소프트의 의도된 설계로, 스레드 스택을 제거하면서 유발될 [메모리 접근 오류](https://en.wikipedia.org/wiki/Segmentation_fault)를 방지하는 차원의 조치이다.
