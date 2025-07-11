# 로그온
> *참고: [Windows Logon Scenarios | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-logon-scenarios)*

**[로그온](https://en.wikipedia.org/wiki/Login)**(logon)은 개인이 컴퓨터 시스템 또는 프로그램에 접속하기 위한 식별 및 인증 절차이다. [윈도우](Windows.md) [운영체제](https://en.wikipedia.org/wiki/Operating_system)는 모든 사용자가 로컬 및 네트워크 리소스를 접근하려면 반드시 유효한 계정을 통해 로그온 할 것을 요한다. 먼저 (1) 사용자 인증이 이루어진 다음, (2) 인증된 사용자의 권한을 확인하여 리소스 접근을 제어하고 보호한다. 가장 기본적인 사용자 로그온 인증 방식으로 비밀번호 기반이 존재한다.

### 보안 식별자
**[보안 식별자](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers)**(security identifier; SID)는 사용자, 그룹, 그리고 컴퓨터 계정을 식별하는 가변 크기의 데이터 구조이다. 네트워크상 각 계정마다 최초로 생성되면 고유의 SID가 부여된다. 윈도우 내부 프로세스들은 계정의 (사용자 이름이나 그룹명이 아닌) SID를 조회한다.

## 인증서
**인증서**(certificate)는 한 개체와 개체의 공개키 정보를 담는 [디지털 서명](https://en.wikipedia.org/wiki/Digital_signature)으로, 언급한 두 정보의 연관성을 입증한다. 신뢰할 수 있는 조직 (또는 개체), 일명 [인증 기관](https://en.wikipedia.org/wiki/Certificate_authority)에서 개체의 존재를 검증한 이후 인증서를 발행한다.

인증서는 다른 정보들을 담을 수 있다: 예를 들어, X.509는 인증서 형식 및 일련번호, 서명에 사용된 알고리즘, 인증서를 발행한 인증 기관명, 인증을 요청한 개체의 이름과 공개키, 그리고 인증 기관의 서명을 포함한다.

### 자격 증명
[자격 증명](https://en.wikipedia.org/wiki/Credential#Information_technology)(credential)은 사용자 또는 자동화 프로세스 등을 포함한 [보안 주체](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-principals)가 스스로를 식별하기 위해 이전에 인증한 로그온 데이터를 가리킨다: 비밀번호, 케르베로스 프로토콜 티켓 등이 해당한다.

## 로컬 보안 기관
**[로컬 보안 기관](https://learn.microsoft.com/en-us/windows/win32/secauthn/lsa-authentication)**(Local Security Authority; LSA)은 사용자를 인증하고 로컬 시스템에 로그온을 시키는 보호된 서브시스템이다. LSA는 또한 시스템상 로컬 보안의 모든 측면에 대한 정보(통칭 로컬 보안 정책; Local Security Policy)를 관리한다.

# 로그온 세션
**[로그온 세션](https://en.wikipedia.org/wiki/Login_session)**(logon session)은 단일 사용자가 컴퓨터 시스템에 [로그온](#로그온) 때 구축되는 [세션](https://en.wikipedia.org/wiki/Session_(computer_science))이다. 로그온 세션은 사용자에 의해 실행된 프로세스 및 시스템 객체들로 구성되었으며, 로그아웃 할 시 실행된 프로세스들은 모두 종료된다.

### 접근 토큰
**[접근 토큰](https://en.wikipedia.org/wiki/Access_token)**(access token), 또는 간단히 **토큰**(token)은 로그온 세션에 대한 보안 정보를 가지고 있으며 사용자, 그룹, 그리고 권한을 식별한다. 로그온 할 때 생성되어 사용자에 의해 실행된 모든 프로세스는 해당 토큰의 복사본을 가진다. 시스템의 보호되어야 할 리소스 접근이나 동작 수행을 제어하는 데 활용된다.

* 기본 토큰(primary token)
* 기장 토큰(Impersonation token)

### 로그온 식별자
**[로그온 식별자](https://learn.microsoft.com/en-us/windows/win32/secgloss/l-gly)**(logon identifier), 일명 **로그온 ID**는 로그온 세션을 식별하며 사용자가 로그오프를 할 때까지 유효한 LUID이다.

> [로컬 고유 식별자](https://learn.microsoft.com/en-us/windows/win32/secgloss/l-gly)(locally unique identifier; LUID)는 시스템이 재시작할 때까지 고유성을 보장하는 운영체제로부터 생성된 64비트 값이다.

컴퓨터가 실행되는 동안 로그온 ID는 고유하며, 다른 로그온 세션과 동일한 ID를 가질 수 없다. 그러나 사용 가능한 로그온 ID 집합은 컴퓨터가 시작될 때 초기화된다. [`GetTokenInformation`](https://learn.microsoft.com/en-us/windows/win32/api/securitybaseapi/nf-securitybaseapi-gettokeninformation) 함수로 불러온 [접근 토근](#접근-토큰) 정보를 반영하는 [TOKEN_STATISTICS](https://learn.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-token_statistics) 구조체에서 AuthenticationId 맴버는 로그온 ID를 나타낸다.

### 로그온 SID
로그온 SID(Logon SID)는 로그온 세션을 식별하는 [보안 식별자](#보안-식별자)이다. 로그온 세션 도중 DACL에서 로그온 SID를 사용하여 접근을 제어할 수 있다. 로그온 SID는 사용자가 로그오프를 할 때까지 유효하다. 컴퓨터가 실행되는 동안 로그온 SID는 고유하며, 다른 로그온 세션과 동일한 SID를 가질 수 없다. 그러나 사용 가능한 로그온 ID 집합은 컴퓨터가 시작될 때 초기화된다. [`GetTokenInformation`](https://learn.microsoft.com/en-us/windows/win32/api/securitybaseapi/nf-securitybaseapi-gettokeninformation) 함수로 불러온 [접근 토근](#접근-토큰)의 [TOKEN_GROUPS](https://learn.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-token_groups) 구조체로부터 로그온 SID를 확인한다.

## 윈도우 스테이션
**[윈도우 스테이션](https://learn.microsoft.com/en-us/windows/win32/winstation/window-stations)**(window station)은 사용자의 [프로세스](Process.md)와 이를 화면에 표시할 [데스크탑](Shell.md#데스크탑)을 관리한다. 윈도우 스테이션은 [클립보드](https://learn.microsoft.com/en-us/windows/win32/dataxchg/clipboard) 및 [아톰 테이블](https://learn.microsoft.com/en-us/windows/win32/dataxchg/about-atom-tables)을 가지며, 안에 실행된 모든 프로세스가 이를 접근하여 활용할 수 있다. 로그온 세션마다 사용자와 상호작용이 가능한 유일한 윈도우 스테이션인 "Winsta0"가 한 개 존재한다. 그 외의 나머지는 상호작용이 불가하며, 대표적으로 다음 윈도우 스테이션이 해당한다.

* [`Service-0x0-3e7$`](https://learn.microsoft.com/en-us/windows/win32/winstation/window-station-and-desktop-creation): 로컬 시스템 계정의 보안 컨텍스트 및 [SERVICE_INTERACTIVE_PROCESS](https://learn.microsoft.com/en-us/windows/win32/services/interactive-services) 속성이 없는 [서비스](Service.md)가 실행되는 윈도우 스테이션이다.
* `Service-0x0-3e4$`
* `Service-0x0-3e5$`
