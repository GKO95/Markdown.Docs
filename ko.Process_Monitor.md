---
category: 마이크로소프트
title: Process Monitor
alias: procmon
visible: true
---
# 프로세스 모니터
[프로세스 모니터](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)(Process Monitor)는 파일 시스템, [레지스트리](ko.Registry.md), 그리고 [프로세스](ko.Process.md) 및 [스레드](ko.Process.md#스레드) 활동 이력을 실시간으로 모니터링하는 [Sysinternals](ko.Sysinternals.md) 유틸리티 프로그램이다. 비정상적인 시스템 또는 프로그램 동작이 의심될 경우, 프로세스 모니터로부터 캡처된 로그를 정상적으로 동작한 시스템 또는 프로그램과 비교하는 방식으로 트러블슈팅을 진행할 수 있다.

![프로세스 모니터 유틸리티 프로그램](./images/sysinternals_procmon.png)

캡처되는 활동들이 매우 많다보니 원활한 분석을 위해서는 모니터링에 포함하거나 제외할 정보를 필터링 또는 하이라이트를 적용하거나, 예의주시할 활동을 차후 쉽게 찾아갈 수 있도록 북마크를 지정하는 걸 장려한다.

다음은 프로세스 모니터가 제공하는 기능들을 간략히 소개한다:

* **Process Tree**

    프로세스 모니터가 캡처를 시작할 때부터 프로세스가 실행 그리고/또는 종료된 시점이 함께 기록되므로, 이를 기반하여 각 프로세스의 생명주기와 이미 종료된 프로세스의 부모-자식 관계까지 파악할 수 있다.

* **Enable Boot Logging**

    Enable Boot Logging, 일명 부트로그 옵션은 윈도우 운영체제가 재부팅되는 시점에서 어떤 프로세스에서 어떤 활동이 이루어졌는지 살펴볼 수 있는 `.PML` 확장자의 로그를 남긴다.

    > 만일 Enable Boot Logging이 활성화된 이후 (윈도우 진입에 성공한) 두 번의 재부팅이 발생할 경우, 첫 번째 재부팅 내역만이 로그에 저장된다.
