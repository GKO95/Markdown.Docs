# 프로세스 모니터
[프로세스 모니터](https://aka.ms/procmon)(Process Monitor)는 [레지스트리](Registry.md), [파일 시스템](FileSystem.md), [네트워크](Network.md), [프로세스](Process.md#프로세스) 및 [스레드](Process.md#스레드), 그리고 [프로파일링](https://en.wikipedia.org/wiki/Profiling_(computer_programming)) 이벤트 활동 이력을 실시간으로 보여주는 [Sysinternals](Sysinternals.md) 유틸리티 프로그램이다. 프로세스 모니터는 시스템이나 어플리케이션의 비정상적인 동작이 의심될 때 유용한 트러블슈팅 도구로 활용된다.

> 프로세스 모니터를 잘 활용하려면 원하는 정보만을 얼마나 잘 필터링하고 탐색할 수 있는지 역량이 가장 중요하다.

![프로세스 모니터 유틸리티 프로그램](./images/sysinternals_procmon.png)

* **Process Tree**: 프로세스 모니터가 캡처를 시작할 때부터 프로세스가 실행 그리고/또는 종료된 시점이 함께 기록되므로, 이를 기반하여 각 프로세스의 생명주기와 이미 종료된 프로세스의 부모와 자식 관계까지 파악할 수 있다.

* **Enable Boot Logging**: 윈도우 OS가 재부팅될 시, 어떤 프로세스로부터 무슨 활동이 이루어졌는지 살펴볼 수 있는 `.PML` 확장자의 로그를 남긴다. 만일 Enable Boot Logging이 활성화된 이후 (윈도우 진입에 성공한) 두 번의 재부팅이 발생할 경우, 첫 번째 재부팅 내역만이 로그에 저장된다.