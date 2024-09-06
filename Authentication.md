# 인증
> *참고: [Windows Authentication Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)*

**[인증](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)**(authentication)은 시스템에서 식별된 사용자가 실제로 본인이 맞는지 검증하는 절차이다: [비밀번호](https://learn.microsoft.com/en-us/windows-server/security/kerberos/passwords-overview), [스마트카드](https://en.wikipedia.org/wiki/Smart_card), [생체인증](https://en.wikipedia.org/wiki/Biometrics) 등이 해당한다. 네트워크 리소스를 비인가된 사용자로부터 접근을 원천 차단하며, 인가된 사용자는 원격에서도 안전하게 접근할 수 있는 보안을 제공한다.

* 한편, 유사한 용어인 [authorization](https://en.wikipedia.org/wiki/Authorization)은 인증이 완료된 사용자가 해당 리소스를 접근할 "권한"이 있는지 확인하는 것으로 별개의 절차임을 유의하도록 한다.

## 인증 프로토콜
**[인증 프로토콜](https://en.wikipedia.org/wiki/Authentication_protocol)**(authentication protocol)은 두 개체 간 인증 데이터를 전송하기 위해 설계된 [통신 프로토콜](https://en.wikipedia.org/wiki/Communication_protocol)이다.

### 커버로스
> *참고: [Kerberos Authentication Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview)*

**[커버로스](https://en.wikipedia.org/wiki/Kerberos_(protocol))**(Kerberos)는 [상호인증](https://en.wikipedia.org/wiki/Mutual_authentication)에 사용되는 인증 프로토콜 중 하나이다. 비밀번호를 노출시키지 않고 시한부 티켓을 사용하여, [네트워크 도청](https://en.wikipedia.org/wiki/Computer_security#Eavesdropping) 및 [재전송 공격](https://en.wikipedia.org/wiki/Replay_attack)으로부터 보호한다. 다만, [DNS](Domain.md#도메인-네임-시스템)에 의존하기 때문에 호스트 접근을 IP 주소로 시도하면 커버로스 인증을 실패한다; 윈도우는 대안 인증 프로토콜로 [NTLM](#ntlm)을 시도한다.<sup>[<a href="https://learn.microsoft.com/en-us/windows-server/security/kerberos/configuring-kerberos-over-ip">참고</a>]</sup>

커버로스 인증은 [키 배포 센터](https://learn.microsoft.com/en-us/windows/win32/secauthn/key-distribution-center)(key distribution center; KDC)에서 두 가지의 에이전트로 나뉘어 동작한다.

<table style="table-layout: fixed; width: 85%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">커버로스 KDC 에이전트 및 발급 티켓</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">인증 서버 (authentication server; AS)</th><th style="text-align: center;">티켓 부여 서비스 (ticket-granting service; TGS)</th></tr></thead><tbody style="text-align: left;"><tr><td>사용자 인증을 담당하며, 클라이언트에게 발급하는 티켓(즉, ticket-granting ticket; TGT)은 시한부로 사용자 인증을 증명한다. 최초 TGT 발급은 사용자가 컴퓨터에 로그온 할 때이다.</td><td>인증된 사용자의 TGT 유효성 및 요청한 서비스로의 접근 권한을 점검한다. 모두 충족할 경우, 클라이언트가 요청한 서비스에 한정된 티켓(즉, service ticket; ST)을 발급한다.</td></tr></tbody></table>

![커버로스 인증 흐름도](https://upload.wikimedia.org/wikipedia/commons/6/68/Kerberos_protocol.svg)

커버로스 인증 중에서 AS로부터 인증된 사용자인지 여부를 살펴보는 단계가 먼저 진행된다.

1. 컴퓨터 로그온에 사용된 사용자명과 커버로스 인증 요청을 AS로 전송한다.
1. AS는 AD 데이터베이스에 해당 사용자 정보를 조회하여 (1) 비밀번호로 암호화된 TGS 세션 키와 (2) TGS만 열어볼 수 있는 TGT를 클라이언트로 회신한다.
1. 클라이언트는 사용자 비밀번호로 TGS 세션 키를 복호화하며, 이는 차후 TGS와 세션을 구축하는데 사용된다.

만일 사용자가 도메인 리소스나 서비스를 접근해야 할 경우, TGS로부터 인증 유효성 및 권한을 점검받는다.

4. 클라이언트는 (획득하였으나 열어볼 수 없던) TGT에 원하는 서비스로의 접근 요청을 동봉하여 TGS로 전송한다.
4. TGS는 클라이언트로부터 전달 받은 TGT를 열어, 안에 포함된 정보들로 인증 유효성 또는 서비스 접근 권한을 검증한다.
4. 모든 검증을 충족하였을 경우, TGS는 해당 서비스의 서버만 열어볼 수 있는 ST를 클라이언트로 회신한다.

마지막으로 클라이언트는 서비스 서버로 접근을 시도한다.

7. 클라이언트는 (획득하였으나 열어볼 수 없던) ST를 서비스 서버로 전송한다.
7. 서비스 서버는 클라이언트로 전달 받은 ST를 열어, 안에 포함된 정보들로 인증 유효성 또는 서비스 접근 권한을 검증한다.
7. 모든 검증을 충족하였을 경우, 서비스 서버는 본격적으로 클라이언트가 요청한 서비스를 제공한다.

### NTLM
> *참고: [NTLM Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/kerberos/ntlm-overview)*

**[NT LAN 관리자](https://en.wikipedia.org/wiki/NTLM)**(NT LAN Manager; NTLM)는 마이크로소프트에서 개발한 [시도-응답 인증 프로토콜](https://en.wikipedia.org/wiki/Challenge%E2%80%93response_authentication)이다.
