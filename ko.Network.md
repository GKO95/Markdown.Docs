---
category: 윈도우
title: 네트워크
alias: network
visible: true
---
# 네트워크
[네트워크](https://ko.wikipedia.org/wiki/컴퓨터_네트워크)(network)는 [노드](#노드)에 상주하거나 제공된 리소스를 공유하는 컴퓨터의 집합이다. 컴퓨터는 유선, 무선, 그리고 광섬유로 구축된 망에서 흔히 사용되는 통신 프로토콜을 기반으로 데이터를 주고 받는다. 인터넷에서 웹사이트를 접근하거나 공용 스토리지 서버 및 프린터 공유, 그리고 이메일과 메신저 등을 주고 받을 수 있도록 어플리케이션이나 서비스를 지원한다.

여기서 [인터넷](https://ko.wikipedia.org/wiki/인터넷)(Internet)이란, 여러 네트워크를 상호연결하는 [인터네트워킹](https://ko.wikipedia.org/wiki/인터네트워킹)(internetworking)의 줄임말이자 하나의 대명사가 되어버린 대표적인 네트워크이다.

## 네트워크 패킷
> *참고: [What is packet? - Cloudflare.com](https://www.cloudflare.com/learning/network-layer/what-is-a-packet/) & [What are Network Packets and How Do They Work? - TechTarget.com](https://www.techtarget.com/searchnetworking/definition/packet)*

[네트워크 패킷](https://ko.wikipedia.org/wiki/네트워크_패킷)(network packet), 간단히 패킷(packet)은 한 컴퓨터에서 다른 컴퓨터로 네트워크를 통해 전달하려는 데이터를 조각으로 나눈 것이다.

데이터를 여러 패킷으로 나누면 전송 효율성과 신뢰성 향상에 기여할 수 있다. 데이터 전송에 사용 중인 [통신 채널](https://ko.wikipedia.org/wiki/채널_(통신))(동축 케이블, 광케이블, 라디오, 적외선 등) 경로는 해당 데이터가 완전히 전송될 때까지 다른 데이터가 이동할 수 없다. 그러므로 데이터를 패킷으로 작게 나누어 다양한 경로의 채널로 이동하는 [패킷 교환](https://ko.wikipedia.org/wiki/패킷_교환)(packet switching) 기법을 활용하면 통신 혼잡을 방지할 수 있다. 덕분에 네트워크에 연결된 하나의 컴퓨터가 다수의 다른 컴퓨터랑 동시다발적 통신이 가능하다.

![호스트 간에 네트워크를 거친 패킷 교환<sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:Packet_Switching.gif">위키미디어</a></i></sub>](./images/network_packet_switching.gif)

## 네트워크 노드
[네트워크 노드](https://ko.wikipedia.org/wiki/노드_(네트워크))(network node)는 네트워크를 통해 전달되는 패킷의 재분배점 혹은 [통신 도착점](https://en.wikipedia.org/wiki/Communication_endpoint)(communication endpoint)이다. 물리적 네트워크 노드는 둘 중 하나로 분류된다:

<table style="table-layout: fixed; width: 80%; margin: auto;">
<caption style="caption-side: top;">데이터 단말 및 통신 장치 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/데이터_단말_장치">데이터 단말 장치</a> (data terminal equipment; DTE)</th><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/데이터_회선_종단_장치">데이터 통신 장치</a> (data communication equipment; DCE)</th></tr></thead>
<tbody style="text-align: center;"><tr><td>정보를 네트워크 통신 신호로, 혹은 그 반대로 변환하는 장치</td><td>DTE 사이에 위치하여 신호 변환, 오류 정정, 클락 등을 제공하는 장치</td></tr>
<tr><td>예시: 휴대전화, 프린터, <a href="#네트워크-호스트">호스트 컴퓨터</a> 등</td><td>예시: 모뎀, 허브, 브리지, 스위치 등</td></tr></tbody>
</table>

컴퓨터 네트워크가 LAN 혹은 WAN일 경우, 데이터 전송에 임하는 모든 노드들은 반드시 네트워크 주소를 가져야 한다 (일반적으로 각 [NIC](https://ko.wikipedia.org/wiki/네트워크_인터페이스_컨트롤러)마다 한 개의 주소 할당). 인터넷 혹은 인트라넷일 경우, 대다수의 물리적 네트워크 노드는 IP 주소로 식별되는 호스트 컴퓨터이다.

### 네트워크 호스트
[네트워크 호스트](https://ko.wikipedia.org/wiki/호스트_(네트워크))(network host)는 네트워크에 연결된 컴퓨터 혹은 기타 장치를 가리킨다. 호스트는 네트워크 사용자 혹은 타 호스트에 정보 리소스, 서비스, 또는 어플리케이션을 제공하는 [서버](https://ko.wikipedia.org/wiki/서버)(server)의 역할을 맡을 수 있다. 그리고 서버에서 제공하는 리소스 및 서비스에 접근하는 호스트를 [클라이언트](https://ko.wikipedia.org/wiki/클라이언트_(컴퓨팅))(client)라고 부른다. 네트워크에 연결된 모든 장치들을 가리키는 노드 중에서, 사용자 어플리케이션에 관여하는 서버 혹은 클라이언트가 호스트이다.

# OSI 모형
> *참고: [What is the OSI Model? - Cloudflare.com](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)*

[OSI 모형](https://ko.wikipedia.org/wiki/OSI_모형)(Open System Interconnection Reference Model)은 시스템 상호연결에 대한 공통 기반을 제공하기 위해 [국제 표준화 기구](https://ko.wikipedia.org/wiki/국제_표준화_기구)(International Organization for Standard; ISO)에서 고안한 개념적 모형이다. 네트워크 개념 및 활동을 설명하는데 훌륭한 프레임워크로써 현재까지도 OSI 모형은 IT 분야에서 네트워크 절차를 논의하거나 가르칠 때 반드시 언급된다. [ISO 7489-1](https://www.iso.org/standard/20269.html) 표준으로 지정되어 있다.

총 일곱 개의 추상 계층으로 나뉘어져 있으며, 이들은 다음과 같이 나열된다.

![OSI 모형: 계층 번호가 작을수록 네트워크 통신망과 가깝다. <sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:Osi_model_trad.jpg">위키미디어</a></i></sub>](./images/network_osi_model.jpg)

<table style="width: 80%; margin: auto;">
<caption style="caption-side: top;">OSI 모형 네트워크 계층</caption>
<colgroup><col style="width: 2%;"/><col style="width: 10%;"/><col style="width: 10%;"/><col/><col style="width: 10%;"/></colgroup>
<thead><tr><th colspan="2" style="text-align: center;">계층</th><th style="text-align: center;"><a href="#프로토콜-데이터-단위">PDU</a></th><th style="text-align: center;">기능</th><th style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/통신_프로토콜">프로토콜</a></th></tr></thead>
<tbody>
<tr><td style="text-align: center;">7</td><td><a href="#응용-계층">응용</a></td><td style="text-align: center;" rowspan="3">데이터</td><td>어플리케이션 간 데이터 교환 및 상호작용</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/HTTP">HTTP</a>, <a href="https://ko.wikipedia.org/wiki/파일_전송_프로토콜">FTP</a></td></tr>
<tr><td style="text-align: center;">6</td><td><a href="#표현-계층">표현</a></td><td>어플리케이션 간 전달되는 데이터의 서식화, 압축, 그리고 암호화 및 복호화</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/ASCII">ASCII</a></td></tr>
<tr><td style="text-align: center;">5</td><td><a href="#세션-계층">세션</a></td><td>어플리케이션 간 통신에 필요한 세션을 구축 및 관리</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/원격_프로시저_호출">RPC</a></td></tr>
<tr><td style="text-align: center;">4</td><td><a href="#전송-계층">전송</a></td><td style="text-align: center;">데이터그램, 세그먼트</td><td>네트워크 상에서 어플리케이션 간 통신 프로토콜 정의</td><td style="text-align: center;"><a href="#사용자-데이터그램-프로토콜">UDP</a>, <a href="#전송-제어-프로토콜">TCP</a></td></tr>
<tr><td style="text-align: center;">3</td><td><a href="#네트워크-계층">네트워크</a></td><td style="text-align: center;"><a href="#네트워크-패킷">패킷</a></td><td><a href="https://ko.wikipedia.org/wiki/광역_통신망">WAN</a>과 같은 광범위적인 네트워크 간 통신 프로토콜 정의</td><td style="text-align: center;"><a href="#인터넷-프로토콜">IP</a> (<a href="https://ko.wikipedia.org/wiki/IPv4">v4</a>, <a href="https://ko.wikipedia.org/wiki/IPv6">v6</a>)</td></tr>
<tr><td style="text-align: center;">2</td><td><a href="#데이터-링크-계층">데이터 링크</a></td><td style="text-align: center;">프레임</td><td><a href="https://ko.wikipedia.org/wiki/근거리_통신망">LAN</a>과 같은 네트워크 세그먼트 안에서 노드 간 통신 프로토콜 정의</td><td style="text-align: center;"><a href="#매체-접근-제어">MAC</a></td></tr>
<tr><td style="text-align: center;">1</td><td><a href="#물리-계층">물리</a></td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/비트_(단위)">비트</a>, <a href="https://en.wikipedia.org/wiki/Symbol_rate#Symbols">심볼</a></td><td>네트워크 상에서 노드 간 데이터 전송 규격 제공</td><td style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/IEEE_802.3">IEEE 802.3</a>, <a href="https://ko.wikipedia.org/wiki/블루투스">블루투스</a></td></tr>
</tbody>
</table>

### 프로토콜 데이터 단위
[프로토콜 데이터 단위](https://ko.wikipedia.org/wiki/프로토콜_데이터_단위)(protocol data unit; PDU)는 네트워크를 통해 다른 노드로 전송되는 단일 정보 단위이며, 프로토콜마다 정의된 제어 정보와 사용자 데이터로 구성되어 있다. 다시 말해, PDU는 해당 프로토콜에서 정보를 전송할 때 취급하는 데이터 유형을 가리킨다.

일반적으로 PDU는 [헤더](https://ko.wikipedia.org/wiki/헤더_(컴퓨팅))와 [페이로드](https://ko.wikipedia.org/wiki/페이로드_(컴퓨팅))로 구성된다: 전자는 PDU의 출발지 및 목적지 등의 [메타데이터](https://ko.wikipedia.org/wiki/메타데이터)를 포함하며, 후자는 상위 계층에서 활용될 PDU 혹은 데이터를 캡슐화한다 (예를 들어 데이터 링크 계층의 프레임 페이로드는 네트워크 계층의 패킷을 포함). 특히 페이로드의 캡슐화는 국지적인 로컬 네트워크 프로토콜을 공용 프로토콜에 숨기므로써 네트워크 장벽을 너머 수신되어서도 활용할 수 있도록 한다.

## 응용 계층
[응용 계층](https://ko.wikipedia.org/wiki/응용_계층)(application layer)은 파일 공유, 메시지 처리, 그리고 데이터베이스 접근 등 [프로세스](ko.Process.md) 간 직접적인 통신 및 상호작용이 이루어지는 최상단의 7번째 계층이다.

## 표현 계층
[표현 계층](https://ko.wikipedia.org/wiki/표현_계층)(presentation layer)은 응용 계층에서 전달하려는 데이터의 서식화, 암호화 및 복호화를 처리하는 6번째 계층이다. 복잡한 구조의 [객체](ko.Cpp.md#클래스) 등의 데이터들을 전송하는 방안으로 ([TLV](https://ko.wikipedia.org/wiki/형식-길이-값), [XML](https://ko.wikipedia.org/wiki/XML), 또는 [JSON](https://ko.wikipedia.org/wiki/JSON) 등의 매커니즘을 활용하여) 1차원적인 바이트 문자열로 [직렬화](https://ko.wikipedia.org/wiki/직렬화)하는 데, 이러한 개념을 기반하여 송신 및 수신 프로세스에서 데이터 변환 및 독해가 이루어진다.

## 세션 계층
[세션 계층](https://ko.wikipedia.org/wiki/세션_계층)(session layer)은 프로세스 간 통신을 위한 [세션](https://ko.wikipedia.org/wiki/세션_(컴퓨터_과학))(session)을 구축 및 관리하는 5번째 계열이다.

> 세션은 제한 시간이 지정된 쌍방향 링크이며, 특정 시점에서 연결이 수립된 이후로 시간이 지나면 종료하도록 되어있다.

TCP/IP에서 세션 계층은 두 개 이상의 통신 장치 또는 네트워크 말단(컴퓨터나 자동화 시스템, 혹은  활동 중인 사용자) 간의 상호 표현 및 정보 교환을 가능케 한다.

## 전송 계층
[전송 계층](https://ko.wikipedia.org/wiki/전송_계층)(transport layer)은 전달받은 데이터가 노드 내에서 이를 요청한 프로세스에게 전송될 수 있도록 가담하는 4번째 계층이다. 출발지 호스트의 한 어플리케이션에서 목적지 호스트의 다른 어플리케이션으로 일련의 데이터들을 전달하는 기능 및 절차적 방법을 제공하는 데, 대표적인 두 가지의 전송 프로토콜은 다음과 같다:

<table style="width: 60%; margin: auto;">
<caption style="caption-side: top;">전송 계층의 프로토콜 유형</caption>
<colgroup><col style="width: 20%;"/><col style="width: 20%;"/><col/></colgroup>
<thead><tr><th style="text-align: center;">전송 프로토콜</th><th style="text-align: center;">PDU</th><th style="text-align: center;">개요</th></tr></thead>
<tbody><tr><td style="text-align: center;"><a href="#사용자-데이터그램-프로토콜">UDP</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Datagram">데이터그램</a></td><td>빠르고 간단하지만 목적지 도달을 보장하지 못하는 <a href="https://en.wikipedia.org/wiki/Connectionless_communication">비연결형</a>(connectionless) </td></tr>
<tr><td style="text-align: center;"><a href="#전송-제어-프로토콜">TCP</a></td><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure">세그먼트</a></td><td>구조는 복잡하지만 안정적인 통신을 중시하는 <a href="https://en.wikipedia.org/wiki/Connection-oriented_communication">연결지향형</a>(connection-oriented)</td></tr></tbody>
</table>

* 다중화(multiplexing): 송신하려는 각 포트의 데이터그램 및 세그먼트를 모두 거두어 하나의 패킷 페이로드로 캡슐화하는 작업이다.
* 역다중화(demultiplexing): 수신받은 패킷으로부터 각 데이터그램과 세그먼트를 올바른 포트로 전달하는 작업니다.

세그먼트와 데이터그램은 IP 주소와 포트 번호 조합의 [네트워크 소켓](https://ko.wikipedia.org/wiki/네트워크_소켓)(network socket)이란 완전한 통신 주소를 기반해 찾아간다.

> 네트워크 소켓은 데이터를 송신 및 수신하기 위한 종착점(endpoint) 역할을 하는 노드 내의 소프트웨어 구조이다. 소켓의 구조와 성질은 네트워킹 아키텍처의 API에 의해 정의되며, 오로지 실행 중인 어플리케이션의 프로세스 생명주기 동안에만 유효하다.

여기서 [포트](https://ko.wikipedia.org/wiki/포트_(컴퓨터_네트워킹))(port)란, 각 전송 프로토콜마다 통신 연결의 종착점을 고유 식별하고 데이터를 특정 서비스로 유도하기 위해 할당되는 [unsigned](https://ko.wikipedia.org/wiki/Signed와_unsigned#unsigned) 16비트 범위의 번호이다. 허나, 1024 미만의 포트 번호들은 특정 서비스를 위해 이미 예약되어 있는데, 이는 도착한 패킷을 곧바로 프로세스로 넘겨 사용할 수 있도록 하는 편리를 위한 일환이다.

### 사용자 데이터그램 프로토콜
[사용자 데이터그램 프로토콜](https://ko.wikipedia.org/wiki/사용자_데이터그램_프로토콜)(User Datagram Protocol; UDP)은 인터넷 프로토콜 스위트에서 사용하는 핵심 전송 프로토콜 중 하나로 [데이터그램](https://en.wikipedia.org/wiki/Datagram)(datagram)을 취급한다.

> 데이터그램이란, 이전 교환에 의존하지 않고서 출발지로부터 목적지까지 도달할 수 있는 충분한 데이터를 지닌 자립적이고 독립적인 존재이다.

데이터그램은 매우 간단한 구조를 지니며 헤더에는 출발지 및 목적지 소켓, 데이터그램 크기, 그리고 체크섬 정보가 전부이다. 너무 간단한 나머지, UDP 전송 프로토콜은 아래와 같은 성질을 지닌다:

* 비신뢰성(unreliable): 데이터그램이 목적지에 도달한다는 보장이 없고, 또한 목적지에 도달하였다는 것을 확인할 방도가 없다.
* 비연결형(connectionless): 데이터그램은 전송 경로를 확립되지 않은 채로 송신된다.

오히려 UDP의 간단한 구조는 오류 검증 및 정정을 처리하는 비중이 적어 속도가 빠른 장점을 지니므로, 화상통화 또는 스트리밍 방송 등의 실시간 서비스 시스템에서 선호된다.

### 전송 제어 프로토콜
[전송 제어 프로토콜](https://ko.wikipedia.org/wiki/전송_제어_프로토콜)(Transmission Control Protocol; TCP)은 인터넷 프로토콜 스위트에서 사용하는 핵심 전송 프로토콜 중 하나로 [세그먼트](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure)(segment)를 취급한다.

> 세그먼트란, 사전적으로 "파편"을 의미하며 전달하려는 응용 계층의 데이터를 여러 조각으로 쪼개어 별도 패킷으로 전송할 수 있기 때문이다.

UDP와 달리, TCP 전송 프로토콜은 노드 간 통신 연결 확립을 보장하는 연결지향형(connection-oriented)으로써 본래 IP 설계 목적을 보완한다. 그러기 위해 세그먼트 헤더에는 출발지 및 목적지 소켓 외에도 시퀀스 번호, 플래그, 체크섬 등 다양한 정보를 포함한다.

TCP의 연결지향형을 구현하기 위한 일환으로, 만일 클라이언트가 서버와의 연결을 시도한다면 다음 세 단계의 [핸드셰이킹](https://ko.wikipedia.org/wiki/핸드셰이킹)(handshaking) 절차를 거친다:

<table style="width: 60%; margin: auto;">
<caption style="caption-side: top;">TCP 핸드셰이킹 절차</caption>
<colgroup><col style="width: 10%;"/><col style="width: 20%;"/><col style="width: 20%;"/><col/></colgroup>
<thead><tr><th style="text-align: center;">단계</th><th style="text-align: center;">송신 노드</th><th style="text-align: center;">수신 노드</th><th style="text-align: center;">진행</th></tr></thead>
<tbody><tr><td style="text-align: center;">1</td><td style="text-align: center;">클라이언트</td><td style="text-align: center;">서버</td><td><code>SYN</code> 플래그가 설정된 페이로드 없는 bodyless 세그먼트를 전송한다.</td></tr>
<tr><td style="text-align: center;">2</td><td style="text-align: center;">서버</td><td style="text-align: center;">클라이언트</td><td><code>SYN | ACK</code> 플래그가 설정된 bodyless 세그먼트로 응답한다.</td></tr>
<tr><td style="text-align: center;">3</td><td style="text-align: center;">클라이언트</td><td style="text-align: center;">서버</td><td><code>ACK</code> 플래그가 설정된 bodyless 세그먼트로 응답한다.</td></tr></tbody>
</table>

세그먼트의 각 헤더에 저장된 `Sequence #` 시퀀스 번호로부터 원본 데이터를 구성하는 몇 번째 조각인지 식별이 가능하다. 세그먼트를 수신받은 노드는 시퀀스 번호에 대응하는 `Acknowledgement #`로 응답하는데, 이들을 취합하여 전송한 모든 세그먼트가 목적지에 도달하였는지 판단한다. 모든 세그먼트가 목적지에 도달한 게 확인되면 `FIN` 플래그로 치환한 TCP 핸드셰이크 절차를 진행한다.

## 네트워크 계층
[네트워크 계층](https://ko.wikipedia.org/wiki/네트워크_계층)(network layer)은 데이터가 도달해야 할 네트워크로 전송될 수 있도록 관여하는 3번째 계층이다. 해당 계층의 PDU인 패킷(packet)은 [광역 통신망](https://ko.wikipedia.org/wiki/광역_통신망)(wide area network; WAN) 연결에 도맡는 [라우터](https://ko.wikipedia.org/wiki/라우터)(router)에 의해 [라우팅](https://ko.wikipedia.org/wiki/라우팅)되어 목적지로 점차 도달하는 데, 이러한 과정을 [패킷 포워딩](https://en.wikipedia.org/wiki/Packet_forwarding)이라 부른다.

> 대표적인 WAN으로 우리가 흔히 알고 있는 [인터넷](https://ko.wikipedia.org/wiki/인터넷)(Internet)이 이에 해당하며, 이는 [인터넷 프로토콜](#인터넷-프로토콜)(IP)이 핵심이다.

패킷의 헤더에는 프레임과 유사하게 출발지 및 목적지 주소가 들어있고, 그 외에도 전송 계층의 프로토콜 유형과 [TTL](https://ko.wikipedia.org/wiki/Time_to_live) (IPv4 한정) 정보도 포함되어 있다. TTL은 "time to live"의 약자로, 라우터를 통해 다음 네트워크 세그먼트로 넘어가는 [홉](https://ko.wikipedia.org/wiki/홉_(네트워크))(hop) 횟수를 제한시켜 목적지에 도달하지 못할 시 해당 패킷이 소실되어 통신 대역을 낭비하는 것을 방지한다. IPv6에서는 홉 제한(hop limit)이라고 부른다.

### 인터넷 프로토콜
[인터넷 프로토콜](https://ko.wikipedia.org/wiki/인터넷_프로토콜)(Internet Protocol; IP)은 전송 계층에서 소개된 [세그먼트](#전송-제어-프로토콜) 혹은 [데이터그램](#사용자-데이터그램-프로토콜)의 [인터네트워크 통신](https://ko.wikipedia.org/wiki/인터네트워킹)을 가능케 하도록 패킷 구조를 정의하는 네트워크 계층의 대표적인 프로토콜이다.

> 네트워크에서 자주 언급되는 TCP/IP, 일명 [인터넷 프로토콜 스위트](https://ko.wikipedia.org/wiki/인터넷_프로토콜_스위트)(Internet protocol suite)는 [전송 제어 프로토콜](#전송-제어-프로토콜)(TCP)과 인터넷 프로토콜(IP)의 조합을 가리킨다.

![IP의 네트워크 계층 패킷 구조<sub><i>출처: <a href="https://commons.wikimedia.org/wiki/File:UDP_encapsulation.svg">위키미디어</a></i></sub>](./images/network_udp_encapsulation.svg)

간단한 역사를 설명하자면, 인터넷 프로토콜 스위트의 개발은 미국 국방부에서 투자한 "전송 제어 프로그램(Transmission Control Program)"로부터 시작되었으며, 초창기에는 [연결지향형](https://en.wikipedia.org/wiki/Connection-oriented_communication)(connection-oriented) [데이터그램 프로토콜](#사용자-데이터그램-프로토콜) 구현이 목표였다. 그러나 [비연결형](https://en.wikipedia.org/wiki/Connectionless_communication)(connectionless) 성질의 데이터그램을 연결지향형으로 만드는 건 난해한 과제였으며, 결국 이를 지원하는 [새로운 프로토콜](#전송-제어-프로토콜)이 설계되었다.

가장 널리 사용되는 인터넷 프로토콜은 아래와 같이 두 가지 버전이 존재한다.

* [IPv4](https://ko.wikipedia.org/wiki/IPv4): 32비트 주소 (2011년 2월 3일에 IPv4 주소 전부 소진)
* [IPv6](https://ko.wikipedia.org/wiki/IPv6): 128비트 주소

## 데이터 링크 계층
[데이터 링크 계층](https://ko.wikipedia.org/wiki/데이터_링크_계층)(data link layer)은 [네트워크 세그먼트](https://en.wikipedia.org/wiki/Network_segment) 안에서 데이터가 도달해야 할 노드로 전송될 수 있도록 관여하는 2번째 계층이다.

> [데이터 링크](https://ko.wikipedia.org/wiki/데이터_링크)(data link)란, 디지털 정보의 송수신을 위한 지역 간 통신 연결을 가리킨다.

일반적으로 [근거리 통신망](https://ko.wikipedia.org/wiki/근거리_통신망)(local area network; LAN)에 국한되며, 해당 계층의 PDU인 [프레임](https://en.wikipedia.org/wiki/Frame_(networking))(frame)은 헤더에 저장된 [네트워크 연결 장치](https://ko.wikipedia.org/wiki/네트워크_인터페이스_컨트롤러)마다 할당된 (출발지 및 목적지) [MAC 주소](https://ko.wikipedia.org/wiki/MAC_주소) 정보로부터 도달해야 하는 노드로 찾아갈 수 있다. 하지만 하드웨어가 변경되면 MAC 주소도 바뀌고, 타 네트워크에 있는 장치들은 동일한 MAC 주소를 가질 수 있으므로 네트워크 계층에서는 MAC 주소로 목적지를 찾지 않는다.

프레임의 푸터는 [프레임 검사 절차](https://en.wikipedia.org/wiki/Frame_check_sequence)(frame check sequence; FCS) 목적으로 활용되어, 데이터 손실이나 간섭으로 인한 손상을 [체크섬](https://ko.wikipedia.org/wiki/체크섬)(checksum)으로 검사가 가능하다. 만일 프레임을 수신받은 NIC에서 계산된 체크섬이 FCS와 일치하지 않으면 손상된 PDU로 간주하여 버려진다.

### 네트워크 브리지
[네트워크 브리지](https://ko.wikipedia.org/wiki/네트워크_브리지)(network bridge)는 다수의 네트워크 세그먼트를 하나의 네트워크로 만드는 징검다리(bridge) 역할을 하며, 데이터 링크 계층에서 작업을 수행한다.

![OSI 모형으로 나타낸 2-포트 심플 브리지 다이어그램](./images/network_bridge_simple.png)

위의 그림과 같은 두 개의 LAN을 연결하는 네트워크 브리지가 있다고 가정한다: 브리지는 프레임의 목적지 MAC 주소를 읽는데, 만일 목적지 호스트가 다른 네트워크 세그먼트에 있다면 프레임을 그쪽으로 넘겨준다. 허나, 목적지 호스트가 동일 네트워크 세그먼트에 있으면 다른 네트워크 세그먼트로 넘어가는 것을 방지한다.

브리지가 구현된 하드웨어 장치 중 하나가 바로 [네트워크 스위치](https://ko.wikipedia.org/wiki/네트워크_스위치)(network switch)이다. 네트워크 계층의 패킷까지 처리할 수 있는 스위치는 [다계층 스위치](https://en.wikipedia.org/wiki/Multilayer_switch)(multilayer switch)이라고 부른다.

> 반면, 유사한 역할의 이더넷 허브는 수신 호스트를 분별할 수 없는 관계로 모든 포트로 패킷을 재전송하는 매우 간단한 구조로 네트워크 효율이 떨어진다.

## 물리 계층
[물리 계층](https://ko.wikipedia.org/wiki/물리_계층)(physical layer)은 데이터 링크를 통해 비가공된 일련의 비트들, 일명 [비트스트림](https://ko.wikipedia.org/wiki/비트스트림_(컴퓨터_과학))(bitstream)을 전송하는 방법을 규정하는 최하단의 1번째 계층이다. 비트스트림은 [워드](https://ko.wikipedia.org/wiki/워드_(컴퓨팅))나 심볼을 나타내며, 동축 및 광케이블 등의 통신 매질을 거쳐 전송될 물리적 신호로 변환된다.

아래는 물리 계층 연결에 밀접한 연관성을 갖는 하드웨어 장치들 중에서 쉽게 접할 수 있는 일부를 나열한다.

* [네트워크 인터페이스 컨트롤러](https://ko.wikipedia.org/wiki/네트워크_인터페이스_컨트롤러)(network interface controller; NIC): 컴퓨터를 네트워크에 연결시키는 하드웨어이다. 대부분의 메인보드는 NIC가 탑재되어 있다.
* [이더넷 허브](https://ko.wikipedia.org/wiki/이더넷_허브): 여러 [이더넷](https://ko.wikipedia.org/wiki/이더넷) 장치들을 연결하여 자신만의 [작은 네트워크](https://en.wikipedia.org/wiki/Network_segment)로 구축하는 하드웨어이다.
* [공유기](https://ko.wikipedia.org/wiki/라우터)
