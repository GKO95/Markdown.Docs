# 액티브 디렉터리
**[액티브 디렉토리](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)**(Active Directory)

# 도메인

## 도메인 이름 시스템
**[도메인 이름 시스템](https://en.wikipedia.org/wiki/Domain_Name_System)**(domain name system; DNS)은 외우기 어려운 숫자 조합의 [IPv4](TCPIP.md#인터넷-프로토콜) 또는 [IPv6](TCPIP.md#인터넷-프로토콜) 주소를 간단한 [호스트명](https://en.wikipedia.org/wiki/Hostname)으로 접근할 수 있도록 한다. 예를 들어, [`www.example.com`](https://www.example.com/) 호스트명을 DNS에 건네면 *93.184.216.34* (IPv4) 및 *2606:2800:220:1:248:1893:25c8:1946* (IPv6) 주소로 변환한다.

클라이언트 PC는 먼저 시스템에 등록된 로컬 DNS 서버와 통신하여 www.example.com의 IP 주소를 요청한다.

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

만일 로컬 DNS 서버가 www.example.com에 대한 정보가 없을 경우, 다음 절차에 따라 IP 주소 쿼리를 진행한다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">DNS의 인터넷 쿼리 과정</caption><colgroup><col style="width: 6%;"/><col style="width: 19%;"/><col/></colgroup><thead><tr><th style="text-align: center;">단계</th><th style="text-align: center;">쿼리 DNS</th><th style="text-align: center;">설명</th></tr></thead>

<tbody><tr><td style="text-align: center;">1</td><td style="text-align: left;"><a href="https://en.wikipedia.org/wiki/Root_name_server)">루트 네임 서버</a></td><td>전 세계적으로 총 13개만 해당하는 이들은 <a href="https://en.wikipedia.org/wiki/DNS_root_zone">DNS 루트 존</a>에서 인터넷의 <a href="#최상위-도메인">TLD</a> 쿼리 권한을 가진 모든 DNS 서버의 목록을 갖는다. 위의 www.example.com 예시에서, 루트 DNS 서버는 `.com`에 속한 도메인들을 쿼리할 DNS로 안내한다.</td></tr><tr><td style="text-align: center;">2</td><td style="text-align: left;"><a href="#최상위-도메인">최상위 도메인 네임 서버</a></td><td>특정 TLD 안에 속한 도메인들의 이름 정보를 관리한다. 위의 www.example.com 예시에서, 최상위 도메인 DNS 서버는 `example.com` 도메인을 쿼리할 DNS로 안내한다.</td></tr><tr><td style="text-align: center;">3</td><td style="text-align: left;"><a href="https://en.wikipedia.org/wiki/Name_server#Authoritative_name_server">권한 있는 DNS 서버</a></td><td>특정 도메인 안에 속한 리소스들의 이름 정보를 관리한다. 해당 쿼리 단계에서는 대체로 도메인 이름을 소유하는 기업이나 조직에서 관리하는 DNS 서버가 전담한다. 위의 www.example.com 예시에서, 루트 DNS 서버는 `www.example.com`에 속한 도메인들을 쿼리할 DNS로 안내한다.</td></tr></tbody></table>

### 최상위 도메인
> *참고: [List of Internet top-level domains - Wikipedia](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains)*

**[최상위 도메인](https://en.wikipedia.org/wiki/Top-level_domain)**(top-level domain; TLD)은 [도메인 이름](Network.md#도메인-이름)의 최상위 계층에 속하는 도메인이다. 대표적인 일반 TLD로 [.com](https://en.wikipedia.org/wiki/.com), [.org](https://en.wikipedia.org/wiki/.org), [.net](https://en.wikipedia.org/wiki/.net) 등이 해당한다.

### DNS 리졸버
일명 DNS 클라이언트라고 불리며, [윈도우 NT](Windows.md)의 경우 dnscache [서비스](Service.md)가 해당 역할을 담당한다.
