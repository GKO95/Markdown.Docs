---
category: 윈도우
title: 업데이트
---
# WaaS
[Windows as a Service](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview), 일명 WaaS는 [윈도우 NT](ko.Windows.md)의 새로운 버전이 출시되기까지 대략 3-5년 동안 업데이트를 기다려야 하는 방식과 달리, 소규모 기능 업데이트를 주기적으로 빈번하게 제공하는 방식을 일컫는다. WaaS로 배포된 업데이트는 한번에 많은 변화를 적용시키지 않아 사용자가 느끼는 피로감을 줄이는 효과가 있다. [윈도우 10 버전](https://ko.wikipedia.org/wiki/윈도우_10_버전_역사)의 경우, [1507](https://en.wikipedia.org/wiki/Windows_10_(original_release))으로 처음 출시되어 마지막 버전인 [22H2](https://en.wikipedia.org/wiki/Windows_10_version_history#Version_22H2_(2022_Update))까지 배포되었다.

아래는 윈도우 10의 WaaS [업데이트 유형](https://learn.microsoft.com/en-us/windows/deployment/update/get-started-updates-channels-tools#types-of-updates)을 소개한다:

* **[기능 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#annual-feature-updates)**(Feature updates)

    연간 하반기에 새로운 기능을 OS에 추가하는 업데이트이며, 기능 업데이트가 설치되면 윈도우 OS가 상위버전으로 올라가기 때문에 흔히 *In-Place 업그레이드*라고 언급된다. 해당 업데이트는 사용자의 모든 어플리케이션, 설정, 그리고 데이터를 유지할 수 있다. 2015년에 출시된 [초기 버전](https://support.microsoft.com/en-us/topic/windows-10-update-history-93345c32-4ae1-6d1c-f885-6c0b718adf3b)의 윈도우 10에 소개된 버전 [2004](https://support.microsoft.com/en-us/topic/windows-10-update-history-24ea91f4-36e7-d8fd-0ddb-d79d9d0cdbda), [20H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-7dd3071a-3906-fa2c-c342-f7f86728a6e3), [21H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-857b8ccb-71e4-49e5-b3f6-7073197d98fb), 또는 [22H2](https://support.microsoft.com/en-us/topic/windows-10-update-history-8127c2c6-6edf-4fdf-8b9f-0f7be1ef3562) 등 In-Place 업그레이드가 해당한다.

* **품질 업데이트**(Quality updates)

    해당 버전의 윈도우 OS에서 발견된 문제나 성능 및 보안을 개선하는 등 품질 관리를 위한 업데이트이다. 이전 품질 업데이트를 모두 내포하기 때문에 *누적 업데이트*라고 부른다. 품질 업데이트는 아래와 같이 보안 및 비보안 업데이트가 존재한다:

    <table style="width: 80%; margin: auto;"><caption style="caption-side: top;">품질 업데이트 구성</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#monthly-security-update-release">월별 보안 업데이트</a></th><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#optional-non-security-preview-release">선택적 비보안 미리 보기</a></th></tr></thead><tbody><tr style="text-align: center;"><td>두 번째 화요일 오전 10시 PST</td><td>네 번째 화요일 오전 10시 PST</td></tr><tr style="text-align: center;"><td>예시: <a href="https://support.microsoft.com/en-us/topic/december-13-2022-kb5021233-os-builds-19042-2364-19043-2364-19044-2364-and-19045-2364-44e774aa-60c4-4e38-b7e7-c886d210db3b">KB5021233</a>(2022.12B), <a href="https://support.microsoft.com/en-us/topic/january-10-2023-kb5022282-os-builds-19042-2486-19044-2486-and-19045-2486-9587e4e3-c2d7-48a6-86e2-8cd9146b47fd">KB5022282</a>(2023.01B) 등</td><td>예시: <a href="https://support.microsoft.com/en-us/topic/november-15-2022-kb5020030-os-builds-19042-2311-19043-2311-19044-2311-and-19045-2311-preview-237a9048-f853-4e29-a3a2-62efdbea95e2">KB5020030</a>(2022.11C), <a href="https://support.microsoft.com/en-us/topic/january-19-2023-kb5019275-os-builds-19042-2546-19044-2546-and-19045-2546-preview-8ae1b678-d38c-4249-848d-06b722e7c0ad">KB5019275</a>(2023.01C) 등</td></tr><tr><td>안정성 수정 및 보안 취약성 해결; 대부분 기업 또는 조직에서는 의무적으로 설치.</td><td>다음 월별 보안 업데이트 이전에 안정성 패치를 미리 적용하여 유효성 확인 가능.</td></tr></tbody></table>

    > 최신 월별 보안 업데이트 및 선택적 비보안 미리 보기를 각각 "LCU (Latest Cumulative Update)" 그리고 "LCU Preview"라고 언급한다.

    이전 윈도우 OS 버전의 월별 업데이트는 개수가 상당하여 관심있는 업데이트만 선택적으로 설치하는 경우가 다반수였다. 이러한 행위는 마이크로소프트도 헤아릴 수 없는 수많은 업데이트 조합, 일명 "무한한 파편화(infinite fragmentation)"를 초래하여 업데이트 여부에 의한 호환성 문제 및 비정상 시스템 증상을 유발하였다. 품질 업데이트는 파편화를 방지하기 위해 이전 업데이트를 전부 포함시켜 누락된 게 없도록 조치하였다.

* **[서비스 스택 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/servicing-stack-updates)**(Servicing stack update; SSU)

    서비스 스택(servicing stack)은 윈도우 업데이트 설치를 실질적으로 담당하는 ([CBS](#구성요소-기반-서비스-스택) 및 CSI를 포함한) 이진 파일들의 집합을 가리킨다. SSU는 서비스 스택에 패치가 필요할 때만 배포되며 재부팅을 요하지 않으나, 한 번 설치된 서비스 스택은 더 이상 제거될 수 없다. 서비스 스택을 최신 버전으로 유지하지 않으면 최신 윈도우 업데이트를 설치하는 데 문제가 발생할 수 있다. 최근 SSU는 품질 업데이트에 포함되어 있으나, 예를 들어 [윈도우 서버 2019](https://ko.wikipedia.org/wiki/윈도우_서버_2019)의 [KB5028168](https://support.microsoft.com/en-us/topic/july-11-2023-kb5028168-os-build-17763-4645-eff2d1e1-5f91-4d9a-aef1-ae26bdf51321) 업데이트는 2021년 8월 10일 SSU([KB5005112](https://support.microsoft.com/en-us/topic/kb5005112-servicing-stack-update-for-windows-10-version-1809-august-10-2021-df6a9e0d-8012-41f4-ae74-b79f1c1940b2))를 반드시 사전에 설치되어야 한다.

* **드라이버 업데이트**(Driver updates)

* **마이크로소프트 제품 업데이트**(Microsoft product updates)

### 구성요소 기반 서비스 스택
> 참고: *[Understanding Component-Based Servicing - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/ask-the-performance-team/understanding-component-based-servicing/ba-p/373012)*

구성요소 기반 서비스 스택(Component-based servicing stack), 일명 CBS는 윈도우 배포, 기능 및 역할 변경, 그리고 구성요소 수리에 핵심 요소로 도맡는 윈도우 모듈 설치자 TrustedInstaller.exe를 가리킨다. 아래와 같이 CBS가 개입된 프로그램이 실행되면 `%WinDir%\Logs\CBS\CBS.log` 파일에 활동 이력이 기록된다.

* DISM
* SFC

## 서비스 채널
[서비스 채널](https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#servicing-channels)(Servicing channel)은 장치에 설치된 윈도우 OS를 얼마나 자주 업데이트할 것인지 지정할 수 있도록 한다. 윈도우 클라이언트 OS에는 총 세 가지의 서비스 채널이 다음과 같이 존재한다:

* **[일반 공급 채널](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#general-availability-channel)**(General Availability Channel; GA)

    기능 업데이트를 매년 하반기에 출시하는 채널이다. 업데이트에 대한 설정이 구성되어 있지 않으면 장치는 기본적으로 해당 채널을 통해 가능한 즉시 업데이트를 설치한다. 이전에는 3월과 9월을 기점으로 출시되는, 즉 반기 채널(Semi-Annual Channel)이라고 불렸으나 [21H2](https://en.wikipedia.org/wiki/Windows_11_version_history#Version_21H2_(original_release)) 버전 이후부터 GA로 [변경](https://blogs.windows.com/windowsexperience/2021/11/16/how-to-get-the-windows-10-november-2021-update/)되었다.

* **[장기 서비스 채널](https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#long-term-servicing-channel)**(Long-term Servicing Channel; LTSC)

    기능 업데이트를 배제하고 오로지 품질 업데이트만 배포하여 장치의 보안성을 보장하는 채널이다. 윈도우 엔터프라이즈 LTSC 제품을 대상으로 품질 업데이트가 출시되지만 설치 여부는 사용자가 선택할 수 있다. LTSC는 절대로 일반 사용자가 사용하는 범용 장치에 배포되도록 설계되지 않았으며, ATM이나 의료기기와 같은 특수 장치를 위해서만 사용되어야 한다.

    > 범용 및 특수 장치를 구분하는 가장 일반적인 방법으로 최신 빌드가 권장되는 [마이크로소프트 365](https://www.microsoft.com/Microsoft-365) 제품 설치 여부로 판단한다. 

    만일 기능 업데이트가 적용된 상위 빌드 또는 채널을 GA로 변경하려면 인플레이스 업그레이드가 유일한 방안이다. 이전에는 장기 서비스 브랜치(Long-term Servicing Branch; LTSB)로 불렸으며, 일부 제품은 현재까지도 LTSB로 언급되어 사용되고 있다(예를 들어, [윈도우 2016 LTSB](https://learn.microsoft.com/en-us/lifecycle/products/windows-10-2016-ltsb) 등).

* **[윈도우 참가자 프로그램](https://learn.microsoft.com/ko-kr/windows/deployment/update/waas-overview#long-term-servicing-channel)**(Windows Insider Program)

    GA로 출시되기 전에 미리 기능 업데이트를 체험할 수 있는 채널이다. 참가자들은 해당 기능 업데이트가 GA에 출시되기 전에 마이크로소프트에 이슈를 보고하는 등의 피드백으로 기여할 수 있다.
