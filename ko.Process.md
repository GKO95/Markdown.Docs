---
category: 운영체제
title: 프로세스
---
# 프로세스
[프로세스](https://ko.wikipedia.org/wiki/프로세스)(process)는 운영체제에서 프로그램(예를 들어, `.EXE` 확장자 파일 유형)을 실행하기 위해 필요한 데이터와 리소스를 담는 컨테이너이다. 프로세스와 프로그램(일명 [어플리케이션](https://ko.wikipedia.org/wiki/응용_소프트웨어); application)은 엄연히 다른 존재이다. 아래는 [파일 탐색기](https://ko.wikipedia.org/wiki/파일_탐색기) 프로그램에서 창을 세 개 열어 각각 문서(Documents), 다운로드(Download), 그리고 사진(Pictures) 디렉토리로 이동한 것을 [작업 관리자](https://ko.wikipedia.org/wiki/작업_관리자_(윈도우))로 보여준다.

![작업 관리자에 표시된 파일 탐색기 (Windows Explorer) 프로그램의 프로세스들](./images/process_application_difference.png)

[C](ko.C.md)/[C++](ko.Cpp.md) 등의 프로그래밍 언어로 작성된 코드를 컴파일하여 생성된 부산물이 바로 프로그램이다. 프로그램은 운영체제가 이해할 수 있는 [기계어](https://ko.wikipedia.org/wiki/기계어)의 집합체(일명 프로그램 이미지)에 불과하며, 새로 컴파일을 하거나 파일에 손상이 가하지 않는 이상 내용물은 절대 변하지 않는다. 다시 말해, 프로그램은 목적을 완수하기 위해 따라야 하는 작업 지침서이고 이를 수행할 작업자와 준비물이 배치된 공간이 바로 프로세스이다.

> 객체지향 프로그래밍 개념에 익숙하다면, 프로그램과 프로세스를 각각 클래스와 객체로 간주할 수 있다. 실제로 프로세스를 프로그램의 객체(instance)라고 부르기도 한다.

동일한 프로그램이라도 각 프로세스는 독립적으로 다른 작업을 수행할 수 있다. 위의 예시에서 세 개의 파일 탐색기 프로세스가 각자 다른 경로로 이동하여도 이러한 행위는 타 프로세스에 어떠한 영향을 미치지 않는다. 프로그램을 실행하면 다수의 프로세스가 생성될 수 있는데, 이들은 각각 수행하는 역할이 달라 효율성을 고려한 개발자의 설계 의도가 반영되기도 한다.

다음은 프로세스를 구성하고 있는 리소스들이다.

* 프로그램 이미지
* [가상 주소 공간](#가상-주소-공간)
* [스레드](#스레드)
* [핸들](#핸들)

## 가상 주소 공간
[가상 주소 공간](https://ko.wikipedia.org/wiki/가상_주소_공간)(virtual address space; VAS)은 프로세스마다 데이터를 저장할 수 있는 가상의 [메모리](ko.Memory.md) 공간이다. 여기서 "가상"이 갖는 의미는 매우 중요하며, 아래의 성질들을 충족시킨다.

![가상 주소 공간과 물리 메모리의 관계<sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:Virtual_address_space_and_physical_address_space_relationship.svg">위키미디어</a></i></sub>](./images/process_vas.png)

1. **독립성**

    가상의 메모리 공간이므로, 설치된 [물리 메모리](https://ko.wikipedia.org/wiki/주기억장치)(즉, [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리))의 크기에 의한 제약이 없다. 시스템에 4 GB RAM이 설치되었다고 가정할 시, 1 GB 메모리 용량이 필요한 프로세스가 동시에 네 개 이상 실행되어도 메모리 부족 문제가 발생하지 않는다. 다만, 성능 저하가 나타날 수 있으며 자세한 내용은 페이징(paging) 기법을 다룰 때 설명할 예정이다.

2. **고립성**

    개별적 메모리 공간이므로, 일반적으로 프로세스 간 접근이 불가하고 어떠한 영향을 미치거나 받지도 않는다. 위의 파일 탐색기 예시에서 제가각 다른 작업을 할 수 있던 것은 VAS 때문이며, 만일 프로세스 하나가 충돌로 종료되어도 나머지는 아무렇지 않은 듯 온전히 실행된다. 예외로 모든 프로세스가 공통적으로 갖는 시스템 공간이나 타 프로세스와 함께 리소스를 사용하기 위한 공유 메모리 등이 있다.

3. **연속성**

    매핑된 메모리 공간이므로, 물리 메모리상 띄엄띄엄 분산된 메모리 조각들을 하나의 연속된 가상 메모리 공간으로 구현한다. 가상 메모리의 [페이지](#페이지)(page)와 물리 메모리의 [페이지 프레임](#페이지)(page frame)를 일대일 매핑하는 것은 [메모리 관리 장치](https://ko.wikipedia.org/wiki/메모리_관리_장치)(MMU) 하드웨어에서 담당한다. 해당 성질은 메모리의 [파편화](https://en.wikipedia.org/wiki/Fragmentation_(computing))를 방지하는 효과를 기대할 수 있다.

가상 주소 공간은 크게 (1) 개별 프로세스 데이터 및 리소스를 저장하는 "프로세스 공간"과 (2) 모든 프로세스에 공통적인 윈도우 운영체제 데이터 및 리소스를 담고있는 "시스템 공간"으로 나뉘어지는데, 흔히 전자와 후자를 각각 사용자 공간(user space) 및 커널 공간(kernel space)이라고 부르기도 한다. 윈도우 운영체제 및 아키텍처에 따라 각 공간들이 가질 수 있는 최대 크기는 아래와 같이 [지정](https://learn.microsoft.com/en-us/windows/win32/memory/memory-limits-for-windows-releases)되어 있다.

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">가상 주소 공간의 최대 크기</caption>
<colgroup><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/><col style="width: 25%;"/></colgroup>
<thead><tr><th rowspan="2" style="text-align: center;">가상 주소 공간</th><th rowspan="2" style="text-align: center;">32비트 프로세스</th><th colspan="2" style="text-align: center; border-bottom-style: none;">64비트 프로세스</th></tr><tr><th style="text-align: center;">윈도우 8 (혹은 윈도우 서버 2012) 그리고 이전</th><th style="text-align: center;">윈도우 8.1 (혹은 윈도우 서버 2012 R2) 그리고 이후</th></tr></thead>
<tbody style="text-align: center;">
<tr><td>사용자 공간</td><td>2 GB</td><td>8 TB</td><td>128 TB</td></tr>
<tr><td>커널 공간</td><td>2 GB</td><td>8 TB</td><td>128 TB</td></tr>
</tbody>
</table>

> 32비트와 64비트는 각각 4 GB (= $2^{32}$) 그리고 16 EB (= $2^{64}$) 크기만큼의 주소를 표현할 수 있다.

프로세스에 `IMAGE_FILE_LARGE_ADDRESS_AWARE` 플래그 설정여부에 따라 사용자 공간을 2 GB으로 제한할 지 (32비트 프로세스 기본값), 혹은 그 이상 크기를 확장할 지 (64비트 프로세스 기본값) 선택할 수 있다. 특히, 해당 플래그가 설정된 32비트 프로세스는 32비트 윈도우 운영체제에서 1 GB 커널 공간 확보를 위해 3 GB만 사용자 공간으로 사용한다. 반면 64비트 윈도우 운영체제에서는 4 GB 전체를 사용자 공간으로 활용한다.

![비주얼 스튜디오에서 <code>IMAGE_FILE_LARGE_ADDRESS_AWARE</code> 플래그 설정여부 위치](./images/process_large_addresses.png)

가상 주소 공간을 확인할 수 있는 도구로 [Sysinternals](ko.Sysinternals.md)의 [VMMap](ko.VMMap.md) 유틸리티 프로그램이 있다.

### 페이지
[페이지](https://ko.wikipedia.org/wiki/페이지_(컴퓨터_메모리))(page)는 가상 주소 공간에서 관리되는 가장 작은 단위의 메모리 블록이며, 이와 매핑된 하나의 물리 메모리 조각을 페이지 프레임(page frame) 혹은 간단히 프레임(frame)이라고 부른다. 페이지 및 프레임의 크기는 고정되어 있으며 일반적으로 4 KB이다.

페이지에는 세 가지 [상태](https://learn.microsoft.com/en-us/windows/win32/memory/page-state)가 존재하며, 이에 따라 가상 메모리를 사용할 수 있는지 여부가 결정된다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">가상 주소 공간의 페이지 상태</caption>
<colgroup><col style="width: 10%;"/><col style="width: 90%;"/></colgroup>
<thead><tr><th style="text-align: center;">상태</th><th style="text-align: center;">설명</th></tr></thead>
<tbody><tr><td style="text-align: center;">여유<br/>(Free)</td><td>가상 주소 공간에서 사용되고 있지 않는 가상 메모리이다.</td></tr>
<tr><td style="text-align: center;"><a href="ko.Memory.md#가상-메모리">예약된</a><br/>(Reserved)</td><td>가상 주소 공간의 메모리를 차지하지만 시스템에서는 메모리를 사용하지 않는 것으로 인식한다.</td></tr>
<tr><td style="text-align: center;"><a href="ko.Memory.md#커밋된-메모리">커밋된</a><br/>(Committed)</td><td>가상 주소 공간의 메모리를 차지하면서 시스템에서는 메모리를 사용하는 것으로 인식한다.<ul><li>페이지가 커밋되었다고 곧바로 물리 메모리를 할당받는 게 아니다.</li><li>시스템에 커밋된 메모리 전체 크기를 커밋 총량(commit charge)이라고 부르며 "<a href="ko.Memory.md#메모리">물리 메모리</a> + <a href="ko.Memory.md#페이징-파일">페이징 파일</a>" 크기만큼으로 제한된다.</li></ul></td></tr></tbody>
</table>

## 핸들
[핸들](https://ko.wikipedia.org/wiki/핸들_(컴퓨팅))(handle)은 프로세스가 파일이나 오브젝트와 같은 리소스에 접근하거나 불러오기 위해 사용하는 추상적인 [참조](ko.C.md#포인터)이다. 일반적으로 핸들을 아래와 같이 선언한다.

```c
typedef void* HANDLE;
```

비록 핸들은 `void` 자료형 포인터로 나타날지언정, 가상 메모리상 데이터가 위치한 주소를 직접적으로 가리키는 포인터와 엄밀히 다른 존재이다. 가상 주소 공간 안에는 리소스들의 포인터가 나열된 배열 혹은 테이블이 있는데, 여기서 원하는 리소스에 접근하기 위한 테이블 진입점을 색인하는 요소가 바로 핸들이다. 만일 어떠한 이유로 리소스 포인터가 바뀌었다 하여도, 테이블에서의 해당 리소스 진입점은 여전하기 때문에 핸들의 유효성이 그대로 유지된다는 점에서 "추상적"인 참조라고 언급하였다.

리소스의 핸들을 생성 및 제거하는 것을 각각 열고(open) 닫는다(close)고 표현한다. 더 이상 사용되지 않는 리소스의 열린 핸들을 전부 닫아주지 않으면 프로세스가 종료될 때까지 테이블 및 메모리에 리소스가 계속 잔여하는 [핸들 누수](https://ko.wikipedia.org/wiki/핸들_누수)(handle leak) 문제가 발생한다.

# 스레드
[스레드](https://ko.wikipedia.org/wiki/스레드_(컴퓨팅))(thread)는 프로세스의 프로그램 이미지 코드를 실행하기 위해 [CPU](ko.Processor.md)에서 처리할 수 있는 작업 흐름의 단위이다. 프로세스는 기본적으로 하나의 스레드를 갖는데 개발자의 설계에 의해 추가로 생성하여 두 개 이상의 스레드를 활용할 수 있고, 동일한 가상 주소 공간에 상주하기 때문에 서로의 리소스를 아무런 제약없이 공유할 수 있다. 그러나 프로세스에는 최소한 하나의 스레드가 존재해야 하므로, 모든 스레드가 종료되면 해당 프로세스는 자동적으로 함께 종료된다.

# 같이 보기
* [Processes and Threads - Win32 apps &#124; Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/procthread/processes-and-threads)
* [Pushing the Limits of Windows: Virtual Memory](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-virtual-memory/ba-p/723750)
