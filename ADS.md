# 액티브 디렉토리
**[액티브 디렉토리](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)**(Active Directory; AD)는 마이크로소프트에서 개발한 [디렉토리 서비스](https://en.wikipedia.org/wiki/Directory_service)이다; [윈도우](Windows.md) 환경에 특화된 [도메인](https://en.wikipedia.org/wiki/Windows_domain)에 등록된 사용자, 장치, 보안 주체 등의 [네트워크](Network.md) 리소스들을 조직화하여 관리하는 특수한 데이터베이스이다.

액티브 디렉토리를 구성하기 위한 필수 사항들은 다음과 같다:

* 윈도우 서버 OS
* 윈도우 도메인
* [액티브 디렉토리 도메인 서비스](#액티브-디렉토리-도메인-서비스)(일명 AD DS)
   * [도메인 네임 시스템](#도메인-네임-시스템) (일명 DNS)

## 액티브 디렉토리 도메인 서비스
**[액티브 디렉토리 도메인 서비스](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)**(Active Directory Domain Service; AD DS)

## 도메인 네임 시스템
> *참고: [Reviewing DNS Concepts | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/reviewing-dns-concepts)*

**[도메인 네임 시스템](https://en.wikipedia.org/wiki/Domain_Name_System)**(domain name system; DNS)은 외우기 어려운 숫자 조합의 [IPv4](TCPIP.md#인터넷-프로토콜) 또는 [IPv6](TCPIP.md#인터넷-프로토콜) 주소를 간단한 [호스트명](https://en.wikipedia.org/wiki/Hostname)으로 접근할 수 있도록 한다. 예를 들어, [`www.example.com`](https://www.example.com/) 호스트명을 DNS에 건네면 *93.184.216.34* (IPv4) 및 *2606:2800:220:1:248:1893:25c8:1946* (IPv6) 주소로 변환한다.

클라이언트 PC는 먼저 시스템에 등록된 로컬 DNS 서버와 통신하여 호스트명의 IP 주소를 요청한다.

* 가정용 인터넷일 경우 [ISP](Network.md#인터넷-서비스-제공자)가 제공하는 DNS가 해당하며, 아래 예시의 168.126.63.1은 서울특별시 종로구에 위치한 KT 통신망의 DNS 서버이다.

```
PS C:\Windows\System32> ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : CONTOSO-CLIENT1
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   DHCP Enabled. . . . . . . . . . . : No
   IPv4 Address. . . . . . . . . . . : 192.168.1.1(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : 192.168.1.10
   DNS Servers . . . . . . . . . . . : 168.126.63.1
```

만일 로컬 DNS 서버가 해당 호스트명에 대해 캐싱된 정보가 없을 경우, 다음 절차에 따라 IP 주소 쿼리를 진행한다.

### 재귀적 네임 쿼리
로컬 DNS 서버는 요청이 들어온 호스트명의 IP 주소를 쿼리하기 위해 [DNS 리졸버](https://en.wikipedia.org/wiki/Name_server#Recursive_Resolver)(DNS resolver) 역할을 맡는다. 아래 예시는 DNS 리졸버 (즉, 로컬 DNS 서버)가 ftp.contoso.com의 IP 주소를 쿼리하는 절차를 보여준다.

![DNS의 인터넷 재귀 네임 쿼리 과정](https://learn.microsoft.com/en-us/windows-server/identity/media/reviewing-dns-concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)

1. 로컬 DNS 서버는 ftp.contoso.com의 IP 주소가 무엇인지 요청을 받는다.
1. 로컬 DNS 서버는 루트 힌트에 저장된 IP 주소로부터 "[루트 서버](https://en.wikipedia.org/wiki/Root_name_server)"와 통신을 시도한다.
1. 루트 서버로부터 ftp.contoso.**com**의 [최상위 도메인](https://en.wikipedia.org/wiki/Top-level_domain)(TLD)에 따라 "Com 서버"로 안내된다.
1. Com 서버로부터 ftp.**contoso.com**의 도메인 이름에 따라 "Contoso 서버"로 안내된다.
1. Contoso 서버로부터 **ftp.contoso.com** 호스트 이름의 IP 주소를 답변 받는다.
1. 로컬 DNS 서버는 IP 주소를 클라이언트에게 회신한다.
