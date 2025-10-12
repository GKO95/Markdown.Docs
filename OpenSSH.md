# OpenSSH
> 비록 OpenSSH는 [크로스 플랫폼](https://en.wikipedia.org/wiki/Cross-platform_software) 소프트웨어이지만, 본 문서는 [Windows](Windows.md) 운영체제에 최적화된 [*OpenSSH for Windows*](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview)를 위주로 기술을 설명한다.

**[OpenSSH](https://en.wikipedia.org/wiki/OpenSSH)**<sub>([공식 사이트](https://www.openssh.com/))</sub>는 클라이언트와 서버 간 보안이 취약한 통신망에서도 암호화된 채널을 구축하는 [SSH](https://en.wikipedia.org/wiki/Secure_Shell) [프로토콜](https://en.wikipedia.org/wiki/Communication_protocol) 기반의 대표적인 보안 네트워킹 스위트이다. 흔히 원격 [로그인](Logon.md) 및 명령어 실행, 파일 전송 등에 활용된다. [Telnet](https://en.wikipedia.org/wiki/Telnet) 및 [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol) 같은 비암호화 프로토콜을 대체하고자 [자유 소프트웨어](https://en.wikipedia.org/wiki/Free_software)로 처음 공개된 SSH가 차후 [사유화](https://en.wikipedia.org/wiki/Proprietary_software)되어, 이를 [오픈 소스](https://en.wikipedia.org/wiki/Open-source_software)로 공개하려는 개발자들에 의해 제작된 게 바로 OpenSSH이다.

[윈도우 OS](Windows.md)는 빌드 1809 "[Redstone 5](https://en.wikipedia.org/wiki/Windows_10,_version_1809)"부터 본격적으로 지원하기 시작하여 [리눅스](https://en.wikipedia.org/wiki/Linux)와 원격 접속이 가능해졌다. OpenSSH 클라이언트는 기본적으로 설치되어 있는 반면, OpenSSH 서버의 경우 Windows Server 2022까지는 [FOD](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)를 통해 개별 설치가 필요하다.

* `ssh`: 사용자의 로컬 시스템에 실행되는 SSH 클라이언트 컴포넌트이다.
* `sshd`: 원격으로 관리될 시스템에서 반드시 실행되어야 할 SSH 서버 컴포넌트이다.
* `ssh-keygen`: SSH 인증 키를 생성, 관리, 변환을 담당한다.
* `ssh-agent`: [공개 키 인증](Certificate.md)에 사용될 개인 키를 저장한다.
* `ssh-add`: SSH 서버가 허용한 개인 키를 에이전트에 추가한다.

### 보안 파일 전송 프로토콜
**[보안 파일 전송 프로토콜](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)**(Secure File Transfer Protocol; SFTP)은 보안이 취약한 단순 FTP를 대체하기 위해 SSH의 보안 채널 위에서 동작하는 파일 전송 프로토콜이다. 단, SFTP는 단순히 FTP를 SSH 위에서 실행하는 게 아닌 새로 설계된 프로토콜이다.

* `sftp`: SFTP를 제공하는 서비스이다.
* `scp`: SSH의 파일 복사 유틸리티이다.

## 아키텍처
본 문서는 2006년에 발표된 개정된 SSH 프로토콜인 **SSH-2**를 기준으로 아키텍처와 보안 채널 구축 과정을 소개한다. 비록 SSH-1과 호환되지 않으나 향상된 보안과 추가된 기능으로 가장 널리 사용되는 프로토콜이기 때문에, OpenSSH는 7.6 버전부터 SSH-2만을 지원하고 있다.

SSH 프로토콜은 세 가지의 구성 요소의 계층 구조를 이룬다.

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">SSH-2의 계층 구조</caption><colgroup><col style="width: 20%;"/><col style="width: 15%;"/><col style="width: 65%;"/></colgroup><thead><tr><th style="text-align: center;">계층<sup>†</sup></th><th style="text-align: center;"><a href="TCPIP.md">TCPIP</a> 해당</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td>[3] <a href="#연결-계층"><b>연결 계층</b></a></td><td style="text-align: center;"><a href="TCPIP.md#어플리케이션-계층">Application</a></td><td>인증된 클라이언트가 요청한 서비스마다 채널을 생성하여 작업을 수행한다.</td></tr><tr><td>[2] <a href="#사용자-인증-계층"><b>사용자 인증 계층</b></a></td><td style="text-align: center;"><a href="TCPIP.md#어플리케이션-계층">Application</a></td><td>구축된 암호화된 구축 채널에서 클라이언트가 서버로부터 인증을 시도한다.</td></tr><tr><td>[1] <a href="#전송-계층"><b>전송 계층</b></a></td><td style="text-align: center;"><a href="TCPIP.md#전송-계층">Transport</a></td><td><a href="TCPIP.md#전송-제어-프로토콜">TCP</a> <a href="https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports">포트 22</a>를 통해 클라이언트와 서버 간 암호화된 통신 구축을 마련한다.</td></tr></tbody></table>

<sup>_† SSH 아키텍처를 규정하는 RFC에는 [OSI](https://en.wikipedia.org/wiki/OSI_model) 및 [TCPIP](TCPIP.md) 모델처럼 계층에 번호를 부여하지 않으며, 위의 도표는 단순히 이해를 돕기 위한 차원이다._</sup>

아래는 SSH-2의 보안 채널이 구축되는 과정을 순서대로 보여준다.<sup>[[출처](https://bytebytego.com/guides/guides/how-does-ssh-work/)]</sup>

![SSH-2의 클라이언트와 서버 간 세션 연결 과정](https://assets.bytebytego.com/diagrams/0224-how-does-ssh-work.png)

## 전송 계층
**[전송 계층](https://www.ietf.org/rfc/rfc4253.txt)**(transport layer)은 클라이언트와 서버 간 SSH 통신을 위한 암호화된 채널을 구축하는 계층이다. 실질적인 SSH 통신이 이루어지기 전에 알고리즘 합의 및 키 교환 과정을 거쳐 연결의 무결성, 기밀성, 그리고 압축성을 제공한다. 대부분 [TCP](TCPIP.md#전송-제어-프로토콜)를 활용하며, 2001년에 [IANA](https://en.wikipedia.org/wiki/Internet_Assigned_Numbers_Authority)가 [포트](https://en.wikipedia.org/wiki/Port_(computer_networking)) 22를 SSH로 표준화하였다.

우선 클라이언트와 서버는 SSH 프로토콜 및 소프트웨어 버전이 기입된 ID 문자열을 교환하여 호환성을 검사한다. 이후 SSH_MSG_KEX_INIT 메시지의 전송을 시작으로 다음 네 가지 항목에 대하여 클라이언트가 선호하는 알고리즘을 서버에서 합의가 가능한지 여부를 살펴본다:

* [KEX 알고리즘](#키-교환)
* [호스트 키 알고리즘](#키-교환)
* Cipher
* MAC

### 키 교환
**키 교환**(key exchange), 일명 **KEX**는 전송 계층에서 이루어지는 SSH 핸드셰이크 과정이며, SSH_MSG_KEXINIT 메시지를 시작으로 다음 두 가지 중요한 작업을 수행한다.

1. [공유 비밀](https://en.wikipedia.org/wiki/Shared_secret) [대칭 키](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) 생성: *SSH 통신의 암복호화에 사용될, 하지만 오로지 클라이언트와 서버만 아는 비밀을 만든다.*
1. 해시 생성 및 서명 검증: *SSH 터널의 고유성을 식별 및 일관성을 검증할 장치를 마련한다.*

SSH 알고리즘 합의를 마친 직후에 진행되며, 본 문서는 대표적인 KEX 알고리즘인 [Diffie-Hellman 키 교환](https://en.wikipedia.org/wiki/Diffie-Hellman_key_exchange) 개념만 간단히 소개한다.

![Diffie-Hellman 키 교환의 개념 및 예시](https://upload.wikimedia.org/wikipedia/commons/c/c8/DiffieHellman.png)

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Deffie-Hellman (DH) 알고리즘을 활용한 KEX 과정</caption><colgroup><col style="width: 7%;"/><col style="width: 23%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">메시지</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</a></td><td>SSH_MSG_KEXINIT</td><td>클라이언트와 서버는 암호화 연산에서 공통된 파라미터를 사용할 것을 공개적으로 합의하지만, 각자 무작위로 생성한 자신만의 KEX 개인 키 x, y를 가지고 있다.</td></tr><tr><td style="text-align: center;">2</b></a></td><td>SSH_MSG_KEXDH_INIT</td><td>클라이언트는 KEX 개인 키 x와 공통 파라미터를 조합하여 KEX 공개 키 e를 만들어 다음 항목들을 서버로 전달한다:<ul><li>클라이언트의 KEX 공개 키 e</li></ul></td></tr><tr><td style="text-align: center;">3</td><td>SSH_MSG_KEXDH_REPLY</td><td>서버는 KEX 개인 키 y와 공통 파라미터를 조합하여 KEX 공개 키 f를 만들어 다음 항목들을 클라이언트로 전달한다:<ul><li>서버의 KEX 공개 키 f</li><li>서버의 호스트 공개 키 및 인증서</li><li>해시 서명 <sub>(서버는 이미 KEX 공개 키 e와 f를 알고 있고, 심지어 공유 비밀 키 K까지 생성)</sub></li></ul></td></tr><tr><td style="text-align: center;">4</td><td>-</td><td>클라이언트와 서버는 도출된 공유 비밀 키 K 및 교환 해시 H로 SSH 터널에서 본격적으로 사용될 세 쌍(클라이언트 방향 & 서버 방향)의 대칭 파생 키를 생성한다:<sup>[<a href="https://www.rfc-editor.org/rfc/rfc4253.html#section-7.2">참고</a>]</sup><ul><li>초기화 벡터 x2</li><li>암호화 키 x2 <sub>(Cipher에서 지정된 알고리즘 사용)</sub></li><li>무결성 키 x2 <sub>(MAC에서 지정된 알고리즘 사용)</sub></li></ul></td></tr><tr><td style="text-align: center;">5</td><td>SSH_MSG_NEWKEYS</td><td>키 교환 과정이 마무리되고, 이후 SSH 터널의 모든 암복호화는 오로지 새로 파생된 여섯 개의 암호키만을 사용한다.</td></tr></tbody></table>

여기서 교환 해시(exchange hash; 일명 해시)란, SSH [핸드셰이크](https://en.wikipedia.org/wiki/Handshake_(computing))에 관여하는 아래 항목들을 조합하여 [일방향](https://en.wikipedia.org/wiki/One-way_function) [해시 함수](https://en.wikipedia.org/wiki/Hash_function)로 변환한 결과물이다.<sup>[[출처](https://www.rfc-editor.org/rfc/rfc4253.html#section-8)]</sup> 즉, 해당 SSH 터널의 고유성을 식별하는 "[지문](https://en.wikipedia.org/wiki/Public_key_fingerprint)(thumbprint)" 역할을 한다:

* 클라이언트 및 서버의 ID 문자열
* 클라이언트 및 서버의 SSH_MSG_KEXINIT [페이로드](https://en.wikipedia.org/wiki/Payload_(computing))
* 호스트 공개 키
* 클라이언트 및 서버의 KEX 공개 키 e, f
* 공유 비밀 키 K

예를 들어, KEX 알고리즘으로 diffie-hellman-group14-sha256이 합의되었다면 [SHA-256](https://en.wikipedia.org/wiki/SHA-2)으로 해싱된다.

SSH 프로토콜은 각 세션마다 식별할 수 있는 고유의 해시를 활용하여 연결이 일관되는지 확인하는 매커니즘을 제시한다. 다시 말해, 클라이언트가 SSH 핸드셰이킹하는 서버가 변치 않았음을 검증이 병행된다. 만일 [중간자 공격](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) 등의 개입으로 본래 세션을 맺으려는 서버가 달라진 게 확인되면 SSH 핸드셰이크는 즉시 종료된다.

> 클라이언트는 신뢰할 수 있는 서버의 공개 키를 known_hosts 파일에서 관리하며, 처음 접하는 서버의 경우 [TOFU](https://en.wikipedia.org/wiki/Trust_on_first_use) 모델을 따른다.

서버는 [비대칭](https://en.wikipedia.org/wiki/Public-key_cryptography) SSH 호스트 암호키를 생성하며 이는 KEX 알고리즘이 아닌 "호스트 키 알고리즘"이 사용된다:

* SSH 서버가 해시 H로부터 서명을 생성할 *호스트 개인 키*
* SSH 클라이언트가 해시 H의 서명을 검증할 수 있는 *호스트 공개 키*

비록 공유 비밀 키 K가 생성되어도 클라이언트와 서버 모두 공유하고 있어, 만일 한 곳에서 공격을 받았다면 SSH 전체가 보안상 위험에 취해진다. 이를 방징하기 위해 공유 비밀 키 K는 교환 해시 H와 KEX 알고리즘에 따라 함께 조합하여 새로운 대칭 암호키를 파생하되, 클라이언트와 서버가 사용할 암호키를 나누어 생성한다.

### 서비스 요청
SSH_MSG_NEWKEYS 메시지를 시작으로 암호화된 SSH 터널의 [전송 계층](#전송-계층) 위에는 다양한 작업, 일명 **서비스**(service)를 수행할 수 있도록 확정성을 제공한다. SSH 서비스는 SSH_MSG_SERVICE_REQUEST 메시지로 요청된다. 자체 제작이 가능하지만, 다음 두 명칭은 이미 아래 목적으로 예약되어 있다.<sup>[[출처](https://www.rfc-editor.org/rfc/rfc4253#section-10)]</sup>

1. [`ssh-userauth`](#사용자-인증-계층): 클라이언트(사용자)를 서버에 인증한다.
1. [`ssh-connection`](#연결-계층): SSH 터널이 다양한 채널을 관리할 수 있도록 구성한다.

만일 서버가 서비스 요청을 거절한다면 적절한 SSH_MSG_DISCONNECT 메시지와 함께 SSH 터널을 반드시 종료해야 한다. 반면, 서비스를 지원할 시 SSH_MSG_SERVICE_ACCEPT 메시지로 응답한다.

## 사용자 인증 계층
**[사용자 인증 계층](https://www.ietf.org/rfc/rfc4252.txt)**(user authentication layer)은 서버로 접속하려는 클라이언트가 인증된 사용자인지 확인하는 계층이다. 일반적으로 SSH 터널의 암호화가 본격화된 다음 곧바로 사용자 인증이 진행된다. 사용자 인증 방식에는 대표적으로 세 가지가 있다:

* [비밀번호](https://en.wikipedia.org/wiki/Password)
* [사용자 공개 키](https://learn.microsoft.com/windows-server/administration/openssh/openssh_keymanagement): 서버에 저장된 클라이언트의 공개 키를 활용해 개인 키의 서명을 검증하여 인증한다.
* [호스트 기반](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Host-based_Authentication): 서버에 저장된 신뢰할 수 있는 클라이언트만이 비대칭 암호키 검증으로 인증될 수 있다.

서버가 지원하는 인증 방식 목록을 전달받은 클라이언트는 그 중 자유롭게 선택하여 시도할 수 있다.

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">SSH 인증 메시지 코드</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">메시지</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td>SSH_MSG_USERAUTH_REQUEST</td><td>SSH 인증은 반드시 해당 메시지로 요청되어야 하며, 인증 방식과 성공 시 수행할 서비스를 포함한다.</td></tr><tr><td>SSH_MSG_USERAUTH_FAILURE</td><td>서버가 인증을 거절할 때 전송된다.</td></tr><tr><td>SSH_MSG_USERAUTH_SUCCESS</td><td>서버가 인증을 수락할 때 전송되며, 단 한번만 발송되어야 하고 이후 인증 요청은 가급적 무시되어야 한다.</td></tr></tbody></table>

## 연결 계층
**[연결 계층](https://www.ietf.org/rfc/rfc4254.txt)**(connection layer)은 사용자 인증을 마친 암호화된 SSH 터널에서 다양한 채널을 [멀티플렉싱](https://en.wikipedia.org/wiki/Multiplexing)하여 관리하는 계층이다. SSH에서 기본적으로 제공하는 채널 유형은 다음과 같이 존재한다.

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">SSH 채널의 유형 및 설명</caption><colgroup><col style="width: 15%;"/><col style="width: 25%;"><col style="width: 60%;"/></colgroup><thead><tr><th colspan="2" style="text-align: center;">채널</th></th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">세션</td><td><code>session</code></td><td><a href="Shell.md">셸</a>, <a href="https://en.wikipedia.org/wiki/Application_software">어플리케이션</a>, 명령, 혹은 내장 <a href="Windows.md#환경-서브시스템">서브시스템</a> 등의 프로그램 원격 실행</td></tr><tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/X_Window_System">X11</a></td><td><code>x11</code></td><td>원격에서 실행 중인 <a href="https://en.wikipedia.org/wiki/Graphical_user_interface">GUI</a> 어플리케이션의 창을 로컬 시스템에 표시<sup>[<a href="https://en.wikipedia.org/wiki/X_Window_System#Remote_desktop">참고</a>]</sup></td></tr><tr><td rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Port_forwarding">TCP/IP 포워딩</a><sup>†</sup></td><td><code>direct-tcpip</code></td><td>[원격 → 로컬] 타 사용자가 서버 포트 R 접근 시, SSH 서버에서 클라이언트의 포트 L로 연결하도록 안내</td></tr><tr><td><code>forwarded-tcpip</code></td><td>[로컬 → 원격] 클라이언트가 로컬 포트 L 접근 시, SSH 클라이언트에서 서버의 포트 R로 연결하도록 안내</td></tr></tbody></table>

<sup>_† TCP/IP 포워딩은 방화벽 및 신뢰할 수 없는 네트워크에서 트래픽을 암호화된 SSH의 포트 22로 안내하여 서비스 접근 허용하기 위해 사용된다._</sup>

채널에 대한 요청은 크게 두 가지로 나뉘어진다.

* [전역 요청](https://www.rfc-editor.org/rfc/rfc4254.html#section-4)(global request): 채널과 무관하게 원격 단말 전반에 영향을 주는 요청이며, TCP/IP 포워딩이 이에 해당한다.
* [채널별 요청](https://www.rfc-editor.org/rfc/rfc4254.html#section-5.4)(channel-specific request): 주어진 채널 내에서만 영향을 미치는 요청이며, 세션과 X11이 이에 해당한다.
