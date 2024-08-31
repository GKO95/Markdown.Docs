# 인증
> *참고: [Windows Authentication Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)*

**[인증](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)**(authentication)은 시스템에서 식별된 사용자가 실제로 본인이 맞는지 검증하는 절차이다: [비밀번호](https://learn.microsoft.com/en-us/windows-server/security/kerberos/passwords-overview), [스마트카드](https://en.wikipedia.org/wiki/Smart_card), [생체인증](https://en.wikipedia.org/wiki/Biometrics) 등이 해당한다. 네트워크 리소스를 비인가된 사용자로부터 접근을 원천 차단하며, 인가된 사용자는 원격에서도 안전하게 접근할 수 있는 보안을 제공한다.

* 한편, 유사한 용어인 [authorization](https://en.wikipedia.org/wiki/Authorization)은 인증이 완료된 사용자가 해당 리소스를 접근할 "권한"이 있는지 확인하는 것으로 별개의 절차임을 유의하도록 한다.

## 인증 프로토콜
**[인증 프로토콜](https://en.wikipedia.org/wiki/Authentication_protocol)**(authentication protocol)은 두 개체 간 인증 데이터를 전송하기 위해 설계된 [통신 프로토콜](https://en.wikipedia.org/wiki/Communication_protocol)이다.

### 커버로스
> *참고: [Kerberos Authentication Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview)*

**[커버로스](https://en.wikipedia.org/wiki/Kerberos_(protocol))**(Kerberos)는 [상호인증](https://en.wikipedia.org/wiki/Mutual_authentication)에 사용되는 인증 프로토콜 중 하나이다.

![커버로스 인증 흐름도](https://learn.microsoft.com/en-us/troubleshoot/windows-server/windows-security/media/kerberos-authentication-troubleshooting-guidance/authentication-flow.png)

### NTLM
> *참고: [NTLM Overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/kerberos/ntlm-overview)*

**[NT LAN 관리자](https://en.wikipedia.org/wiki/NTLM)**(NT LAN Manager; NTLM)는 마이크로소프트에서 개발한 [시도-응답 인증 프로토콜](https://en.wikipedia.org/wiki/Challenge%E2%80%93response_authentication)이다.

# 인증서
**[인증서](https://en.wikipedia.org/wiki/Authorization_certificate)**(certificate)
