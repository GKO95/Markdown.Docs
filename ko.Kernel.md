---
category: 운영체제
title: 커널
---
# 커널
[커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅))(kernel)은 [운영체제](https://ko.wikipedia.org/wiki/운영체제)의 핵심 프로그램으로 일반적으로 시스템 전체에 대한 제어권을 가진다. [메모리](ko.Memory.md)에 항상 상주하여 하드웨어와 소프트웨어 간 상호작용을 가능케 하여, 운영체제의 아래 역할을 담당한다.

* [장치 드라이버](ko.Driver.md#장치-드라이버)를 통한 하드웨어 리소스 제어: 메모리, 입출력, 암호화 등
* [프로세스](ko.Process.md) 간 위의 하드웨어 리소스 할당에 대한 갈등 중재
* 공통 리소스 활용의 최적화: [CPU](ko.Processor.md) 및 캐시 사용률, 파일 시스템, 네트워크 소켓 등

## 아키텍처
운영체제 설계에 따라 커널은 크게 세 가지의 아키텍처로 설계될 수 있다.

![커널 설계에 따른 아키텍처<sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:OS-structure2.svg">위키미디어</a></i></sub>](./images/kernel_structures.svg)

본 부문에서 자주 인용될 "서비스"란, 호출 가능한 커널 루틴 혹은 그 집합을 의미한다.

### 모놀리식 커널
[모놀리식 커널](https://ko.wikipedia.org/wiki/모놀리식_커널)(monolithic kernel)은 모든 서비스가 단일 프로그램으로 빌드되어 [커널 공간](ko.Processor.md#보호-링)에서 처리하는 구조이다. 단조로운 구조에 관리가 매우 편하고, 단일 프로그램에서 모든 커널 작업 및 서비스가 수행되니 성능 속도가 매우 빠르다. 단, 커널 공간의 특성상 사소한 오류가 시스템 전체에 영향을 줄 수 있는 위험이 항상 존재한다. 운영체제 런타임 도중에 장치 드라이버를 언제든지 불러올 수 있는 모듈성(modularity)을 지원한다.

마이크로소프트의 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 및 [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x) 시리즈가 모놀리식 커널를 사용하였다.

### 마이크로커널
[마이크로커널](https://ko.wikipedia.org/wiki/마이크로커널)(microkernel)은 운영체제 구동에서 기초적이지만 필연적인 저급 커널 서비스만을 제외한 나머지를 [사용자 공간](ko.Processor.md#보호-링)으로 분리시킨 구조이다: [스케줄링](ko.Processor.md#스케줄링), 메모리 관리, 기초적인 [IPC](ko.Process.md#프로세스-간-통신)가 최소한으로 마련되어야 할 서비스이다. 한편, 사용자 모드에서 고급 커널 서비스를 제공하는 프로그램들을 서버(server)라고 부른다.

> 마이크로커널의 기초적인 IPC 서비스는 사용자 모드에 위치한 서버 혹은 장치 드라이버 간 통신을 위해 반드시 필요한 기능이다.

모놀리식 커널의 단점인 방대한 커널 이미지로 인한 코드 관리의 어려움 및 장치 드라이버 충돌에 의한 커널 전체에 미치는 악영향을 해소하고자 고안되었다. 커널 서비스의 서버화는 커널 개발 및 업데이트 시간을 단축시키지만, 사용자 모드에 위치하여 하드웨어 접근성 효율이 떨어진다. IPC 통신으로 인한 성능 저하 및 커널 동작 절차의 복잡성도 단점으로 함께 지적되었다.

### 하이브리드 커널
[하이브리드 커널](https://ko.wikipedia.org/wiki/하이브리드_커널)(hybrid kernel)은 마이크로커널처럼 서비스에 따라 서버가 나뉘어져 있으나, 모놀리식 커널처럼 전부 (혹은 대부분) 커널 공간에 존재한다. 결국 시스템 안전성 취약점은 여전히 존재하지만, 마이크로커널에 비해 사용자 및 커널 모드 전환이 불필요하여 커널 서비스의 성능 [오버헤드](https://ko.wikipedia.org/wiki/오버헤드)가 없다.

마이크로소프트의 [윈도우 NT](ko.Windows.md)가 하이브리드 커널의 영향을 받은 대표적인 운영체제이다.

# NT 커널
[윈도우 NT](ko.Windows.md) 운영체제의 커널 이미지 `ntoskrnl.exe`는 아래와 같이 구성된다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">윈도우 커널 이미지의 구성</caption>
<colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col/><col/><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead><tr><th style="text-align: center;">이미지</th><th style="text-align: center;">계층</th><th colspan="10" style="text-align: center;">구성</th></tr></thead>
<tbody><tr><td rowspan="3" style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/Ntoskrnl.exe"><code>ntoskrnl.exe</code></a></td><td rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</td><td colspan="9" style="text-align: center;">시스템 서비스 담당자</td></tr><tr><td style="text-align: center;">입출력 관리자</td><td style="text-align: center;">캐시 관리자</td><td style="text-align: center;">보안 참조 모니터</td><td style="text-align: center;">전력 관리자</td><td style="text-align: center;">PnP 관리자</td><td style="text-align: center;">메모리 관리자</td><td style="text-align: center;">프로세스 관리자</td><td style="text-align: center;">객체 관리자</td><td style="text-align: center;">구성 관리자</td><td style="text-align: center;">ALPC</td></tr>
<tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">Kernel</a></td><td colspan="10" style="text-align: center;">-</td></tr></tbody>
</table>

Executive는 특정 작업을 수행하는 여러 구성원들로 이루어진 상위 계층이다. 사용자 모드에서 [시스템 서비스](ko.WinAPI.md#시스템-서비스)를 호출하면 해당 작업에 대응하는 구성원의 루틴이 실행된다. 아래 접두사로부터 어떤 목적의 커널 서비스인지 분별할 수 있다.
