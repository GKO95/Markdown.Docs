# DTrace
> *출처: [DTrace on Windows - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/dtrace)*

[**DTrace**](https://en.wikipedia.org/wiki/DTrace)는 운영 중인 어플리케이션 및 [커널](Kernel.md)을 실시간으로 트러블슈팅할 수 있도록 하는 [C 언어](C.md)로 제작된 [트레이싱](https://en.wikipedia.org/wiki/Tracing_(software)) 프레임워크이다. 처음에는 [Sun Microsystems](https://en.wikipedia.org/wiki/Sun_Microsystems)에서 자신들의 UNIX 기반 운영체제인 [Solaris](https://en.wikipedia.org/wiki/Oracle_Solaris)를 위해 개발하였으나, 추후 오픈 소스로 공개되며 [Linux](https://en.wikipedia.org/wiki/Linux) 및 [Windows](Windows.md) 버전으로도 포팅되었다. DTrace는 [Windows Server 2025](https://en.wikipedia.org/wiki/Windows_Server_2025)부터 기본적으로 시스템에 탑재되었지만,  Windows 19040 이상인 그 이전 빌드의 경우에서 DTrace를 별도로 [설치](https://learn.microsoft.com/windows-hardware/drivers/devtest/dtrace#installing-dtrace-under-windows)해야 한다.

DTrace가 지원되는 Windows에서 이를 활성화하려면 BCD를 설정해야 하지만 아래 사항에 대해 유의하도록 한다.

```
bcdedit /set dtrace on
```

* Windows 업그레이드로 빌드가 바뀌면 BCD의 DTrace 설정도 원래대로 초기화된다.
* [BitLocker](BitLocker.md)를 사용하고 있다면 임시 중단(suspend)시켜 BCD 변경으로 인해 복구 키를 요구하는 걸 방지할 수 있다.

## 아키텍처
![DTrace 아키텍처](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/images/dtrace-architecture.png)

DTrace 엔진은 크게 두 프로그램으로 구성된다.

1. DTrace.exe: 사용자의 상호작용으로 입력받은 D 스크립트를 중간 언어인 DIF로 컴파일하여 커널 영역으로 전달한다.
1. DTrace.sys: 사용자 모드에서 전달받은 DIF를 실행하는 DIF Virtual Machine을 제공한다.

한편, Traceext.sys는 Windows 커널의 확장 드라이버로 DTrace가 트레이싱을 하는 데 필요한 Windows 기능을 노출시킨다. [스택 추적](https://en.wikipedia.org/wiki/Stack_trace)이나 메모리 접근이 이루어질 때, 커널은 DTrace가 필요한 기능이나 정보를 트레이싱 확장 드라이버에 구현된 [콜아웃](https://learn.microsoft.com/windows-hardware/drivers/network/callout)을 통해 제공한다.
