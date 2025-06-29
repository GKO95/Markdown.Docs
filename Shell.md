# 셸
**[셀](https://en.wikipedia.org/wiki/Shell_(computing))**(shell)은 사용자와 시스템 간 상호작용을 관리하는 컴퓨터 프로그램이다. 일반적으로 [명령줄 인터페이스](https://en.wikipedia.org/wiki/Command-line_interface)(CLI) 프로그램이지만, 일부 [그래픽 사용자 인터페이스](https://en.wikipedia.org/wiki/Graphical_user_interface)(GUI) 프로그램 또한 셸로 분류된다. Windows OS에서는 대표적으로 [명령 프롬프트](https://en.wikipedia.org/wiki/Cmd.exe) 및 [Windows Shell](https://learn.microsoft.com/en-us/windows/win32/shell/shell-entry)이 해당한다. 여기서 후자는 아래의 기능들을 아울러 명칭한다.

* [파일 탐색기](https://en.wikipedia.org/wiki/File_Explorer): 셸의 네임스페이스, 즉 디스크와 파일 및 폴더를 탐색할 수 있는 컴포넌트이다. 이를 실행하는 explorer.exe는 그 외에도 작업 표시줄, 시작 메뉴, 그리고 데스크탑 일부를 실행하는 등의 역랗도 담당한다.
* [작업 표시줄](https://en.wikipedia.org/wiki/Taskbar)
* [시작 메뉴](https://en.wikipedia.org/wiki/Start_menu)
* [데스크탑](#데스크탑)

## 데스크탑
**[데스크탑](https://learn.microsoft.com/en-us/windows/win32/winstation/desktops)**(desktop)은 논리 디스플레이 화면을 표시하고, 이에 구현될 [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface)를 구성하는 [창](https://learn.microsoft.com/en-us/windows/win32/winmsg/windows), [메뉴](https://learn.microsoft.com/en-us/windows/win32/menurc/menus), [후크](https://learn.microsoft.com/en-us/windows/win32/winmsg/hooks) 등의 [사용자 인터페이스](https://en.wikipedia.org/wiki/User_interface) 객체들을 함유한다. 해당 객체는 [커널 공간](Process.md#가상-주소-공간)의 [페이징 풀](Memory.md#메모리-풀)에 저장되며, 이를 *데스크탑 힙* 영역이라 부른다. [윈도우 스테이션](#윈도우-스테이션)은 최소한 한 개의 ("Default") 데스크탑을 가진다.

Winsta0은 세 가지의 데스크탑을 가지며 오로지 하나만 활성화(일명 입력 데스크탑)된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Winsta0의 세 가지 데스크탑 유형</caption><colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup><thead><tr><th style="text-align: center;">데스크탑</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">Default</td><td>Winlogon에 의해 생성되어 사용자와 상호작용하는 데스크탑이다.</td></tr><tr><td style="text-align: center;">Disconnect</td><td>원격 세션에서 로그오프를 한 게 아닌 단순히 연결이 끊긴 경우에 활성화된다.</td></tr><tr><td style="text-align: center;">Winlogon</td><td>사용자 로그온 과정의 입력 데스크탑으로, 셸이 표시할 준비가 되었거나 30초를 경과하면 시스템은 Default 데스크탑으로 전환한다. 한편, <a href="https://en.wikipedia.org/wiki/Control-Alt-Delete">CTRL+ALT+DEL</a> 조합이나 UAC 다이얼로그 상자가 열려도 활성화된다.</td></tr></tbody></table>

> 입력 데스크탑은 사용자가 그 외 데스크탑과 상호작용이 불가하기 때문에, UAC 프롬프트 중에는 Default 데스크탑의 리소스 접근을 차단하는 역할을 한다.

윈도우 스테이션의 상호작용 여부에 따라 Default 데스크탑 힙 크기가 달라진다. 아래 [윈도우 서브시스템](Windows.md#윈도우-서브시스템)의 레지스트리 값에는 SharedSection을 확인할 수 있다.

* `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Subsystems\Windows`

    ```
    SharedSection=1024,20480,768
    ```

    해당 항목의 세 값들은 데스크탑 유형에 따라 각각 순서대로 아래 메모리 크기를 가진다.
    
    1. 모든 데스크탑이 접근할 수 있는 공유 페이징 풀: 1024 KB
    2. Default 데스크탑 힙 (상호작용 가능): 20480 KB
    3. Default 데스크탑 힙 (상호작용 불가): 768 KB

* Winsta0의 나머지 두 데스크탑의 힙 크기는 다음과 같이 고정된다.

    1. Disconnect: 96 KB
    1. Winlogon: 192 KB
