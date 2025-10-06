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

* [KEX 알고리즘](#kex-알고리즘)
* [호스트 키 알고리즘](#호스트-키-알고리즘)
* Ciphers
* MAC

### KEX 알고리즘
**KEX 알고리즘**(key exchange algorithm)은 SSH 클라이언트와 서버 간 암복호화에 필요하나 오로지 그들만 알고 있는 [공유 비밀](https://en.wikipedia.org/wiki/Shared_secret) [대칭 키](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)를 생성하기 위한 암호 키 교환 방법을 정의한다. SSH 알고리즘 합의를 마친 직후에 진행되며, [Diffie-Hellman 키 교환](https://en.wikipedia.org/wiki/Diffie-Hellman_key_exchange) 알고리즘이 대표적이다.

> 본 문서는 Diffie-Hellman (DH) 키 교환 알고리즘이 공유 비밀 키를 어떻게 생성하는지 개념만 간단히 소개한다.

![Diffie-Hellman 키 교환의 개념 및 예시](https://upload.wikimedia.org/wikipedia/commons/c/c8/DiffieHellman.png)

<table style="width: 90%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">SSH-2의 계층 구조</caption><colgroup><col style="width: 7%;"/><col style="width: 23%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">순서</th><th style="text-align: center;">메시지</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">1</a></td><td>SSH_MSG_KEXINIT</td><td>클라이언트와 서버는 암호화 연산에서 공통된 파라미터를 사용할 것을 공개적으로 합의하지만, 각자 무작위로 생성한 자신만의 KEX 개인 키 x, y를 가지고 있다.</td></tr><tr><td style="text-align: center;">2</b></a></td><td>SSH_MSG_KEXDH_INIT</td><td>클라이언트는 KEX 개인 키 x와 공통 파라미터를 조합하여 KEX 공개 키 e를 만들어 다음 항목들을 서버로 전달한다:<ul><li>클라이언트의 KEX 공개 키 e</li></ul></td></tr><tr><td style="text-align: center;">3</td><td>SSH_MSG_KEXDH_REPLY</td><td>서버는 KEX 개인 키 y와 공통 파라미터를 조합하여 KEX 공개 키 f를 만들어 다음 항목들을 클라이언트로 전달한다:<ul><li>서버의 KEX 공개 키 f</li><li>서버의 <a href="#호스트-키-알고리즘">호스트 공개 키</a> 및 인증서</li><li>해시 서명 <sub>(서버는 이미 KEX 공개 키 e와 f를 알고 있고, 심지어 공유 비밀 키 K까지 생성)</sub></li></ul></td></tr></tbody></table>

위의 절차를 통해 SSH 서버와 클라이언트는 각각 자신의 KEX 개인 키 x, y를 노출시키지 않고서도 KEX 공개 키 e, f를 서로 교환하여 공유 비밀 K를 생성할 수 있게 된다. 또한, DH의 비대칭 키는 [핸드셰이킹](https://en.wikipedia.org/wiki/Handshake_(computing)) 이후로 버려지기 때문에 [전반향 안정성](https://en.wikipedia.org/wiki/Forward_secrecy)(forward secrecy)을 보장한다.

### 호스트 키 알고리즘
> *참고: [Key-Based Authentication in OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/windows-server/administration/openssh/openssh_keymanagement)*

**호스트 키 알고리즘**(host key algorithm)은 SSH 통신 연결을 위한 핸드셰이킹 도중에 서버가 스스로 누구인지를 알리는데 활용되는 알고리즘이다. 해당 알고리즘으로 서버는 자신이 소유하는 [비대칭](https://en.wikipedia.org/wiki/Public-key_cryptography) 호스트 개인 키를 사용해 해시를 서명한다. 그리고  SSH_MSG_KEXDH_REPLY 단계로 해시의 서명을 전달받은 클라이언트는 서버의 호스트 공개 키로 SSH 세션이 일관성을 유지하고 있음을 확인한다.

여기서 해시(hash)란, SSH 핸드셰이크에 관여하는 아래 항목들을 조합하여 [일방향](https://en.wikipedia.org/wiki/One-way_function) [해시 함수](https://en.wikipedia.org/wiki/Hash_function)로 변환한 결과물이다.<sup>[[출처](https://www.rfc-editor.org/rfc/rfc4253.html#page-21)]</sup> 즉, 해당 SSH 세션의 고유성을 식별하는 "[지문](https://en.wikipedia.org/wiki/Public_key_fingerprint)(thumbprint)" 역할을 한다:

* 클라이언트 및 서버의 ID 문자열
* 클라이언트 및 서버의 SSH_MSG_KEXINIT [페이로드](https://en.wikipedia.org/wiki/Payload_(computing))
* 호스트 공개 키
* 클라이언트 및 서버의 [KEX 공개 키](#kex-알고리즘)
* [공유 비밀 키](#kex-알고리즘)

예를 들어, KEX 알고리즘으로 diffie-hellman-group14-sha256이 합의되었다면 [SHA-256](https://en.wikipedia.org/wiki/SHA-2)으로 해싱된다. 단, 이는 해시 자체를 생성하는 알고리즘으로, 해시의 서명은 호스트 키 알고리즘에 따라 별도로 지정된다.

[중간자 공격](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) 등으로 서명 검증을 실패하면 세션의 일관성 또한 문제가 있음을 의미하여 SSH 핸드셰이크는 즉시 종료된다. 다시 말해, 해시 서명은 클라이언트가 연결하려는 서버가 *정말로 해당 서버가 맞는지* 진정성을 검증하는 게 아니다. 대신 클라이언트는 신뢰할 수 있는 서버의 공개 키를 known_hosts 파일에서 관리한다.

만일 처음으로 접하는 서버일 경우 [TOFU](https://en.wikipedia.org/wiki/Trust_on_first_use)(즉, 최초 사용 시 신뢰) 모델에 따라 사용자가 연결을 승인할 때 공개 키를 known_host에 저장한다.

```
The authenticity of host Server1 (x.x.x.x)' can't be established.
Are you sure you want to continue connecting (yes/no)?
```

### Ciphers

### MAC

## 사용자 인증 계층
**[사용자 인증 계층](https://www.ietf.org/rfc/rfc4252.txt)**(user authentication layer)

사용자를 인증하는 몇 가지 방법을 제공한다: 비밀번호, 공개키, 호스트 기반 인증 등이 존재한다. 전송 계층이 제공하는 서비스에 의존해 구축된 통신 연결상에서 운영된다.

## 연결 계층
**[연결 계층](https://www.ietf.org/rfc/rfc4254.txt)**(connection layer)

인증된 통신 연결상 존재하는 다수의 다양한 채널을 멀티플렉싱, 즉 채널들의 흐름을 제어하는 서비스를 제공한다. 그 외에도 로그인 세션의 [터널링](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling)과 [TCP 포워딩](https://en.wikipedia.org/wiki/Port_forwarding)을 허용한다.
