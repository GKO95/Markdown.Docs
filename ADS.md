# 액티브 디렉토리
**[액티브 디렉토리](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)**(Active Directory; AD)는 [윈도우 서버 2000](https://en.wikipedia.org/wiki/Windows_2000)에 처음으로 소개된 [마이크로소프트](https://www.microsoft.com)에서 개발한 [디렉토리 서비스](https://en.wikipedia.org/wiki/Directory_service)이다; [윈도우](Windows.md) 환경에 특화된 [도메인](Domain.md)에 등록된 사용자, 장치, 보안 주체 등의 [네트워크](Network.md) 리소스들을 조직화하여 관리하는 특수한 데이터베이스이다. 대체로 도메인과 관련된 작업을 다루기 때문에 [DNS](Domain.md#도메인-네임-시스템) 의존성이 매우 높다. 액티브 디렉토리는 다음 서비스들을 제공하며 각각 윈도우 서버의 역할로 추가될 수 있다.

* [도메인 서비스](#도메인-서비스)
* [인증서 서비스](#인증서-서비스)
* [페더레이션 서비스](#페더레이션-서비스)
* [경량 디렉토리 서비스](#경량-디렉토리-서비스)
* [권한 관리 서비스](#권한-관리-서비스)

# 도메인 서비스
> *참고: [Understanding the Active Directory Logical Model | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-the-active-directory-logical-model)*

**[액티브 디렉토리 도메인 서비스](https://en.wikipedia.org/wiki/Active_Directory#Domain_Services)**(Active Directory Domain Services; AD DS)는 모든 [윈도우 도메인](https://en.wikipedia.org/wiki/Windows_domain)의 핵심이며, 도메인 맴버들의 정보를 담고 있다. 다시 말해, AD DS 역할이 설치된 서버는 해당 도메인의 [도메인 컨트롤러](#도메인-컨트롤러)로 승격되어 데이터베이스에 저장된 개체들(사용자, 컴퓨터, 그룹, 장치 등)을 관리한다.

도메인 서비스의 논리 구조는 다음과 같은 구성으로 이루어진다.

![TailspinToys 및 WingtipToys 포레스트 관계도](https://learn.microsoft.com/en-us/entra/identity/domain-services/media/concepts-forest-trust/kerberos-over-forest-trust-process-diagram.png)

### 도메인 트리
**[도메인 트리](https://learn.microsoft.com/en-us/windows/win32/ad/domain-trees)**(domain tree)는 계층적 구조를 이루는 도메인들의 묶음을 가리키며, 대표적으로 일련의 부모-자식 관계가 도메인 트리를 형성한다. 위의 예시에서 tailspintoys.com 도메인은 europe.tailspintoys.com을 자식 도메인으로 가지며, 이들은 하나의 도메인 트리를 구성한다. 자식 도메인도 자신만의 도메인 컨트롤러(즉, ChildDC1)를 가진다.

* 부모-자식 간에 암묵적인 신뢰 관계를 지니고 있다.

### 포레스트
**[포레스트](https://learn.microsoft.com/en-us/windows/win32/ad/forests)**(forest)는 AD DS가 제공하는 가장 높은 조직 구조이며, 한 개 이상의 도메인 트리로 구성된다. 포레스트 안에 속한 모든 도메인들에게 공용되는 스키마 및 전역 카탈로그를 제공하고 리소스 공유를 지원하는 한편, 타 포레스트와의 상호작용을 제한하는 보안 경계 역할을 맡는다. 위의 예시는 두 포레스트가 있어 각각 TailspinToys 도메인 트리와 WingtipToys 도메인 트리를 내포한다.

* 포레스트 루트 도메인은 트리 루트 도메인들과 암묵적인 신뢰 관계를 지니고 있다.

### 조직 구성 단위
**[조직 구성 단위](https://en.wikipedia.org/wiki/Active_Directory#Organizational_units)**(organizational unit; OU)는 도메인 내 개체들을 정리하는 일종의 폴더와 같은 존재이다. OU로 분류된 개체들은 그룹 정책이나 권한 등을 관리하는 목적으로 활용된다.

## 도메인 컨트롤러
**[도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows))**(domain controller; DC)는 보안 인증 요청에 응답을 하는 서버이며, AD 환경에서는 윈도우 서버 OS가 그 역할을 수행한다. AD가 등장하기 이전의 윈도우 도메인은 아래의 두 DC 유형으로 구성되었다.

* [주 도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows)#Primary_domain_controller)(Primary Domain Controller; PDC): 도메인 데이터베이스 변경 작업이 이루어질 수 있는 도메인의 유일한 DC이며, 일반적으로 최초로 생성된 DC가 이에 해당한다.

* [백업 도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows)#Backup_domain_controller)(Backup Domain Controller; BDC): 도메인 데이터베이스 읽기 작업만 수행할 수 있으며 (예를 들어, 사용자 인증 등), PDC 이외의 구성된 나머지 DC가 해당한다. 만일 PDC가 실패할 경우, BDC 중 하나가 PDC로 승격되어 대체한다.

[액티브 디렉토리](#액티브-디렉토리) 환경에서는 모든 DC가 도메인 데이터베이스의 읽기 및 쓰기 권한을 가지게 된 다중 마스터(multi-master) 모델을 채택하였다. 즉, PDC와 BDC의 개념이 사라졌다. 이는 방대하고 복잡해진 네트워크 환경을 지원할 수 있도록 변경된 설계이다. 

### FSMO 역할
**[유동적 단일 마스터 작업](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles)**(Flexible Single Master Operation; FSMO) 역할은 여러 AD DC가 동시 다발적으로 변경 작업을 수행할 수 있는 다중 마스터 모델에서, 충돌 또는 혼돈이 일어날 시 운영에 치명적일 수 있는 특정 작업을 한 DC에 전임하도록 분류한 역할들이다. 아래는 현재 AD 환경에 주어진 FSMO 역할들을 나열한다.

* [스키마 마스터](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles#schema-master-fsmo-role)
* [도메인 네이밍 마스터](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles#domain-naming-master-fsmo-role)
* [RID 관리자](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles#rid-master-fsmo-role)
* [PDC 에뮬레이터](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles#pdc-emulator-fsmo-role)
* [인프라구조 마스터](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles#infrastructure-master-fsmo-role)

### 전역 카탈로그
**[전역 카탈로그](https://learn.microsoft.com/en-us/windows/win32/ad/global-catalog)**(global catalog; GC)로 승격된 DC는 자신이 속한 도메인의 개체에 대해서는 완전 복제, 그리고 포레스트에 속한 그 외의 모든 개체는 읽기 전용 부분 복제를 가진다.

# 인증서 서비스
**[액티브 디렉토리 인증서 서비스](https://en.wikipedia.org/wiki/Active_Directory#Certificate_Services)**(Active Directory Certificate Services; AD CS)는 [온프레미스](https://en.wikipedia.org/wiki/On-premises_software) [공개 키 기반 구조](https://en.wikipedia.org/wiki/Public_key_infrastructure)를 설립한다.

# 페더레이션 서비스
**[액티브 디렉토리 페더레이션 서비스](https://en.wikipedia.org/wiki/Active_Directory_Federation_Services)**(Active Directory Federation Services; AD FS)는 [통합 인증](https://en.wikipedia.org/wiki/Single_sign-on) 서비스이다.

# 경량 디렉토리 서비스
**[액티브 디렉토리 경량 관리 서비스](https://en.wikipedia.org/wiki/Active_Directory#Lightweight_Directory_Services)**(Active Directory Lightweight Directory Services; AD LDS)는 [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) 프로토콜을 적용한 [AD DS](#액티브-디렉토리-도메인-서비스)이다. AD DS와 동일한 기능을 제공하지만 도메인 및 [도메인 컨트롤러](https://en.wikipedia.org/wiki/Domain_controller_(Windows))의 생성을 요구하지 않는다.

# 권한 관리 서비스
**[액티브 디렉토리 권한 관리 서비스](https://en.wikipedia.org/wiki/Active_Directory_Rights_Management_Services)**(Active Directory Rights Management Services; AD RMS) 윈도우 서버에 내장된 [정보 권한 관리](https://en.wikipedia.org/wiki/Information_rights_management) 소프트웨어이다.
