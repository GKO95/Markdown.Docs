# 인터넷 프로토콜 스위트
**[인터넷 프로토콜 스위트](https://en.wikipedia.org/wiki/Internet_protocol_suite)**(Internet protocol suite), 일명 **TCP/IP**는 [인터넷](https://en.wikipedia.org/wiki/Internet)을 포함한 [컴퓨터 네트워크](Network.md)에서 정보를 주고받는 데 사용되는 [통신규약](https://en.wikipedia.org/wiki/Communication_protocol)의 모음을 정리한 프레임워크이다. 1970년대부터 1990년대까지 이어진 [프로토콜 전쟁](https://en.wikipedia.org/wiki/Protocol_Wars) 당시 기술적, 상업적 측면에서 우위를 점하여 "[사실상 표준](https://en.wikipedia.org/wiki/De_facto)"이 되었으나, 당시 TCP/IP와 대립하던 [OSI 모형](https://en.wikipedia.org/wiki/OSI_model)(Open System Interconnection model)은 네트워크 개념 및 활동을 설명하는 데 훌륭한 프레임워크로써 IT 분야에서 네트워크 절차를 논의하거나 가르칠 때 반드시 언급된다.

![네트워크 호스트 간 TCP/IP 통신 흐름](https://upload.wikimedia.org/wikipedia/commons/c/c4/IP_stack_connections.svg)

총 네 개의 계층으로 나뉘어져 있으며, 이들은 다음과 같이 나열된다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">TCP/IP 네트워크 계층</caption><colgroup><col style="width: 18%;"/><col style="width: 12%;"/><col/><col style="width: 18%;"/></colgroup><thead><tr><th style="text-align: center;">계층&nbsp;<sub>[번호]</sub></th><th style="text-align: center;"><a href="#프로토콜-데이터-단위">PDU</a></th><th style="text-align: center;">기능</th><th style="text-align: center;">통신규약</th></tr></thead><tbody><tr><td>[4] <a href="#어플리케이션-계층"><b>어플리케이션</b></a></td><td style="text-align: center;">데이터</td><td>어플리케이션 간 데이터 교환 및 상호작용</td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/HTTP">HTTP</a>(<a href="https://en.wikipedia.org/wiki/HTTPS">S</a>), <a href="https://en.wikipedia.org/wiki/Domain_Name_System">DNS</a>, <a href="https://en.wikipedia.org/wiki/Server_Message_Block">SMB</a>, <a href="https://en.wikipedia.org/wiki/Secure_Shell">SSH</a>, <a href="https://en.wikipedia.org/wiki/Telnet">Telnet</a> 등</td></tr><tr><td>[3] <a href="#전송-계층"><b>전송</b></a></td><td style="text-align: center;">데이터그램,<br/>세그먼트</td><td>네트워크 상에서 어플리케이션 간 통신 프로토콜 정의</td><td style="text-align: center;"><a href="#사용자-데이터그램-프로토콜">UDP</a>, <a href="#전송-제어-프로토콜">TCP</a></td></tr><tr><td>[2] <a href="#인터넷-계층"><b>인터넷</b></a></td><td style="text-align: center;"><a href="Network.md#네트워크-패킷">패킷</a></td><td>광범위적인 네트워크 간 (즉, <a href="https://en.wikipedia.org/wiki/Wide_area_network">WAN</a>) 통신 프로토콜 정의</td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/IPv4">IPv4</a>, <a href="https://en.wikipedia.org/wiki/IPv6">IPv6</a></td></tr><tr><td>[1] <a href="#링크-계층"><b>링크</b></a></td><td style="text-align: center;">프레임</td><td>네트워크 세그먼트 안에서 노드 간 (즉, <a href="https://en.wikipedia.org/wiki/Local_area_network">LAN</a>) 통신 프로토콜 정의</td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Address_Resolution_Protocol">ARP</a>, <a href="https://en.wikipedia.org/wiki/Medium_access_control">MAC</a></td></tr></tbody></table>

역사적을 미국 국방부에서 투자한 "전송 제어 프로그램(Transmission Control Program)"로부터 개발이 시작되었다. 초창기에는 [연결지향형](https://en.wikipedia.org/wiki/Connection-oriented_communication)(connection-oriented) [데이터그램 프로토콜](#사용자-데이터그램-프로토콜) 구현이 목표였으나, [비연결형](https://en.wikipedia.org/wiki/Connectionless_communication)(connectionless) 성질의 데이터그램을 연결지향형으로 만드는 건 난해한 과제였다. 결국 이를 해결하기 위해 설계된 통신규약이 바로 *TCP* 그리고 *IP*이다.

그러므로 TCP/IP를 구성하는 세 가지의 핵심 통신규약은 다음과 같다:

1. [사용자 데이터그램 프로토콜](#사용자-데이터그램-프로토콜)(UDP)
1. [전송 제어 프로토콜](#전송-제어-프로토콜)(TCP)
1. [인터넷 프로토콜](#인터넷-프로토콜)(IP)

### 프로토콜 데이터 단위
**[프로토콜 데이터 단위](https://en.wikipedia.org/wiki/Protocol_data_unit)**(protocol data unit; PDU)는 네트워크를 통해 다른 노드로 전송되는 단일 정보 단위이며, 프로토콜마다 정의된 제어 정보와 사용자 데이터로 구성되어 있다. 다시 말해, PDU는 해당 프로토콜에서 정보를 전송할 때 취급하는 데이터 유형을 가리킨다.

![TCP/IP 모델을 예시로 보여주는 계층간 PDU 관계](https://upload.wikimedia.org/wikipedia/commons/3/3b/UDP_encapsulation.svg)

일반적으로 PDU는 [헤더](https://en.wikipedia.org/wiki/Header_(computing))와 [페이로드](https://en.wikipedia.org/wiki/Payload_(computing))로 구성된다: 전자는 PDU의 출발지 및 목적지 등의 [메타데이터](https://en.wikipedia.org/wiki/Metadata)를 포함하며, 후자는 상위 계층에서 활용될 PDU 혹은 데이터를 캡슐화한다. 위의 [TCP/IP](TCPIP.md) 모델을 예시로 들어, 어플리케이션 계층의 HTTP 프로토콜 데이터는 전송 계층의 UDP 프로토콜의 페이로드에 포함된다. 이러한 페이로드의 캡슐화는 국지적인 로컬 네트워크 프로토콜을 공용 프로토콜에 숨기므로써 네트워크 장벽을 너머 수신되어서도 활용할 수 있도록 한다.

## 어플리케이션 계층
**[어플리케이션 계층](https://en.wikipedia.org/wiki/Application_layer)**(application layer)은 파일 공유, 메시지 처리, 데이터베이스 접근 등 프로세스 간 직접적인 통신 및 상호작용이 이루어지는 최상단 계층이다. 어플리케이션의 역할 및 목적에 따라 다양한 통신규약이 있으며, 아래는 일부를 간략히 나열한다.

* [HTTP](https://en.wikipedia.org/wiki/HTTP): 웹 브라우저에서 World Wide Web(일명 WWW)의 웹사이트를 표현하는 프로토콜
    * [HTTPS](https://en.wikipedia.org/wiki/HTTPS): [TLS/SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)로 암호화된 보안 통신을 제공하는 HTTP의 확장판

### SMB
**[서버 메시지 블록](https://en.wikipedia.org/wiki/Server_Message_Block)**(server message block; SMB)

### RPC
**[원격 프로시저 호출](https://en.wikipedia.org/wiki/Remote_procedure_call)**(remote procedure call; RPC)

## 전송 계층
**[전송 계층](https://en.wikipedia.org/wiki/Transport_layer)**(transport layer)은 전달받은 데이터가 노드 내에서 이를 요청한 프로세스에게 전송될 수 있도록 가담하는 계층이다. 출발지 호스트의 한 어플리케이션에서 목적지 호스트의 다른 어플리케이션으로 일련의 데이터들을 전달하는 기능 및 절차적 방법을 제공하는데, 대표적인 두 가지의 전송 프로토콜은 다음과 같다:

<table style="width: 60%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">전송 계층의 프로토콜 유형</caption><colgroup><col style="width: 20%;"/><col style="width: 20%;"/><col/></colgroup><thead><tr><th style="text-align: center;">전송 프로토콜</th><th style="text-align: center;">PDU</th><th style="text-align: center;">개요</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="#사용자-데이터그램-프로토콜">UDP</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Datagram">데이터그램</a></td><td>빠르고 간단하지만 목적지 도달을 보장하지 못하는 <a href="https://en.wikipedia.org/wiki/Connectionless_communication">비연결형</a>(connectionless) </td></tr><tr><td style="text-align: center;"><a href="#전송-제어-프로토콜">TCP</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure">세그먼트</a></td><td>구조는 복잡하지만 안정적인 통신을 중시하는 <a href="https://en.wikipedia.org/wiki/Connection-oriented_communication">연결지향형</a>(connection-oriented)</td></tr></tbody></table>

* 다중화(multiplexing): 송신하려는 각 포트의 데이터그램 및 세그먼트를 모두 거두어 하나의 패킷 페이로드로 캡슐화하는 작업이다.
* 역다중화(demultiplexing): 수신받은 패킷으로부터 각 데이터그램과 세그먼트를 올바른 포트로 전달하는 작업니다.

세그먼트와 데이터그램은 (IP 주소와 [포트](https://en.wikipedia.org/wiki/Port_(computer_networking)) 번호 조합의) [네트워크 소켓](https://en.wikipedia.org/wiki/Network_socket)이란 완전한 통신 주소를 기반해 찾아간다.

> 포트(port)는 각 전송 프로토콜마다 통신 연결의 종착점을 고유 식별하고 데이터를 특정 서비스로 유도하기 위해 할당되는 양의 16비트 범위의 번호이다.

네트워크 소켓은 데이터를 송신 및 수신하기 위한 종착점(endpoint) 역할을 하는 노드 내의 소프트웨어 구조이다. 소켓의 구조와 성질은 네트워킹 아키텍처의 API에 의해 정의되며, 오로지 실행 중인 어플리케이션의 프로세스 생명주기 동안에만 유효하다.

### 사용자 데이터그램 프로토콜
**[사용자 데이터그램 프로토콜](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**(User Datagram Protocol; UDP)은 TCP/IP에서 사용하는 핵심 전송 프로토콜 중 하나로 [데이터그램](https://en.wikipedia.org/wiki/Datagram)(datagram)을 취급한다.

데이터그램이란, 이전 교환에 의존하지 않고서 출발지로부터 목적지까지 도달할 수 있는 충분한 데이터를 지닌 자립적이고 독립적인 존재이다. 데이터그램은 매우 간단한 구조를 지니며 헤더에는 출발지 및 목적지 소켓, 데이터그램 크기, 그리고 체크섬 정보가 전부이다. 너무 간단한 나머지, UDP 전송 프로토콜은 아래와 같은 성질을 지닌다:

* 비신뢰성(unreliable): 데이터그램이 목적지에 도달한다는 보장이 없고, 또한 목적지에 도달하였다는 것을 확인할 방도가 없다.
* 비연결형(connectionless): 데이터그램은 전송 경로를 확립되지 않은 채로 송신된다.

오히려 UDP의 간단한 구조는 오류 검증 및 정정을 처리하는 비중이 적어 속도가 빠른 장점을 지니므로, 화상통화 또는 스트리밍 방송 등의 실시간 서비스 시스템에서 선호된다.

### 전송 제어 프로토콜
**[전송 제어 프로토콜](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)**(Transmission Control Protocol; TCP)은 TCP/IP에서 사용하는 핵심 전송 프로토콜 중 하나로 [세그먼트](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure)(segment)를 취급한다.

세그먼트란, 사전적으로 "파편"을 의미하며 전달하려는 어플리케이션 계층의 데이터를 여러 조각으로 쪼개어 별도 패킷으로 전송할 수 있다. TCP 전송 프로토콜은 노드 간 통신 연결 확립을 보장하는 연결지향형(connection-oriented)으로써 본래 IP 설계 목적을 보완한다. 그러기 위해 세그먼트 헤더에는 출발지 및 목적지 소켓 외에도 시퀀스 번호, 플래그, 체크섬 등 다양한 정보를 포함한다.

클라이언트의 어플리케이션이 서버와 TCP 통신을 시도할 시, 3방향 [핸드셰이킹](https://en.wikipedia.org/wiki/Handshake_(computing))을 절차를 통해 확실한 통신 연결을 보장한다.

![TCP 통신 연결을 위한 3방향 핸드셰이킹](https://upload.wikimedia.org/wikipedia/commons/9/98/Tcp-handshake.svg)

<table style="width: 75%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">TCP 연결 핸드셰이킹 절차</caption><colgroup><col style="width: 8%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">단계</th><th style="text-align: center;">송신 노드</th><th style="text-align: center;">수신 노드</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td style="text-align: center;">클라이언트</td><td style="text-align: center;">서버</td><td><code>SYN</code> 플래그가 설정된 페이로드 없는 bodyless 세그먼트를 전송한다.</td></tr><tr><td style="text-align: center;">2</td><td style="text-align: center;">서버</td><td style="text-align: center;">클라이언트</td><td><code>SYN | ACK</code> 플래그가 설정된 bodyless 세그먼트로 응답한다.</td></tr><tr><td style="text-align: center;">3</td><td style="text-align: center;">클라이언트</td><td style="text-align: center;">서버</td><td><code>ACK</code> 플래그가 설정된 bodyless 세그먼트로 응답한다.</td></tr></tbody></table>

세그먼트의 각 헤더에 저장된 `Sequence #` 시퀀스 번호로부터 원본 데이터를 구성하는 몇 번째 조각인지 식별이 가능하다. 세그먼트를 수신받은 노드는 시퀀스 번호에 대응하는 `Acknowledgement #`로 응답하는데, 이들을 취합하여 전송한 모든 세그먼트가 목적지에 도달하였는지 판단한다. 위의 3방향 핸드셰이킹을 완료하여 TCP 연결이 구축되면, 본격적으로 [어플리케이션 계층](#어플리케이션-계층)의 데이터 전송이 이루어진다.

데이터 전송을 완료하여 더 이상의 통신이 필요없을 시, 클라이언트 혹은 서버에서 4방향 핸드셰이킹을 개시하여 구축된 TCP 연결을 종료한다. 때문에 아래 다이어그램은 "클라이언트-서버"가 아닌 "발신자-수신자" 관계로 소개한다.

![TCP 통신 종료를 위한 4방향 핸드셰이킹](https://upload.wikimedia.org/wikipedia/commons/5/55/TCP_CLOSE.svg)

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">TCP 종료 핸드셰이킹 절차</caption><colgroup><col style="width: 8%;"/><col style="width: 15%;"/><col style="width: 15%;"/><col/></colgroup><thead><tr><th style="text-align: center;">단계</th><th style="text-align: center;">송신 노드</th><th style="text-align: center;">수신 노드</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</td><td style="text-align: center;">발신자</td><td style="text-align: center;">수신자</td><td><code>FIN</code> 플래그가 설정된 bodyless 세그먼트로 TCP 종료 요청을 알린다.</td></tr><tr><td style="text-align: center;">2</td><td style="text-align: center;">수신자</td><td style="text-align: center;">발신자</td><td><code>ACK</code> 플래그가 설정된 bodyless 세그먼트로 발신자의 <code>FIN</code>에 응답한다.</td></tr><tr><td style="text-align: center;">3</td><td style="text-align: center;">수신자</td><td style="text-align: center;">발신자</td><td><code>FIN</code> 플래그가 설정된 bodyless 세그먼트로 수신자도 (더 이상 전송할 데이터가 없을 시) TCP 종료 준비가 되었음을 알린다.</td></tr><tr><td style="text-align: center;">4</td><td style="text-align: center;">발신자</td><td style="text-align: center;">수신자</td><td><code>ACK</code> 플래그가 설정된 bodyless 세그먼트로 수신자의 <code>FIN</code>에 응답한다.</td></tr></tbody></table>

이러한 4방향 핸드셰이킹을 마쳤으면 수신자 측에서 발신자의 요청에 따른 TCP 연결을 종료한다.

## 인터넷 계층
**[인터넷 계층](https://en.wikipedia.org/wiki/Internet_layer)**(internet layer)은 데이터가 도달해야 할 네트워크로 전송될 수 있도록 관여하는 계층이다. 해당 계층의 PDU인 [패킷](Network.md#네트워크-패킷)은 [광역 통신망](https://en.wikipedia.org/wiki/Wide_area_network)(wide area network; WAN)을 연결하는 [라우터](https://en.wikipedia.org/wiki/Router_(computing))를 통해 목적지까지 점차적으로 [라우팅](https://en.wikipedia.org/wiki/Routing)되어 도달하는 데, 이러한 과정을 [패킷 포워딩](https://en.wikipedia.org/wiki/Packet_forwarding)이라 부른다.

> 대표적인 WAN으로 우리가 흔히 알고 있는 [인터넷](https://en.wikipedia.org/wiki/Internet)(Internet)이 이에 해당하며, 이를 가능케 한 게 바로 [인터넷 프로토콜](#인터넷-프로토콜)이다.

### 인터넷 프로토콜
[인터넷 프로토콜](https://en.wikipedia.org/wiki/Internet_Protocol)(Internet Protocol; IP)은 전송 계층에서 소개된 [세그먼트](#전송-제어-프로토콜) 혹은 [데이터그램](#사용자-데이터그램-프로토콜)의 [인터네트워크 통신](https://en.wikipedia.org/wiki/Internetworking)을 가능케 하도록 패킷 구조를 정의하는 네트워크 계층의 대표적인 프로토콜이다. 가장 널리 사용되는 인터넷 프로토콜은 아래와 같이 두 가지 버전이 존재한다.

* [IPv4](https://en.wikipedia.org/wiki/IPv4): 32비트 주소 (2011년 2월 3일에 IPv4 주소 전부 소진)
* [IPv6](https://en.wikipedia.org/wiki/IPv6): 128비트 주소

패킷의 헤더에는 출발지 및 목적지 주소가 들어있고, 그 외에도 전송 계층의 프로토콜 유형과 [TTL](https://en.wikipedia.org/wiki/Time_to_live) (IPv4 한정) 정보도 포함되어 있다. TTL은 "time to live"의 약자로, 라우터를 통해 다음 네트워크 세그먼트로 넘어가는 [홉](https://en.wikipedia.org/wiki/Hop_(networking))(hop) 횟수를 제한시켜 목적지에 도달하지 못할 시 해당 패킷이 소실되어 통신 대역을 낭비하는 것을 방지한다. IPv6에서는 홉 제한(hop limit)이라고 부른다.

아래는 가상의 CONTOSO-CLIENT1 컴퓨터에 [ipconfig](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig) 명령으로 확보한 IP 정보 예제이다.

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

## 링크 계층
**[링크 계층](https://en.wikipedia.org/wiki/Link_layer)**(link layer)은 [네트워크 세그먼트](https://en.wikipedia.org/wiki/Network_segment) 안에서 데이터가 도달해야 할 노드로 전송될 수 있도록 관여하는 계층이다.

> [데이터 링크](https://en.wikipedia.org/wiki/Data_link)(data link)란, 디지털 정보의 송수신을 위한 지역 간 통신 연결을 가리킨다.

일반적으로 [근거리 통신망](https://en.wikipedia.org/wiki/Local_area_network)(local area network; LAN)에 국한되며, 해당 계층의 PDU인 [프레임](https://en.wikipedia.org/wiki/Frame_(networking))(frame)은 헤더에 저장된 [네트워크 연결 장치](https://en.wikipedia.org/wiki/Network_interface_controller)마다 할당된 (출발지 및 목적지) [MAC 주소](https://ko.wikipedia.org/wiki/MAC_주소) 정보로부터 도달해야 하는 노드로 찾아갈 수 있다. 하지만 하드웨어가 변경되면 MAC 주소도 바뀌고, 타 네트워크에 있는 장치들은 동일한 MAC 주소를 가질 수 있으므로 네트워크 계층에서는 MAC 주소로 목적지를 찾지 않는다.
