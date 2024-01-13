# 프로세스
[프로세스](https://ko.wikipedia.org/wiki/프로세스)(process)는 [.exe](https://ko.wikipedia.org/wiki/EXE) 확장자 파일 유형과 같은 [어플리케이션](https://ko.wikipedia.org/wiki/응용_소프트웨어) 등의 [프로그램](https://ko.wikipedia.org/wiki/컴퓨터_프로그램)을 실행하기 위해 필요한 데이터와 리소스를 담는 컨테이너이다. 프로세스와 프로그램은 엄연히 다른 존재이며, 후자는 [C](C.md)/[C++](Cpp.md) 등의 프로그래밍 언어로 작성된 코드를 [기계어](https://ko.wikipedia.org/wiki/기계어)의 집합체로 컴파일하여 생성된 부산물이다. 즉 프로그램은 동작 방식이 단순 코드로 작성된 [이미지](https://ko.wikipedia.org/wiki/실행_파일)에 불과하며, 이를 실행에 옮기는 주체가 바로 프로세스이다.  객체(instance)라고 부르기도 한다.

> 객체지향 프로그래밍 개념에 빗대어 프로세스를 프로그램의 객체(instance)라고 상당수의 문서에서 자주 언급되는 편이다.

동일한 프로그램에서 실행되어도, [가상 주소 공간](#가상-주소-공간)에 의해 각 프로세스는 서로에게 영향을 미치지 않으며 자신만의 작업을 수행할 수 있다.

## 가상 주소 공간
[가상 주소 공간](https://ko.wikipedia.org/wiki/가상_주소_공간)(virtual address space; VAS)은 프로세스마다 데이터를 저장할 수 있는 가상의 [메모리](Memory.md) 공간이다. [Sysinternals](Sysinternals.md)의 [VMMap](VMMap.md) 유틸리티 프로그램은 프로세스의 가상 주소 공간을 살펴볼 수 있는 유용한 도구이다. 여기서 "가상"이 갖는 의미는 매우 중요하며, 아래의 세 가지 성질들을 충족시킨다.

1. **고립성**

    프로세스마다 주어지는 개별적인 메모리 공간이므로, 일반적으로 프로세스 간 접근이 불가하고 어떠한 영향을 미치거나 받지도 않는다. 가상 주소 공간을 구성하는 두 가지 공간에 대한 설명은 다음과 같다:

    * *사용자 공간(user space)*

        개별 프로세스의 데이터 및 리소스가 저장되는 "프로세스 공간"이다. 샤용자 공간은 모든 프로세스마다 각각 주어지며, 타 프로세스에서 단순 접근이 절대로 불가한 구조로 되어있다. 만일 한 프로세스에서 충돌이 발생하여도 시스템 및 나머지 프로세스들은 아무런 영향이 없으며 온전히 실행된다. 즉 가상 주소 공간의 고립성은 바로 사용자 공간의 특성에서 비롯된 것이다.

    * *커널 공간(kernel space)*

        프로세스가 실행되기 위해 반드시 필요한 운영체제 [커널](Kernel.md)의 데이터 및 리소스가 저장되는 "시스템 공간"이다. 커널 공간은 성능 및 효율성을 위해 여러 개로 나누지 않으므로, 모든 프로세스는 하나의 커널 공간을 공용하여 동일한 정보를 가진다. 그러나 프로세스는 ([장치 드라이버](Driver.md)를 활용하지 않는 이상) 하드웨어적 보안 조치로 인해 커널 공간에 함부로 접근이 불가하다.

    다시 말해, 비록 가상 주소 공간에 커널 공간이 포함되어 있어도 [시스템 서비스](WinAPI.md#시스템-서비스) 외에는 프로세스가 직접 접근할 수 없는 공간이기 때문에 사용자 공간의 고립성이 여전히 유효하다.

1. **독립성**

    가상의 메모리 공간이므로, 설치된 [물리 메모리](https://ko.wikipedia.org/wiki/주기억장치)(즉, [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리))의 크기에 의한 제약을 받지 않는다. 32비트와 64비트는 각각 4 GB (= 2<sup>32</sup>) 그리고 16 EB (= 2<sup>64</sup>) 크기만큼의 주소를 표현할 수 있다는 걸 감안하면, 아래는 프로세스가 할당받을 수 있는 이론상 메모리 공간이며 물리 메모리 크기와 아무런 상관이 없다.

    <table style="table-layout: fixed; width: 80%; margin: auto;">
    <caption style="caption-side: top;">가상 주소 공간의 최대 크기</caption>
    <colgroup><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/></colgroup>
    <thead><tr><th rowspan="2" style="text-align: center;">가상 주소 공간</th><th rowspan="2" style="text-align: center;">32비트 프로세스</th><th colspan="2" style="text-align: center; border-bottom-style: none;">64비트 프로세스</th></tr><tr><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/윈도우_8">윈도우 8</a> 및 이전</th><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/윈도우_8.1">윈도우 8.1</a> 및 이후</th></tr></thead><tbody style="text-align: center;"><tr><td>사용자 공간</td><td>2 GB</td><td>8 TB</td><td>128 TB</td></tr><tr><td>커널 공간</td><td>2 GB</td><td>8 TB</td><td>128 TB</td></tr></tbody>
    <caption style="caption-side: bottom; text-align: left;"><sup>출처: <a href="https://learn.microsoft.com/en-us/windows/win32/memory/memory-limits-for-windows-releases"><i>Memory Limits for Windows and Windows Server Releases -Win32 apps | Microsoft Learn</i></a></sup></caption>
    </table>

    프로그램의 `IMAGE_FILE_LARGE_ADDRESS_AWARE` 플래그 설정 여부에 따라 프로세스의 사용자 공간을 2 GB으로 제한할 지 (32비트 프로세스 기본값), 혹은 그 이상 크기를 확장할 지 (64비트 프로세스 기본값) 선택할 수 있다.

    ![비주얼 스튜디오에서 <code>IMAGE_FILE_LARGE_ADDRESS_AWARE</code> 플래그 설정여부 위치](./images/process_large_addresses.png)

    * 32비트 프로세스는 32비트 윈도우 운영체제에서 3 GB만 사용자 공간으로 사용하고, 나머지 1 GB는 커널 공간으로 확보한다.
    * 32비트 프로세스는 64비트 윈도우 운영체제에서 4 GB 전체를 사용자 공간으로 활용한다.

1. **연속성**

    매핑된 메모리 공간이므로, 물리 메모리상 띄엄띄엄 분산된 메모리 조각들을 하나의 연속된 가상 메모리 공간으로 구현한다. 가상 메모리의 [페이지](#페이지)(page)와 물리 메모리의 [페이지 프레임](#페이지)(page frame)를 일대일 매핑하는 것은 [메모리 관리 장치](https://ko.wikipedia.org/wiki/메모리_관리_장치)(MMU) 하드웨어에서 담당한다. 해당 성질은 메모리의 [파편화](https://en.wikipedia.org/wiki/Fragmentation_(computing))를 방지하는 효과를 기대할 수 있다.

    ![가상 주소 공간과 물리 메모리의 관계](https://upload.wikimedia.org/wikipedia/commons/3/32/Virtual_address_space_and_physical_address_space_relationship.svg)

## 핸들
[핸들](https://ko.wikipedia.org/wiki/핸들_(컴퓨팅))(handle)은 프로세스가 파일이나 오브젝트와 같은 리소스에 접근하거나 불러오기 위해 사용하는 추상적인 [참조](C.md#포인터)이다. 일반적으로 핸들을 아래와 같이 선언한다.

```c
typedef void* HANDLE;
```

비록 핸들은 `void` 자료형 포인터로 나타날지언정, 가상 메모리상 데이터가 위치한 주소를 직접적으로 가리키는 포인터와 엄밀히 다른 존재이다. 가상 주소 공간 안에는 리소스들의 포인터가 나열된 배열 혹은 테이블이 있는데, 여기서 원하는 리소스에 접근하기 위한 테이블 진입점을 색인하는 요소가 바로 핸들이다. 만일 어떠한 이유로 리소스 포인터가 바뀌었다 하여도, 테이블에서의 해당 리소스 진입점은 여전하기 때문에 핸들의 유효성이 그대로 유지된다는 점에서 "추상적"인 참조라고 언급하였다.

리소스의 핸들을 생성 및 제거하는 것을 각각 열고(open) 닫는다(close)고 표현한다. 더 이상 사용되지 않는 리소스의 열린 핸들을 전부 닫아주지 않으면 프로세스가 종료될 때까지 테이블 및 메모리에 리소스가 계속 잔여하는 [핸들 누수](https://ko.wikipedia.org/wiki/핸들_누수)(handle leak) 문제가 발생한다.

# 스레드
[스레드](https://ko.wikipedia.org/wiki/스레드_(컴퓨팅))(thread)는 프로세스의 프로그램 이미지 코드를 실행하기 위해 [CPU](Processor.md)에서 처리할 수 있는 작업 흐름의 단위이다. 프로세스는 기본적으로 하나의 스레드를 갖는데 개발자의 설계에 의해 추가로 생성하여 두 개 이상의 스레드를 활용할 수 있고, 동일한 가상 주소 공간에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 그러나 프로세스에는 최소한 하나의 스레드가 존재해야 하므로, 모든 스레드가 종료되면 해당 프로세스는 자동적으로 함께 종료된다.

# 시스템 프로세스
시스템 프로세스(system processes)는 운영체제 구동을 위해 반드시 존재하는 프로세스들을 가리킨다. 이들은 작업 관리자의 *세부 정보* 탭이나 [Sysinternals](Sysinternals.md)의 [프로세스 탐색기](Process_Explorer.md)를 통해 직접 확인이 가능하다.

* **[유휴 프로세스](https://ko.wikipedia.org/wiki/시스템_유휴_프로세스)(Idle process)**

    [프로세서](Processor.md#프로세서)가 아무런 작업을 하지 않고 있음을 나타내기 위한 실체가 없는 PID 0의 가짜 프로세스이다. 만일 유휴 프로세스의 프로세서 사용량이 90%로 집계된 경우, 이는 반대로 프로세서의 10%만이 실제로 실행 중인 프로세스를 처리하는 작업에 동원되고 있음을 의미한다. 이러한 이유로 유휴 프로세스의 [스레드](#스레드) 개수는 시스템의 [논리 프로세서](Processor.md#논리-프로세서) 개수와 일치한다.

* **시스템 프로세스(System process)**

    [Ntoskrnl.exe](Kernel.md#nt-커널) 또는 로드된 [드라이버](Driver.md) 등의 [커널](Kernel.md#커널) 스레드를 반영하기 위한 PID 4의 특수한 프로세스이다. 시스템 프로세스는 "프로세스 자체"를 가리키는 게 아닌, [커널 모드](Processor.md#권한-수준)에서 생성된 "시스템 스레드의 집합"을 지칭한다. 시스템 스레드는 일반 사용자 모드 스레드와 동일한 속성과 문맥을 지니고 있으나, [프로세스 공간](#가상-주소-공간)의 주소가 존재하지 않으며 오로지 [시스템 공간](#가상-주소-공간)의 코드만 실행한다.

    > 즉 어떤 프로세스라도 장치 드라이버를 통해 스레드를 생성하면 이 또한 시스템 스레드가 되기 때문에 트러블슈팅 과정에서 유의해야 한다.

    * *보안 시스템 프로세스(Secure System process)*

        [VTL1](Hypervisor.md#가상-보안-모드) 보안 커널 주소 공간, 핸들, 그리고 시스템 스레드가 상주하는 프로세스이다. [스케줄링](Processor.md#스케줄링), 객체 및 메모리 관리는 VTL0 커널에서 이루어지기 때문에 실질적인 운영체제 동작에는 아무런 관여를 하지 않는다. 해당 프로세스는 단순히 사용자에게 [VBS](Hypervisor.md#가상화-기반-보안)가 활성화되었음을 가시화하는 게 전부이다.

* **[메모리 압축 프로세스](https://en.wikipedia.org/wiki/Virtual_memory_compression)(Memory Compression process)**

    프로세스의 가상 주소 공간으로부터 [페이징 아웃](Memory.md#페이징-파일) 될 페이지를 [HDD](https://ko.wikipedia.org/wiki/하드_디스크_드라이브) 또는 [SSD](https://ko.wikipedia.org/wiki/솔리드_스테이트_드라이브)와 같은 [보조기억장치](Storage.md)로 보내기 전에 압축시켜 [물리 메모리](Memory.md)에 상주시키는 기법을 활용할 수 있다. 이때 타 프로세스의 [워킹 세트](Memory.md#워킹-세트)로부터 방출되어 압축된 [대기 메모리](Memory.md#캐시-메모리)가 바로 메모리 압축 프로세스의 사용자 주소 공간에 저장된다. 그러므로 메모리 압축 프로세스의 워킹 세트는 작업 관리자의 메모리 성능에 표시된 "(압축)" 크기와 일치한다.

## 세션 관리자
[세션 관리자](https://ko.wikipedia.org/wiki/세션_관리자_하위_시스템)(Session Manager; smss.exe)는 윈도우 운영체제에서 가장 최초로 생성되는 사용자 모드 프로세스이다.

# 같이 보기
* [Processes and Threads - Win32 apps &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/procthread/processes-and-threads)
* [Pushing the Limits of Windows: Virtual Memory](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-virtual-memory/ba-p/723750)
