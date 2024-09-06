# 인증서
**[공개 키 인증서](https://en.wikipedia.org/wiki/Public_key_certificate)**(public key certificate; PKC), 간단히 **인증서**(certificate)는 [공개 키](https://en.wikipedia.org/wiki/Key_authentication)의 유효성을 증명하는 전자 문서이다. 만일 클라이언트가 서버(즉, 인증서 소유자)와 [HTTPS](https://en.wikipedia.org/wiki/HTTPS) 등의 암호화된 통신 세션을 구축할 시, 먼저 클라이언트는 통신 요청을 공개 키로 암호화하여 서버로 전송해야 한다. 여기서 필요한 공개 키의 유효성을 입증하는 존재가 바로 인증서이다.

다음은 인증서 안에 포함된 내용물들을 소개한다.

![발급자로부터 소유자가 PKC를 취득하는 절차](https://upload.wikimedia.org/wikipedia/commons/6/65/PublicKeyCertificateDiagram_It.svg)

1. 인증서의 공개 키와 이에 대한 정보
1. 인증서의 소유자(subject) 신원 정보
1. 인증서의 내용을 검증한 발급자(issuer)의 디지털 서명

인증서를 살펴본 클라이언트가 발급자를 신뢰하고 유효한 디지털 서명임을 확인하였다면, 인증서에 포함된 공개 키를 사용하여 인증서 소유자와 보안된 통신이 이루어진다.

## 공개 키 기반 구조
**[공개 키 기반 구조](https://en.wikipedia.org/wiki/Public_key_infrastructure)**(public key infrastructure; PKI)

### 공개 키 암호 방식
**[공개 키 암호 방식](https://en.wikipedia.org/wiki/Public-key_cryptography)**(public-key cryptography)

## Schannel
> *참고: [TLS/SSL overview (Schannel SSP) | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/security/tls/tls-ssl-schannel-ssp-overview)*
