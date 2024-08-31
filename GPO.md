# 그룹 정책
**[그룹 정책](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview)**(Group Policy)은 [윈도우 OS](Windows.md) 컴퓨터와 사용자들의 설정을 구성하고 관리한다. 일반 클라이언트 OS 뿐만 아니라 서버에 특화된 동작이나 보안 설정을 구성할 때에도 활용된다. 그룹 정책은 로컬 [파일 시스템](FileSystem.md)에서, 또는 [액티브 디렉토리 도메인 서비스](ADS.md#도메인-서비스)를 통해 편집할 수 있다.

* 컴퓨터 구성(Computer Configuration): 사용자 여부와 상관없이 시스템 전반에 영향을 미치는 정책들을 포함하며, 컴퓨터가 시작할 때 적용된다.
* 사용자 구성(User Configuration): 동일한 시스템이라도 개별 사용자에 따라 달리 적용되는 정책들을 포함하며, 사용자의 로그온 때 적용된다.

## 그룹 정책 개체
**[그룹 정책 개체](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/policy/group-policy-objects)**(Group Policy Object; GPO)은 그룹 정책 설정을 담고 있는 AD DS의 도메인 개체 중 하나이다. GPO는 아래 두 가지 구성요소로 이루어진다.

* 그룹 정책 컨테이너(Group Policy Container; GPC): GPO 이름 및 GUID, 버전, 접근 권한, 상태 등의 GPO 속성에 대한 정보를 담고 있다. GPC는 AD 도메인 데이터베이스에 저장되는 "GPO의 메타데이터"라고 이해할 수 있다.

* 그룹 정책 템플릿(Group Policy Template; GPT): [관리 템플릿 파일](#관리-템플릿) (.admx 확자자), 스크립트 등의 실질적인 정책 설정을 담는 [파일 시스템](FileSystem.md) 폴더이다. GPT는 각 도메인 컨트롤러의 SYSVOL 디렉토리의 Policies 폴더 안에 저장되어 있다.

AD 환경에서 PDC가 그룹 정책을 배포할 때, GPC와 GPT가 도메인을 구성하는 다른 DC에 복제된다.

### 관리 템플릿
**[관리자 템플릿](https://learn.microsoft.com/en-us/windows/client-management/understanding-admx-backed-policies)**(administrative template)

## 그룹 정책 처리
> *출처: [Group Policy processing for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-processing)*

다음은 AD 환경에서 GPO가 사용자 및 컴퓨터에 적용되는 과정을 소개한다.

![GPO 처리 순서도](https://stealthbits.com/wp-content/uploads/2019/04/image-33.png)

사용자가 로그인을 하거나 컴퓨터가 부팅할 시, 사용자와 컴퓨터에 적용될 GPO 목록을 AD로부터 전달받기 위해 [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) 쿼리를 전송한다. 쿼리는 AD 계층 내 사용자 또는 컴퓨터의 위치(예를 들어, 도메인, OU 등)를 기반하며, 아래는 ftp.contoso.com 도메인에 속한 사용자의 LDAP 쿼리 요청 예시이다.

```ldif
dn: CN=John Doe,OU=IC,OU=Developer,DC=ftp,DC=contoso,DC=com
```

LDAP 쿼리를 전달받은 AD DC는 해당 사용자 및 컴퓨터가 상주한 사이트, 도메인, 그리고 OU에 링크된 GPO 목록으로 회신한다. GPO 목록은 다음 순서대로 취득 및 처리된다: 로컬, 사이트, [도메인](Domain.md), 그 다음 [조직 구성 단위](ADS.md#조직-구성-단위)(OU)이다. 네스티드 OU의 경우, 부모에서 자식 순서대로 GPO를 적용한다.

* 상위 정책과 겹치는 부분이 있어도, 하위 정책의 적용을 강제(enforcement)시킬 수 있다.
* 상위 정책이 적용되는 걸 방지하기 위해 상속(inheritance)을 차단할 수 있다.
* GPO는 보안 그룹 맴버쉽 또는 [WMI](WMI.md) 필터를 통해 특정 사용자나 컴퓨터에만 적용될 수 있도록 필터링될 수 있다.
