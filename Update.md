# 윈도우 업데이트
과거의 레거시 Windows OS 및 [서비스 팩](https://en.wikipedia.org/wiki/Service_pack)(現 누적 업데이트)은 3-5년 주기로 출시되어, 이를 파일로 구하여 설치를 진행하였다. [Windows 10](https://en.wikipedia.org/wiki/Windows_10)부터는 2-4주 간격으로 "서비스"처럼 제공되는 소규모 업데이트로 바뀌어, 이를 **[Windows as a Service](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview)**(WaaS)라고 부른다. 다시 말해, Windows 이미지 및 업데이트 배포, 역할 및 기능 추가, 그리고 드라이버 설치 등의 행위는 전부 Windows Servicing이라고 언급한다.

아래는 윈도우 10의 WaaS부터 변동된 [업데이트 유형](https://learn.microsoft.com/windows/deployment/update/get-started-updates-channels-tools#types-of-updates)들을 소개한다:

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">Windows Servicing 유형</caption><colgroup><col style="width: 20%;"/><col style="width: 80%;"/></colgroup><thead><tr><th style="text-align: center;">용어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows/deployment/update/release-cycle#annual-feature-updates">기능 업데이트</a><br/>(Feature Update)</td><td>연간 하반기(즉, H2)에 기존 프로그램, 설정, 데이터 등을 유지한 채 새로운 기능을 Windows에 추가하는 업데이트이다. 기능 업데이트가 설치되면 (1) Windows 10에서 11으로 넘버링이 올라가거나 (2) 22H2에서 24H2로 버전이 상승하는 등의 빌드 변화가 일어난다.</td></tr><tr><td style="text-align: center;">품질 업데이트<br/>(Quality Update)</td><td>해당 버전의 Windows에서 발견된 문제나 성능 및 보안을 개선하는 등 품질 관리를 위한 업데이트이다. 이전 품질 업데이트를 모두 내포하여 <i>누적 업데이트</i>라고도 부르며, 크게 두 종류로 나뉘어진다.<ol><li><a href="https://learn.microsoft.com/windows/deployment/update/release-cycle#monthly-security-update-release">월별 보안 업데이트</a>(두 번째 화요일 오전 10시 PST): <i>안정성 수정 및 보안 취약성 해결; 대부분 기업 또는 조직에서는 의무적으로 설치한다.</i></li><li><a href="https://learn.microsoft.com/windows/deployment/update/release-cycle#optional-non-security-preview-release">선택적 비보안 미리 보기</a>(네 번째 화요일 오전 10시 PST): <i>다음 월별 보안 업데이트 이전에 안정성 패치를 미리 적용하여 유효성을 확인할 수 있다.</i></li></ol>최신 월별 보안 업데이트 및 선택적 비보안 미리 보기를 각각 "LCU (Latest Cumulative Update)" 그리고 "LCU Preview"라고 언급한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/windows/deployment/update/servicing-stack-updates">서비스 스택 업데이트</a><br/>(Servicing Stack Update; SSU)</td><td>Windows 업데이트를 담당하는 시스템 파일들, 즉 <a href="#서비스-스택">서비스 스택</a>을 대상으로 적용되는 매우 중요한 업데이트이다. Windows 업데이트에는 SSU 버전 최소 요구사항이 있으며, 이를 충족하지 못하면 업데이트가 불가하다. 항상 최신 상태로 유지하기 위해 SSU는 한번 설치되면 제거될 수 없다.<ul><li>2021년 2월부터 최신 버전의 SSU는 품질 업데이트에 함께 포함되어 배포되고 있다.</li></ul></td></tr><tr><td style="text-align: center;">드라이버 업데이트</td><td>시스템 장치에 적합한 <a href="Driver.md">드라이버</a> 및 <a href="https://en.wikipedia.org/wiki/Firmware">펌웨어</a> (<a href="https://aka.ms/surface">마이크로소프트 서피스</a> 한정)  업데이트를 제공한다.</td></tr><tr><td style="text-align: center;">마이크로소프트 제품 업데이트</td><td><a href="https://aka.ms/m365">오피스</a>, <a href="https://aka.ms/msdefender">디펜더</a>, <a href="https://aka.ms/exchange">Exchange</a>, <a href="https://aka.ms/sql">SQL</a> 등의 마이크로소프트 퍼스트 파티 소프트웨어를 대상으로 업데이트 서비스를 지원하며, 사용자 혹은 프로그램에 의해 수동으로 활성화되어야 한다.</td></tr></tbody></table>

<sup>_† 마이크로소프트에서 사용되는 소프트웨어 <a href="https://learn.microsoft.com/troubleshoot/windows-client/deployment/standard-terminology-software-updates">업데이트 용어</a>도 함께 살펴보는 걸 권장한다._</sup>

### 업데이트 파편화
WaaS가 소개되기 이전에는 월별로 다양한 Windows 업데이트를 개별적으로 제공하였다. 사용자는 관심있는 업데이트만 선택적으로 설치할 수 있었는데, 이는 헤아릴 수 없는 수많은 업데이트 조합의 "**무한한 파편화**(infinite fragmentation)"를 초래하였다. 새로 배포될 업데이트가 이로 인해 발생하는 호환성 문제 및 비정상 시스템 증상을 마이크로소프트는 사실상 추적하고 관리하는 게 불가능하였다. 그리고 WaaS의 품질 업데이트는 파편화를 방지하기 위해 이전 업데이트를 전부 포함시켜 누락된 게 없도록 조치하였다.

한편, 당시 기술지원이 유효하였던 레거시 Windows의 클라이언트 및 서버 OS에서도 업데이트 파편화를 최소화하고자 2016년 10월부터 누적 업데이트 모델로 전환하였다. 단, 업데이트 패키지의 용량 부담과 부작용을 최소화하기 위해 다음과 같이 변경하였다.

> 현 시점에서 Windows 10 이전에 출시된 레거시 Windows OS는 모두 기술지원이 종료되면서 아래 용어는 더 이상 언급되지 않는다.

* **[월별 롤업](https://learn.microsoft.com/troubleshoot/windows-client/deployment/standard-terminology-software-updates#monthly-rollup)** & **[월별 롤업 미리보기](https://learn.microsoft.com/troubleshoot/windows-client/deployment/standard-terminology-software-updates#preview-of-monthly-rollup)**(Monthly Rollup & Preview of Monthly Rollup)

    특정 제품에 대한 보안성 및 안정성 업데이트를 하나의 패키지로 제공하며, 이전 월별 롤업을 모두 내포하는 누적 업데이트이다. 레거시 윈도우 OS에서 중요 업데이트로 분류되며 "보안 월별 품질 롤업" 및 "보안 월별 품질 롤업 미리보기"이란 이름으로 다운로드 및 설치된다. WaaS의 품질 업데이트와 유사한 모델을 지닌다.

* **[보안 전용 업데이트](https://learn.microsoft.com/troubleshoot/windows-client/deployment/standard-terminology-software-updates#security-only-update)**(Security-only update)

    특정 제품의 당월 보안성 업데이트만 포함된 패키지로써 제공되는 업데이트이다. 즉, 이번달 보안 전용 업데이트를 설치하지 않으면 다음달 보안 전용 업데이트에 누락되어 나타나지 않는다. 레거시 윈도우 OS에서 중요 업데이트로 분류되어 "보안 전용 품질 업데이트"란 이름으로 다운로드 및 설치된다.

## 서비스 채널
[서비스 채널](https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#servicing-channels)(Servicing channel)은 설치된 윈도우 OS를 얼마나 자주 업데이트할 것인지 지정할 수 있도록 한다. 윈도우 클라이언트 OS에는 총 세 가지의 서비스 채널이 다음과 같이 존재한다:

1. **[일반 공급 채널](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#general-availability-channel)**(General Availability; GA)

    기능 업데이트를 매년 하반기에 출시하는 채널이다. 업데이트에 대한 설정이 구성되어 있지 않으면 장치는 기본적으로 해당 채널을 통해 가능한 즉시 업데이트를 설치한다. 이전에는 3월과 9월을 기점으로 출시되는 반기 채널(Semi-Annual Channel)이라고 불렸으나 [21H2](https://en.wikipedia.org/wiki/Windows_11_version_history#Version_21H2_(original_release)) 버전 이후부터 GA로 [변경](https://blogs.windows.com/windowsexperience/2021/11/16/how-to-get-the-windows-10-november-2021-update/)되었다.

1. **[장기 서비스 채널](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#long-term-servicing-channel)**(Long-term Servicing Channel; LTSC)

    기능 업데이트를 배제하고 오로지 품질 업데이트만 배포하여 장치의 보안성을 보장하는 채널이다. 윈도우 엔터프라이즈 LTSC 제품을 대상으로 품질 업데이트가 출시되지만 설치 여부는 사용자가 선택할 수 있다. LTSC는 ATM이나 의료기기와 같은 특수 장치를 위해서만 사용되어야 한다.

1. **[윈도우 참가자 프로그램](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#windows-insider)**(Windows Insider Program)

    GA로 출시되기 전에 미리 기능 업데이트를 체험할 수 있는 채널이다. 참가자들은 해당 기능 업데이트가 GA에 출시되기 전에 마이크로소프트에 이슈를 보고하는 등의 피드백으로 기여할 수 있다.

> 범용 및 특수 장치를 구분하는 가장 일반적인 방법으로 최신 빌드가 권장되는 [마이크로소프트 365](https://www.microsoft.com/Microsoft-365) 제품 설치 여부로 판단한다. 

만일 LTSC에서 기능 업데이트가 적용된 상위 빌드 또는 채널을 GA로 변경하려면 인플레이스 업그레이드가 유일한 방안이다. 이전에는 장기 서비스 브랜치(Long-term Servicing Branch; LTSB)로 불렸으며, 일부 제품은 현재까지도 LTSB로 언급되어 사용되고 있다(예를 들어, [윈도우 2016 LTSB](https://learn.microsoft.com/en-us/lifecycle/products/windows-10-2016-ltsb) 등).

## 서비스 관리 도구
다음은 윈도우 업데이트에 대한 서비스 관리 도구를 소개한다.

* 윈도우 업데이트(Windows Update; WU)
* [비즈니스용 윈도우 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/waas-manage-updates-wufb)(Windows Update for Business; WUfB)
* 윈도우 서버 업데이트 서비스(Windows Server Update Services; WSUS)
* 시스템 센터 구성 관리자(System Center Configuration Manager; SCCM)
* [인튠](https://learn.microsoft.com/en-us/mem/intune/fundamentals/what-is-intune)(Intune)

윈도우 업데이트 클라이언트(일명 WU 클라이언트)는 윈도우에 탑재되어 자동 업데이트 및 사용자측 업데이트 관리를 위한 UI나 API 형태의 인터페이스를 제공하는 구성요소이다. WU 서비스와 클라이언트의 설정 조합으로 다음과 같은 방식의 구성이 가능하다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">업데이트 클라이언트 구성</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">구성</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">비관리형 자동 업데이트</td><td>WU 클라이언트가 클라우드 서비스에서 업데이트를 스캔하여 자동으로 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">비관리형 API 업데이트</td><td>WU 클라이언트가 클라우드 서비스에서 업데이트를 스캔하고 API 호출자를 통해 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">관리형 자동 업데이트</td><td>WU 클라이언트가 WSUS 서버로부터 업데이트를 스캔하여 자동으로 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">관리형 API 업데이트</td><td>WU 클라이언트가 WSUS 서버로부터 업데이트를 스캔하고 API 호출자를 통해 업데이트가 진행된다.</td></tr></tbody></table>

# 컴포넌트 기반 서비스
> *참고: [Understanding Component-Based Servicing - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/ask-the-performance-team/understanding-component-based-servicing/ba-p/373012)*

컴포넌트 기반 서비스(Component-based servicing; CBS)는 [윈도우 비스타](Windows.md)부터 소개된 운영체제의 컴포넌트화 설계를 가리키며, [윈도우 업데이트](WaaS.md#윈도우-업데이트)나 [드라이버](Driver.md) 파일, 또는 시스템에 ([OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse) 서버와 같은) 추가 기능을 구현하는 선택적 [컴포넌트](#컴포넌트)를 이전 윈도우 OS보다 더 견고하고 안전하게 설치한다. 만일 부분적 혹은 부적합한 설치로 인한 불안정 이슈도 대응이 가능하다.

## 컴포넌트
컴포넌트(component)는 시스템 이미지를 구성하거나 추가 기능을 설치 및 제거하는데 필요한 기본 요소들을 가리키다.

<table style="width: 95%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">컴포넌트를 구성하는 파일</caption><colgroup><col style="width: 33.3%;"/><col/><col style="width: 33.3%;"/></colgroup><thead><tr><th style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Binary_file">바이너리 파일</a></th><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/win32/SbsCs/manifests">매니페스트 파일</a></th><th style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/install/catalog-files">카탈로그 파일</a></th></tr></thead><tbody><tr><td><a href="Process.md">실행 파일</a>, <a href="C.md#라이브러리">라이브러리</a>, <a href="Driver.md">드라이버</a> 등 시스템 이미지 내에서 실질적인 역할을 지닌 파일들이다: <a href="https://en.wikipedia.org/wiki/File_Explorer">explorer.exe</a>, <a href="WinAPI.md#시스템-서비스">ntdll.dll</a>, <a href="https://en.wikipedia.org/wiki/NTFS">ntfs.sys</a> 등.</td><td>해당 <a href="https://learn.microsoft.com/en-us/windows/win32/sbscs/about-side-by-side-assemblies-">SxS</a> 바이너리 파일의 의존성이나 메타데이터, 내장된 리소스 정보 등을 묘사한 .MANIFEST 확장자 파일이다.</td><td><a href="https://en.wikipedia.org/wiki/Cryptographic_hash_function">암호화 해시</a>(일명 <a href="https://en.wikipedia.org/wiki/Public_key_fingerprint">지문</a>)에 일대일 대응하는 파일들을 여럿 포함하여 한꺼번에 디지털 서명하는 .CAT 확장자 파일이다.</td></tr></tbody><caption style="caption-side: bottom; text-align: left;"><i><sub>† 매니페스트와 카탈로그 파일은 비주얼 스튜디오로 바이너리 파일을 빌드할 때 생성되는 부산물에 해당한다.</sub></i></caption></table>

모든 컴포넌트는 [WinSxS](#winsxs) 폴더에 저장되며, 올바르지 않은 컴포넌트 접근은 시스템 전반에 악영향을 미칠 수 있기 때문에 TrustedInstaller 권한만이 취급할 수 있다.

### 서비스 스택
**서비스 스택**(servicing stack)은 Windows 업데이트 등의 Servicing을 담당하는 컴포넌트이다.

## WinSxS
> *참고: [Manage the Component Store | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/manage-the-component-store)*

**[WinSxS](https://en.wikipedia.org/wiki/Side-by-side_assembly)**(Windows side-by-side의 약칭)는 [컴포넌트](#컴포넌트) 저장소이며 `%SystemRoot%\WinSxS` 폴더에 해당한다. 윈도우 시스템을 구성하는 모든 컴포넌트가 안에 있으며, 만일 [업데이트](#윈도우-업데이트)로 새로운 버전의 컴포넌트가 배포되어도 롤백을 지원하는 차원에서 이전 버전의 컴포넌트를 일정 기간동안 보관한다.<sup>[<a href="https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/clean-up-the-winsxs-folder">출처</a>]</sup>

WinSxS 폴더에 저장된 컴포넌트들은 [하드 링크](https://en.wikipedia.org/wiki/Hard_link)되어 타 윈도우 디렉토리에도 위치한다. 다음은 [KB5033375](https://support.microsoft.com/en-us/topic/december-12-2023-kb5033375-os-builds-22621-2861-and-22631-2861-90f983aa-efb6-4caa-9cab-7e5cfa00ed36) 12월 누적 업데이트가 설치된 윈도우 11, 버전 23H2 (빌드 22631.2861) 운영체제에서 [fsutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/fsutil) [hardlink](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/fsutil-hardlink) 명령으로 C:\Windows\explorer.exe, 즉 파일 탐색기에 하드 링크된 목록을 살펴본다.

```terminal
fsutil hardlink list C:\Windows\explorer.exe
```
```
\Windows\explorer.exe
\Windows\WinSxS\amd64_microsoft-windows-explorer_31bf3856ad364e35_10.0.22621.2792_none_3182ca8bf563ac2d\explorer.exe
```

두 개의 파일 경로가 발견되었지만, 하드 링크의 특성상 이 둘은 [디스크](Storage.md)의 동일한 영역을 참조하기 때문에 사실상 한 개의 파일이다. 한 경로의 파일을 변경하면 나머지에도 동일하게 반영된다. 예시의 컴포넌트는 C:\Windows 디렉토리에 위치하였으나 System32, Drivers 등의 다른 디렉토리에도 다양한 컴포넌트가 분포되어 있으며, WinSxS는 이들을 한 폴더에서 모두 관리할 수 있도록 한다.

운영체제 빌드가 22631.2861이지만, WinSxS에서 확인된 파일 버전은 [KB5032288](https://support.microsoft.com/en-us/topic/december-4-2023-kb5032288-os-builds-22621-2792-and-22631-2792-preview-538fbe4a-e9de-4312-85cd-d870444341d0) 12월 미리 보기 업데이트의 22621.2792로 확인되었다 (윈도우 11, 버전 23H2는 22H2와 공통된 운영체제 핵심 시스템 파일을 사용하기 때문에<sup>[<a href="https://support.microsoft.com/en-us/topic/windows-11-version-23h2-update-history-59875222-b990-4bd9-932f-91a5954de434">출처</a>]</sup> 빌드 번호 22621가 목격될 수 있다). KB5032288 이후로 파일 탐색기에 변경 사항이 없기 때문이며, WinSxS에서 파일을 최신 버전으로 유지하는 건 변함없다.
