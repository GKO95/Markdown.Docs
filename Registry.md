# 레지스트리
**[레지스트리](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry)**(registry)는 [윈도우](Windows.md) [운영체제](https://en.wikipedia.org/wiki/Operating_system)의 시스템 및 어플리케이션의 설정 정보를 담는 데이터베이스이다. 사용자는 [레지스트리 편집기](https://en.wikipedia.org/wiki/Windows_Registry#Registry_editors), [PowerShell](PowerShell.md) 등의 방법으로 편집할 수 있으나 잘못된 변경은 시스템 전체를 망가뜨릴 수 있는 위험이 있기에 매우 조심해야 한다. 

* [윈도우 10](https://en.wikipedia.org/wiki/Windows_10)의 [참가자 빌드 17063](https://blogs.windows.com/windows-insider/2017/12/19/announcing-windows-10-insider-preview-build-17063-pc/)부터 메모리 관리 능력을 극대화하기 위해, [커널 모드](Processor.md#권한-수준)의 [페이징 풀](Memory.md#메모리-풀)에 저장된 레지스트리 데이터는 [사용자 모드](Processor.md#권한-수준)의 "레지스트리(Registry)" [프로세스](Process.md)로 옮겨졌다.

## 레지스트리 구조
![레지스트리 구조](https://learn.microsoft.com/en-us/windows/win32/sysinfo/images/regtree.png)

### 미리 정의된 키
**[미리 정의된 키](https://learn.microsoft.com/en-us/windows/win32/sysinfo/predefined-keys)**(predefined keys)는 시스템에서 기본적으로 제공되는 레지스트리 키이다. 레지스트리 특성상 원하는 레지스트리 키를 곧바로 열 수 없으며 하위 레지스트리 키로 점차 이동해야 한다. 미리 정의된 키는 항상 열려 있기 때문에, 해당 [핸들](Process.md#핸들)은 레지스트리를 접근하기 위한 진입점으로 활용되어 일명 **[루트 키](https://en.wikipedia.org/wiki/Windows_Registry#Root_keys)**(root keys)라고도 부른다.

> 루트 키의 접두사 HKEY는 "handle keys"의 줄임말이다; 해당 유형의 레지스트리를 접근하기 위한 열린 핸들의 레지스트리 키를 의미한다.

윈도우는 레지스트리가 관여하는 설정 대상에 따라 핸들을 다음과 같이 분류한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">미리 정의된 레지스트리 키 핸들 및 설명</caption><colgroup><col style="width: 25%;"/><col style="width: 10%;"/><col style="width: 65%;"/></colgroup><thead><tr><th style="text-align: center;">핸들</th><th style="text-align: center;">약어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">HKEY_LOCAL_MACHINE</td><td style="text-align: center;">HKLM</td><td>(사용자 프로파일과 무관하게) 현 컴퓨터에 대한 구성 정보를 담는다.</td></tr><tr><td style="text-align: center;">HKEY_CLASSES_ROOT</td><td style="text-align: center;">HKCR</td><td>HKEY_LOCAL_MACHINE\Software의 하위 레지스트리 키이며, 윈도우 탐색기로 파일을 열 때 올바른 프로그램으로 열도록 하는 정보를 담는다.</td></tr><tr><td style="text-align: center;">HKEY_CURRENT_USER</td><td style="text-align: center;">HKCU</td><td>현 로그온 사용자의 구성 정보를 담고 있으며, 해당 사용자 프로파일과 연계되어 있다.</td></tr><tr><td style="text-align: center;">HKEY_USERS</td><td style="text-align: center;">HKU</td><td>현 컴퓨터에 로드된 모든 사용자 프로파일의 구성 정보를 담고 있으며, HKEY_CURRENT_USER를 하위 레지스트리 키로 갖는다.</td></tr><tr><td style="text-align: center;">HKEY_CURRENT_CONFIG</td><td style="text-align: center;">-</td><td>현 컴퓨터의 시스템 시작 때 사용되는 하드웨어 프로파일 정보를 담는다.</td></tr></tbody></table>

### 레지스트리 하이브
