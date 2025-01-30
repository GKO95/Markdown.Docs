# NotMyFault
**[NotMyFault](https://aka.ms/notmyfault)**, 직역하자면 "제가 한 게 아니에요"는 시스템 [충돌](BSOD.md), [응답 없음](https://en.wikipedia.org/wiki/Hang_(computing)), 그리고 [메모리 누수](https://en.wikipedia.org/wiki/Memory_leak)를 의도적으로 일으키는 [Sysinternals](Sysinternals.md) 유틸리티 프로그램이다. 증상을 트러블슈팅하기 보다는 일부러 증상을 일으켜 트러블슈팅을 연습할 수 있도록 하는 프로그램이다. GUI 또는 명령어로 실행할 수 있는 프로그램이며, 반드시 관리자 권한으로 실행되어야 한다.

> 명령어 버전의 유티릴티는 notmyfaultc.exe 프로그램으로 충돌 및 프리징 증상을 일으킨다.

<table style="table-layout: fixed; width: 100%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">NotMyFault 어플리케이션 화면</caption><thead><tr><th style="text-align: center;">시스템 충돌</th><th style="text-align: center;">시스템 행</th><th style="text-align: center;">메모리 누수</th></tr></thead><tbody><tr style="overflow: auto;"><td style="overflow: inherit;">

![NotMyFault 시스템 충돌 탭](./images/sysinternals_notmyfault_crash.png)
</td><td style="overflow: inherit;">

![NotMyFault 응답 없음 탭](./images/sysinternals_notmyfault_hang.png)
</td><td style="overflow: inherit;">

![NotMyFault 메모리 누수 탭](./images/sysinternals_notmyfault_leak.png)
</td></tr></tbody></table>

NotMyFault는 런타임 때 myfault.sys [드라이버](Driver.md#드라이버)를 `%WinDir%\System32\Drivers` 디렉토리에 생성 및 설치하기 위해 반드시 관리자 권한으로 실행된다. 해당 드라이버는 커널 모드에서 의도적으로 충돌 및 누수 등을 일으키기 위해 필요하며, 프로그램 실행 시 커널에 로드된다.

## 시스템 충돌
총 여덟 가지의 시스템 충돌 요인 중 하나를 선택하여 Crash 버튼을 눌러 블루스크린을 발생시킨다.

시스템 콘솔로 충돌을 일으키려면 아래의 명령어를 입력한다: `crash_type_num`에는 1-8 범위의 숫자가 입력되는데, 이들은 다이얼로그 창에 나열된 시스템 충돌 원인들을 순서에 대응한다. 아래는 6번 선택지의 Stack overflow로 충돌을 일으키기 위한 명령이다.

```terminal
notmyfaultc.exe crash 6
```

## 시스템 행
총 세 가지의 응답 없음 증상을 일으키는 선택지가 있으며, 증상에 대한 설명은 아래 표를 참고한다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">NotMyFault 응답 없음 선택지</caption><colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">선택지</th><th style="text-align: center;">대상</th><th style="text-align: center;">증상</th></tr></thead><tbody><tr><td style="text-align: center;">Hang with <a href="Driver.md#입출력-요청-패킷">IRP</a></td><td style="text-align: center;">드라이버</td><td>myfault.sys 드라이버를 먹통으로 만들어 시스템 충돌도 일으킬 수 없다.</td></tr><tr><td style="text-align: center;">Hang with <a href="Processor.md#지연-프로시저-호출">DPC</a></td><td style="text-align: center;">시스템</td><td>시스템 자체가 아무런 반응이 없어 강제 종료 혹은 <a href="BSOD.md#강제-시스템-충돌">시스템 충돌</a>을 일으켜야 한다.</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Deadlock_(computer_science)">Deadlock</a></td><td style="text-align: center;">어플리케이션</td><td>어플리케이션 자체가 아무런 반응이 없으며 <a href="Process.md">프로세스</a> 강제 종료도 되지 않는다.</td></tr></tbody></table>

시스템 콘솔로 시스템 혹은 드라이버 응답 없음을 일으키려면 아래의 명령어를 입력한다 (단, 세 번째 선택지의 Deadlock은 터미널로 일으킬 수 없다). 아래는 2번 선택지인 Hang with DPC로 시스템 행을 일으키기 위한 명령이다.

```terminal
notmyfaultc.exe hang 2
```

## 메모리 누수
윈도우는 두 종류의 [커널 메모리](Memory.md#메모리-풀)가 있으며, 이들은 각각 페이징 풀(paged pool)과 비페이징 풀(nonpaged pool)이다.

NotMyFault 프로그램은 둘 중 하나를 초당 원하는 [킬로바이트](https://ko.wikipedia.org/wiki/킬로바이트) 단위만큼 누수시킬 수 있다. 커널 메모리의 누수로 메모리 풀이 소진되면 시스템은 응답이 없거나 충돌이 발생할 수 있으며, 혹은 어플리케이션이 비정상적으로 종료될 수 있다. [작업 관리자](TaskMgr.md)나 [성능 모니터](Perfmon.md)로부터 커널 메모리가 증가하는 게 수치적으로 볼 수 있으며, 사용 가능한 메모리가 부족해지면서 시스템 성능이 저하되는 증상을 목격할 수 있다.

> 만일 고급 시스템 설정의 [가상 메모리](Memory.md#페이징-파일)가 "시스템이 관리하는 크기"로 지정되어 있으면 메모리가 부족할 때마다 커밋된 메모리 용량이 자동으로 늘어나므로 증상이 다소 늦게 나타난다.
