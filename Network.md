# 네트워크
**[네트워크](https://en.wikipedia.org/wiki/Computer_network)**(network)는 [네트워크 장치](#네트워크-노드)가 제공하는, 또는 장치에 상주하는 리소스를 공유하는 컴퓨터의 집합이다. 컴퓨터는 [유선](https://en.wikipedia.org/wiki/Computer_network#Wired), [무선](https://en.wikipedia.org/wiki/Wireless_network), 혹은 [광섬유](https://en.wikipedia.org/wiki/Fiber-optic_communication)로 구축된 망에서 흔히 사용되는 [통신 프로토콜](https://en.wikipedia.org/wiki/Communication_protocol)을 기반으로 데이터를 주고 받는다. [인터넷](https://en.wikipedia.org/wiki/Internet)에서 웹사이트를 접근하거나 공용 스토리지 서버 및 프린터 공유, 그리고 이메일과 메신저 등을 주고 받을 수 있도록 하는 어플리케이션이나 서비스를 지원한다.

* [인터넷](https://en.wikipedia.org/wiki/Internet)(Internet): *여러 네트워크를 상호연결하는 [인터네트워킹](https://en.wikipedia.org/wiki/Internetworking)(internetworking)의 약어이자 하나의 대명사가 되어버린 대표적인 네트워크이다.*

## 네트워크 패킷
> *참고: [What is packet? - Cloudflare.com](https://www.cloudflare.com/learning/network-layer/what-is-a-packet/) & [What are Network Packets and How Do They Work? - TechTarget.com](https://www.techtarget.com/searchnetworking/definition/packet)*

**[네트워크 패킷](https://en.wikipedia.org/wiki/Network_packet)**(network packet), 간단히 **패킷**(packet)은 한 컴퓨터에서 다른 컴퓨터로 네트워크를 통해 전달하려는 데이터를 조각으로 나눈 것이다.

데이터를 여러 패킷으로 나누면 전송 효율성과 신뢰성 향상에 기여할 수 있다. 데이터 전송에 사용 중인 [통신 채널](https://ko.wikipedia.org/wiki/채널_(통신))(동축 케이블, 광케이블, 라디오, 적외선 등) 경로는 해당 데이터가 완전히 전송될 때까지 다른 데이터가 이동할 수 없다. 그러므로 데이터를 패킷으로 작게 나누어 다양한 경로의 채널로 이동하는 [패킷 교환](https://ko.wikipedia.org/wiki/패킷_교환)(packet switching) 기법을 활용하면 통신 혼잡을 방지할 수 있다. 덕분에 네트워크에 연결된 하나의 컴퓨터가 다수의 다른 컴퓨터랑 동시다발적 통신이 가능하다.

![호스트 간에 네트워크를 거친 패킷 교환](https://upload.wikimedia.org/wikipedia/commons/f/f6/Packet_Switching.gif)

### 네트워크 트래픽
> *참고: [What is Network Traffic? Definition, Types & Management Techopedia](https://www.techopedia.com/definition/29917/network-traffic)*

**[네트워크 트래픽](https://en.wikipedia.org/wiki/Network_traffic)**(network traffic)은 주어진 시간에 네트워크를 통해 움직이는 데이터의 양을 가리킨다. 데이터는 대체로 [네트워크 패킷](#네트워크-패킷)에 감싸여 이동하며, 이는 네트워크에 부하를 가한다. 비정상적인 트래픽 용량은 공격 징후를 시사할 수 있기 때문에, 적절한 네트워크 트래픽 분석은 네트워크 보안에 이점을 제공한다.

### 방화벽
**[방화벽](https://en.wikipedia.org/wiki/Firewall_(computing))**(firewall)은 정해진 규칙에 따라 송수신 네트워크 트래픽을 모니터링 및 제어하는 네트워크 보안 시스템이다. 일반적으로 신뢰할 수 있는 네트워크 그렇지 못한 네트워크 간 장벽을 세워 통신을 차단한다. 물리적인 장치에 의한 *하드웨어 방화벽*, 그리고 시스템 내에서 동작하는 *소프트웨어 방화벽* (예를 들어, [윈도우 방화벽](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/) 등)으로 나뉘어진다.

## 네트워크 노드
**[네트워크 노드](https://en.wikipedia.org/wiki/Node_(networking))**(network node)는 네트워크를 통해 전달되는 패킷의 재분배점 혹은 [통신 도착점](https://en.wikipedia.org/wiki/Communication_endpoint)(communication endpoint)이다. 물리적 네트워크 노드는 둘 중 하나로 분류된다:

<table style="table-layout: fixed; width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">데이터 단말 및 통신 장치 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Data_terminal_equipment">데이터 단말 장치</a> (data terminal equipment; DTE)</th><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Data_circuit-terminating_equipment">데이터 통신 장치</a> (data communication equipment; DCE)</th></tr></thead><tbody style="text-align: center;"><tr><td>정보를 네트워크 통신 신호로, 혹은 그 반대로 변환하는 장치</td><td>DTE 사이에 위치하여 신호 변환, 오류 정정, 클락 등을 제공하는 장치</td></tr><tr><td>예시: 휴대전화, 프린터, <a href="#네트워크-호스트">호스트 컴퓨터</a> 등</td><td>예시: 모뎀, 허브, 브리지, 스위치 등</td></tr></tbody></table>

컴퓨터 네트워크가 LAN 혹은 WAN일 경우, 데이터 전송에 임하는 모든 노드들은 반드시 네트워크 주소를 가져야 한다 (일반적으로 각 [NIC](https://en.wikipedia.org/wiki/Network_interface_controller)마다 한 개의 주소 할당). 인터넷 혹은 인트라넷일 경우, 대다수의 물리적 네트워크 노드는 IP 주소로 식별되는 호스트 컴퓨터이다.

### 네트워크 호스트
**[네트워크 호스트](https://en.wikipedia.org/wiki/Host_(network))**(network host)는 네트워크에 연결된 컴퓨터 혹은 기타 장치를 가리킨다. 호스트는 네트워크 사용자 혹은 타 호스트에 정보 리소스, 서비스, 또는 어플리케이션을 제공하는 [서버](https://en.wikipedia.org/wiki/Server_(computing))(server)의 역할을 맡을 수 있다. 그리고 서버에서 제공하는 리소스 및 서비스에 접근하는 호스트를 [클라이언트](https://en.wikipedia.org/wiki/Client_(computing))(client)라고 부른다. 네트워크에 연결된 모든 장치들을 가리키는 노드 중에서, 사용자 어플리케이션에 관여하는 서버 혹은 클라이언트가 호스트이다.

## 인터넷 서비스 제공자
**[인터넷 서비스 제공자](https://en.wikipedia.org/wiki/Internet_service_provider)**(internet service provider; ISP)
