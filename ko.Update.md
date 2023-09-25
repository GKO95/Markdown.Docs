---
category: 윈도우
title: 업데이트
---
# WaaS
[Windows as a Service](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview), 일명 WaaS는 [윈도우 NT](ko.Windows.md)의 운영체제 파일, 레지스트리 값, 또는 시스템 상태 구성을 변경하는 모든 작업을 일컫는다. [윈도우 업데이트](#윈도우-업데이트), 역할 및 기능 추가, 또는 [드라이버](ko.Driver.md#장치-드라이버) 설치 등이 전부 윈도우 서비스에 해당한다.

본 문서에 앞서 마이크로소프트에서 사용되는 소프트웨어 [업데이트 용어](https://learn.microsoft.com/en-us/troubleshoot/windows-client/deployment/standard-terminology-software-updates)를 소개한다.

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">마이크로소프트 업데이트 용어집</caption><colgroup><col style="width: 25%;"/><col style="width: 75%;"/></colgroup><thead><tr><th style="text-align: center;">용어</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#update">업데이트</a><br/>(Update)</td><td>보안과 관련이 없으면서 치명적이지 않은 버그를 해결하기 위해 널리 제공되는 업데이트이다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#critical-update">중요 업데이트</a><br/>(Critical update)</td><td>보안과 관련이 없지만 치명적인 특정 버그를 해결하기 위해 널리 제공되는 업데이트이다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#security-update">보안 업데이트</a><br/>(Security update)</td><td>특정 제품의 보안 취약성을 해결하기 위해 널리 제공되는 업데이트이다. 마이크로소프트는 보안 취약성 등급을 다음과 같이 분류한다:<ul><li>위험(critical)</li><li>중요(important)</li><li>보통(moderate)</li><li>낮음(low)</li></ul></td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#definition-update">정의 업데이트</a><br/>(Definition update)</td><td>제품의 정의 데이터베이스에 항목을 추가하기 위해 빈번히 널리 제공되는 업데이트이다. 정의 데이터베이스는 악성 코드, 피싱 웹사이트, 정크 메일 등의 특성을 지닌 객체를 감지하는 데 사용된다.</td></tr></tbody></table>

## 윈도우 업데이트
레거시 윈도우 OS는 새로운 버전이 출시되기까지 대략 3-5년 동안 업데이트를 기다려야 했다면, 윈도우 10을 기점으로 소규모 기능 업데이트를 주기적으로 빈번하게 제공하는 방식으로 변경되었다. 한번에 많은 변화를 적용시키지 않아 사용자가 느끼는 피로감을 줄이는 효과가 있다.

아래는 윈도우 10의 WaaS부터 변동된 [업데이트 유형](https://learn.microsoft.com/en-us/windows/deployment/update/get-started-updates-channels-tools#types-of-updates)들을 소개한다:

* **[기능 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#annual-feature-updates)**(Feature updates)

    연간 하반기에 새로운 기능을 OS에 추가하는 업데이트이며, 기능 업데이트가 설치되면 윈도우 OS가 상위버전으로 올라가기 때문에 흔히 *In-Place 업그레이드*라고 언급된다. 해당 업데이트는 사용자의 모든 어플리케이션, 설정, 그리고 데이터를 유지할 수 있다. 2015년에 출시된 [초기 버전](https://support.microsoft.com/en-us/topic/windows-10-update-history-93345c32-4ae1-6d1c-f885-6c0b718adf3b)의 윈도우 10에 소개된 버전 [2004](https://support.microsoft.com/en-us/topic/windows-10-update-history-24ea91f4-36e7-d8fd-0ddb-d79d9d0cdbda), [20H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-7dd3071a-3906-fa2c-c342-f7f86728a6e3), [21H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-857b8ccb-71e4-49e5-b3f6-7073197d98fb), 또는 [22H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-8127c2c6-6edf-4fdf-8b9f-0f7be1ef3562) 등 In-Place 업그레이드가 해당한다.

* **품질 업데이트**(Quality updates)

    해당 버전의 윈도우 OS에서 발견된 문제나 성능 및 보안을 개선하는 등 품질 관리를 위한 업데이트이다. 이전 품질 업데이트를 모두 내포하기 때문에 *누적 업데이트*라고 부른다. 품질 업데이트는 아래와 같이 보안 및 비보안 업데이트가 존재한다:

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">품질 업데이트 구성</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#monthly-security-update-release">월별 보안 업데이트</a></th><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#optional-non-security-preview-release">선택적 비보안 미리 보기</a></th></tr></thead><tbody><tr style="text-align: center;"><td>두 번째 화요일 오전 10시 PST</td><td>네 번째 화요일 오전 10시 PST</td></tr><tr style="text-align: center;"><td><a href="https://support.microsoft.com/en-us/topic/december-13-2022-kb5021233-os-builds-19042-2364-19043-2364-19044-2364-and-19045-2364-44e774aa-60c4-4e38-b7e7-c886d210db3b">KB5021233</a>(2022.12B), <a href="https://support.microsoft.com/en-us/topic/january-10-2023-kb5022282-os-builds-19042-2486-19044-2486-and-19045-2486-9587e4e3-c2d7-48a6-86e2-8cd9146b47fd">KB5022282</a>(2023.01B) 등</td><td><a href="https://support.microsoft.com/en-us/topic/november-15-2022-kb5020030-os-builds-19042-2311-19043-2311-19044-2311-and-19045-2311-preview-237a9048-f853-4e29-a3a2-62efdbea95e2">KB5020030</a>(2022.11C), <a href="https://support.microsoft.com/en-us/topic/january-19-2023-kb5019275-os-builds-19042-2546-19044-2546-and-19045-2546-preview-8ae1b678-d38c-4249-848d-06b722e7c0ad">KB5019275</a>(2023.01C) 등</td></tr><tr><td>안정성 수정 및 보안 취약성 해결; 대부분 기업 또는 조직에서는 의무적으로 설치.</td><td>다음 월별 보안 업데이트 이전에 안정성 패치를 미리 적용하여 유효성 확인 가능.</td></tr></tbody></table>

    > 최신 월별 보안 업데이트 및 선택적 비보안 미리 보기를 각각 "LCU (Latest Cumulative Update)" 그리고 "LCU Preview"라고 언급한다.

    이전 윈도우 OS 버전의 월별 업데이트는 개수가 상당하여 관심있는 업데이트만 선택적으로 설치하는 경우가 다반수였다. 이러한 행위는 마이크로소프트도 헤아릴 수 없는 수많은 업데이트 조합, 일명 "무한한 파편화(infinite fragmentation)"를 초래하여 업데이트 여부에 의한 호환성 문제 및 비정상 시스템 증상을 유발하였다. 품질 업데이트는 파편화를 방지하기 위해 이전 업데이트를 전부 포함시켜 누락된 게 없도록 조치하였다.

* **[서비스 스택 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/servicing-stack-updates)**(Servicing stack update; SSU)

    서비스 스택(servicing stack)은 윈도우 업데이트 설치를 실질적으로 담당하는 ([CBS](#컴포넌트-기반-서비스)를 포함한) 이진 파일들의 집합을 가리킨다. SSU는 서비스 스택에 패치가 필요할 때만 배포되며 재부팅을 요하지 않으나, 한 번 설치된 서비스 스택은 더 이상 제거될 수 없다. 서비스 스택을 최신 버전으로 유지하지 않으면 최신 윈도우 업데이트를 설치하는 데 문제가 발생할 수 있다. 최근 SSU는 품질 업데이트에 포함되어 있으나, 예를 들어 [윈도우 서버 2019](https://ko.wikipedia.org/wiki/윈도우_서버_2019)의 [KB5028168](https://support.microsoft.com/en-us/topic/july-11-2023-kb5028168-os-build-17763-4645-eff2d1e1-5f91-4d9a-aef1-ae26bdf51321) 업데이트는 2021년 8월 10일 SSU([KB5005112](https://support.microsoft.com/en-us/topic/kb5005112-servicing-stack-update-for-windows-10-version-1809-august-10-2021-df6a9e0d-8012-41f4-ae74-b79f1c1940b2))를 반드시 사전에 설치되어야 한다.

* **드라이버 업데이트**(Driver updates)

    시스템 장치에 적합한 [드라이버](ko.Driver.md#장치-드라이버) 업데이트를 제공한다. [마이크로소프트 서피스](https://aka.ms/surface) 제품에 한해 [펌웨어](https://ko.wikipedia.org/wiki/펌웨어) 업데이트도 함께 전달한다.

* **마이크로소프트 제품 업데이트**(Microsoft product updates)

    윈도우 10의 WaaS는 운영체제 대상의 윈도우 업데이트(일명 WU) 외에도 [오피스](https://aka.ms/m365), [디펜더](https://aka.ms/msdefender), [Exchange](https://aka.ms/exchange), [SQL](https://aka.ms/sql) 등의 퍼스트 파티 소프트웨어를 대상으로 마이크로소프트 업데이트(Microsoft Update; MU) 서비스를 지원한다. WU는 모든 윈도우에 기본적으로 활성화된 한편, 상위 서비스인 MU는 사용자 혹은 프로그램에 의해 수동으로 활성화되어야 한다.

### 레거시 OS 업데이트
한편, 당시 기술지원이 유효하였던 레거시 윈도우 클라이언트 및 서버 OS에서도 업데이트 파편화를 최소화하고자 2016년 10월부터 누적 업데이트 모델로 전환하였다. 그러나 윈도우 10의 LCU 방식을 그대로 채택하기에 업데이트 패키지의 용량과 변화가 부담되어 다음과 같이 변경하였다.

* **[월별 롤업](https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#monthly-rollup)**(Monthly Rollup)

    특정 제품에 대한 보안성 및 안정성 업데이트를 하나의 패키지로 제공하며, 이전 월별 롤업을 모두 내포하는 누적 업데이트이다. 레거시 윈도우 OS에서 중요 업데이트로 분류되며 "보안 월별 품질 롤업"이란 이름으로 다운로드 및 설치된다. 윈도우 10 WaaS의 누적 업데이트와 유사한 모델을 지닌다.

    * **[월별 롤업 미리보기](https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#preview-of-monthly-rollup)**(Preview of Monthly Rollup)

        특정 제품에 대한 이전 월별 롤업과 비보안성 업데이트를 하나의 패키지로 제공하는 누적 업데이트이다. 레거시 윈도우 OS에서 선택적 업데이트로 분류되며 "보안 월별 품질 롤업 미리보기"란 이름으로 다운로드 및 설치된다.

* **[보안 전용 업데이트](https://learn.microsoft.com/ko-kr/troubleshoot/windows-client/deployment/standard-terminology-software-updates#security-only-update)**(Security-only update)

    특정 제품의 당월 보안성 업데이트만 포함된 패키지로써 제공되는 업데이트이다. 즉, 이번달 보안 전용 업데이트를 설치하지 않으면 다음달 보안 전용 업데이트에 누락되어 나타나지 않는다. 레거시 윈도우 OS에서 중요 업데이트로 분류되어 "보안 전용 품질 업데이트"란 이름으로 다운로드 및 설치된다.

2016년 10월 이전의 업데이트는 월별 롤업에 포함되지 않기 때문에, 2016년 9월 SSU가 누락되어 2018년 8월 보안 업데이트 설치가 실패하는 이슈가 발생하는 사례가 있다. 즉, 레거시 윈도우 OS는 여전히 업데이트 파편화 문제에 노출되어 있어 주의가 필요하다.

## 서비스 채널
[서비스 채널](https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#servicing-channels)(Servicing channel)은 설치된 윈도우 OS를 얼마나 자주 업데이트할 것인지 지정할 수 있도록 한다. 윈도우 클라이언트 OS에는 총 세 가지의 서비스 채널이 다음과 같이 존재한다:

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">윈도우 업데이트 서비스 채널</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">서비스 채널</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#general-availability-channel">일반 공급 채널</a><br/>(General Availability; GA)</td><td>기능 업데이트를 매년 하반기에 출시하는 채널이다. 업데이트에 대한 설정이 구성되어 있지 않으면 장치는 기본적으로 해당 채널을 통해 가능한 즉시 업데이트를 설치한다. 이전에는 3월과 9월을 기점으로 출시되는 반기 채널(Semi-Annual Channel)이라고 불렸으나 <a href="https://en.wikipedia.org/wiki/Windows_11_version_history#Version_21H2_(original_release)">21H2</a> 버전 이후부터 GA로 <a href="https://blogs.windows.com/windowsexperience/2021/11/16/how-to-get-the-windows-10-november-2021-update/">변경</a>되었다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#long-term-servicing-channel">장기 서비스 채널</a><br/>(Long-term Servicing Channel; LTSC)</td><td>기능 업데이트를 배제하고 오로지 품질 업데이트만 배포하여 장치의 보안성을 보장하는 채널이다. 윈도우 엔터프라이즈 LTSC 제품을 대상으로 품질 업데이트가 출시되지만 설치 여부는 사용자가 선택할 수 있다. LTSC는 ATM이나 의료기기와 같은 특수 장치를 위해서만 사용되어야 한다.</td></tr><tr><td style="text-align: center;"><a href="https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#long-term-servicing-channel">윈도우 참가자 프로그램</a><br/>(Windows Insider Program)</td><td>GA로 출시되기 전에 미리 기능 업데이트를 체험할 수 있는 채널이다. 참가자들은 해당 기능 업데이트가 GA에 출시되기 전에 마이크로소프트에 이슈를 보고하는 등의 피드백으로 기여할 수 있다.</td></tr></tbody></table>

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

<table style="width: 95%; margin: auto;"><caption style="caption-side: top;">업데이트 클라이언트 구성</caption><colgroup><col style="width: 30%;"/><col style="width: 70%;"/></colgroup><thead><tr><th style="text-align: center;">구성</th><th style="text-align: center;">설명</th></tr></thead><tbody><tr><td style="text-align: center;">비관리형 자동 업데이트</td><td>WU 클라이언트가 클라우드 서비스에서 업데이트를 스캔하여 자동으로 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">비관리형 API 업데이트</td><td>WU 클라이언트가 클라우드 서비스에서 업데이트를 스캔하고 API 호출자를 통해 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">관리형 자동 업데이트</td><td>WU 클라이언트가 WSUS 서버로부터 업데이트를 스캔하여 자동으로 업데이트가 진행된다.</td></tr><tr><td style="text-align: center;">관리형 API 업데이트</td><td>WU 클라이언트가 WSUS 서버로부터 업데이트를 스캔하고 API 호출자를 통해 업데이트가 진행된다.</td></tr></tbody></table>

# 컴포넌트 기반 서비스
윈도우 서비스는 운영체제의 컴포넌트화 개념을 기반하여 설계되었다. [컴포넌트](#컴포넌트) 단위로 분리하되, 다양한 분야의 윈도우 개발자들이 기준되는 [매니페스트](https://ko.wikipedia.org/wiki/매니페스트_파일)로부터 프로그램을 개발하여 균일화에 기여한다. 그리고 [윈도우 서비스](#waas)([업데이트](#윈도우-업데이트), 역할 및 기능, [드라이버](ko.Driver.md#장치-드라이버) 추가 등)의 롤백을 지원하여 더욱 견고한 파일 및 컴포넌트 설치 및 제거에 이바지한다.

### 컴포넌트
컴포넌트(component)는 윈도우 서비스에 필요한 기능을 제공하는 가장 기초적인 구성 요소이며, 이를 위해 필요한 (바이너리, 레지스트리 값, [서비스](ko.Service.md) 및 [보안 기술자](https://en.wikipedia.org/wiki/Security_descriptor) 등) 리소스들을 [매니페스트](https://ko.wikipedia.org/wiki/매니페스트_파일)로부터 정의된다.

> 컴포넌트 스토어(component store)는 필요에 따라 파일 시스템에 반영될 수 있도록 모든 버전의 컴포넌트들이 저장된 공간으로 [WinSxS](https://en.wikipedia.org/wiki/Side-by-side_assembly#WinSxS_(Windows_component_store)) 디렉토리가 해당한다.

## 컴포넌트 기반 서비스 스택
> *참고: [Understanding Component-Based Servicing - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/ask-the-performance-team/understanding-component-based-servicing/ba-p/373012)*

컴포넌트 기반 서비스 스택(Component-based servicing stack), 일명 CBS는 윈도우 배포, 기능 및 역할 변경, 그리고 컴포넌트 수리에 핵심 요소로 도맡는 윈도우 모듈 설치자 TrustedInstaller.exe를 가리킨다. 아래와 같이 CBS가 개입된 프로그램이 실행되면 `%WinDir%\Logs\CBS\CBS.log` 파일에 활동 이력이 기록된다.

* DISM
* SFC
