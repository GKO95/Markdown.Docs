# 프로세스 모니터
[프로세스 모니터](https://aka.ms/procmon)(Process Monitor)는 이벤트 활동 이력을 실시간으로 보여주는 [Sysinternals](Sysinternals.md) 유틸리티 프로그램이다. 프로세스 모니터는 시스템이나 어플리케이션의 비정상적인 동작이 의심될 때 유용한 트러블슈팅 도구로 활용된다.

![프로세스 모니터 유틸리티 프로그램](./images/sysinternals_procmon.png)

다음은 프로세스 모니터가 관측하는 이벤트 활동들을 나열 및 소개한다:

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">프로세스 모니터의 이벤트 활동 유형</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">이벤트</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="Registry.md">레지스트리</a></td><td>레지스트리 키 또는 값의 생성, 열거, 탐색, 삭제 등</td></tr><tr><td style="text-align: center;"><a href="FileSystem.md">파일 시스템</a></td><td>로컬 스토리지 및 원격에서 이루어진 파일 시스템 작업</td></tr><tr><td style="text-align: center;"><a href="Network.md">네트워크</a></td><td>UDP 및 TCP 네트워크 활동 (단, 실제로 전송 혹은 수신한 데이터는 파악 불가)</td></tr><tr><td style="text-align: center;"><a href="Process.md">프로세스</a></td><td>프로세스 및 <a href="Process.md#스레드">스레드</a>의 생성 및 종료, <a href="Process.md#가상-주소-공간">가상 주소 공간</a>에 이미지 로드 등</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Profiling_(computer_programming)">프로파일링</a></td><td>시스템에 실행 중인 개별 프로세스마다 매초 항시 수집되는 <a href="Processor.md">CPU</a> 및 <a href="Memory.md">메모리</a>의 누적된 성능 수치 표시<ul><li><i>스레드 프로파일링 활성화는 선택적이며 초당 한 번 또는 열 번 수집하도록 설정할 수 있다.</i></li><li><i><a href="ProcDump.md">ProcDump</a>가 실행 중일 경우, 해당 프로그램으로부터 생성된 진단 결과를 공유받는다.</i></li></ul></td></tr></tbody></table>

## 부트 로깅
윈도우 OS가 재부팅될 시, 어떤 프로세스로부터 무슨 활동이 이루어졌는지 살펴볼 수 있는 `.PML` 확장자의 로그를 남긴다. 만일 Enable Boot Logging이 활성화된 이후 (윈도우 진입에 성공한) 두 번의 재부팅이 발생할 경우, 첫 번째 재부팅 내역만이 로그에 저장된다.

## 프로세스 트리
프로세스 모니터가 캡처를 시작할 때부터 프로세스가 실행 그리고/또는 종료된 시점이 함께 기록되므로, 이를 기반하여 각 프로세스의 생명주기와 이미 종료된 프로세스의 부모와 자식 관계까지 파악할 수 있다.
