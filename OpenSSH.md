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

<table style="width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">SSH-2의 계층 구조</caption><colgroup><col style="width: 12%;"/><col style="width: 18%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">계층</th><th style="text-align: center;">영문</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://www.ietf.org/rfc/rfc4253.txt">전송 계층</a></td><td style="text-align: center;">Transport layer</td><td>알고리즘 합의 및 키 교환(서버 인증 포함)을 제공하는 계층이다. 이로 인해 결과적으로 구축된 암호화된 통신 연결은 무결성, 기밀성, 그리고 선택적으로 압축성을 제공한다.</td></tr><tr><td style="text-align: center;"><a href="https://www.ietf.org/rfc/rfc4252.txt">사용자 인증 계층</a></td><td style="text-align: center;">User authentication layer</td><td>사용자를 인증하는 몇 가지 방법을 제공한다: 비밀번호, 공개키, 호스트 기반 인증 등이 존재한다. 전송 계층이 제공하는 서비스에 의존해 구축된 통신 연결상에서 운영된다.</td></tr><tr><td style="text-align: center;"><a href="https://www.ietf.org/rfc/rfc4254.txt">연결 계층</a></td><td style="text-align: center;">Connection layer</td><td>인증된 통신 연결상 존재하는 다수의 다양한 채널을 멀티플렉싱, 즉 채널들의 흐름을 제어하는 서비스를 제공한다. 그 외에도 로그인 세션의 <a href="https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling">터널링</a>과 TCP <a href="https://en.wikipedia.org/wiki/Port_forwarding">포워딩</a>을 허용한다.</td></tr></tbody></table>

아래는 SSH-2의 보안 채널이 구축되는 과정을 순서대로 보여준다<sup>[[출처](https://bytebytego.com/guides/guides/how-does-ssh-work/)]</sup>.

![SSH-2의 클라이언트와 서버 간 세션 연결 과정](https://assets.bytebytego.com/diagrams/0224-how-does-ssh-work.png)
