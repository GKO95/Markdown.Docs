# 액티브 디렉토리
**[액티브 디렉토리](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)**(Active Directory; AD)는 마이크로소프트에서 개발한 [디렉토리 서비스](https://en.wikipedia.org/wiki/Directory_service)이다; [윈도우](Windows.md) 환경에 특화된 [도메인](https://en.wikipedia.org/wiki/Windows_domain)에 등록된 사용자, 장치, 보안 주체 등의 [네트워크](Network.md) 리소스들을 조직화하여 관리하는 특수한 데이터베이스이다. 액티브 디렉토리는 다음 서비스들을 제공하며 각각 윈도우 서버의 역할로 추가될 수 있다.

* [도메인 서비스](#액티브-디렉토리-도메인-서비스)
* [인증서 서비스](#액티브-디렉토리-인증서-서비스)
* [페더레이션 서비스](#액티브-디렉토리-페더레이션-서비스)
* [경량 디렉토리 서비스](#액티브-디렉토리-경량-디렉토리-서비스)
* [권한 관리 서비스](#액티브-디렉토리-권한-관리-서비스)

## 액티브 디렉토리 논리 구조
> *참고: [Understanding the Active Directory Logical Model | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-the-active-directory-logical-model)*

# 액티브 디렉토리 도메인 서비스
**[액티브 디렉토리 도메인 서비스](https://en.wikipedia.org/wiki/Active_Directory#Domain_Services)**(Active Directory Domain Services; AD DS)는 모든 윈도우 도메인 네트워크의 핵심이며, 도메인 맴버들의 정보를 담고 있다. 다시 말해, AD DS 역할이 설치된 서버는 [도메인 컨트롤러](#도메인-컨트롤러)로 승격된다.

## 도메인 컨트롤러
**[도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows))**(domain controller; DC)

# 액티브 디렉토리 인증서 서비스
**[액티브 디렉토리 인증서 서비스](https://en.wikipedia.org/wiki/Active_Directory#Certificate_Services)**(Active Directory Certificate Services; AD CS)는 [온프레미스](https://en.wikipedia.org/wiki/On-premises_software) [공개 키 기반 구조](https://en.wikipedia.org/wiki/Public_key_infrastructure)를 설립한다.

# 액티브 디렉토리 페더레이션 서비스
**[액티브 디렉토리 페더레이션 서비스](https://en.wikipedia.org/wiki/Active_Directory_Federation_Services)**(Active Directory Federation Services; AD FS)는 [통합 인증](https://en.wikipedia.org/wiki/Single_sign-on) 서비스이다.

# 액티브 디렉토리 경량 디렉토리 서비스
**[액티브 디렉토리 경량 관리 서비스](https://en.wikipedia.org/wiki/Active_Directory#Lightweight_Directory_Services)**(Active Directory Lightweight Directory Services; AD LDS)는 [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) 프로토콜을 적용한 [AD DS](#액티브-디렉토리-도메인-서비스)이다. AD DS와 동일한 기능을 제공하지만 도메인 및 [도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows))의 생성을 요구하지 않는다.

# 액티브 디렉토리 권한 관리 서비스
**[액티브 디렉토리 권한 관리 서비스](https://en.wikipedia.org/wiki/Active_Directory_Rights_Management_Services)**(Active Directory Rights Management Services; AD RMS) 윈도우 서버에 내장된 [정보 권한 관리](https://en.wikipedia.org/wiki/Information_rights_management) 소프트웨어이다.
