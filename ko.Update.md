---
category: 윈도우
title: 업데이트
---
# WaaS
[Windows as a Service](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview), 일명 WaaS는 [윈도우 NT](ko.Windows.md)의 새로운 버전이 출시되기까지 대략 3-5년 동안 업데이트를 기다려야 하는 방식과 달리, 소규모 기능 업데이트를 주기적으로 빈번하게 제공하는 방식을 일컫는다. WaaS로 배포된 업데이트는 한번에 많은 변화를 적용시키지 않아 사용자가 느끼는 피로감을 줄이는 효과가 있다. [윈도우 10 버전](https://ko.wikipedia.org/wiki/윈도우_10_버전_역사)의 경우, [1507](https://en.wikipedia.org/wiki/Windows_10_(original_release))으로 처음 출시되어 마지막 버전인 [22H2](https://en.wikipedia.org/wiki/Windows_10_version_history#Version_22H2_(2022_Update))까지 배포되었다.

아래는 윈도우 10의 WaaS [업데이트 유형](https://learn.microsoft.com/en-us/windows/deployment/update/get-started-updates-channels-tools#types-of-updates) 중 대표적인 두 가지를 소개한다:

* **[기능 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#annual-feature-updates)**(Feature updates)

    연간 하반기에 새로운 기능을 OS에 추가하는 업데이트이며, 기능 업데이트가 설치되면 윈도우 OS가 상위버전으로 올라가기 때문에 흔히 *In-Place 업그레이드*라고 언급된다. 해당 업데이트는 사용자의 모든 어플리케이션, 설정, 그리고 데이터를 유지할 수 있다.

    > 이전에는 반기마다, 즉 3월과 9월을 기점으로 출시되었으나 [21H2](https://en.wikipedia.org/wiki/Windows_11_version_history#Version_21H2_(original_release))부터 [변경](https://blogs.windows.com/windowsexperience/2021/11/16/how-to-get-the-windows-10-november-2021-update/)되었다.

* **품질 업데이트**(Quality updates)

    해당 버전의 윈도우 OS에서 발견된 문제나 성능 및 보안을 개선하는 등 품질 관리를 위한 업데이트이다. 이전 품질 업데이트를 모두 내포하기 때문에 *누적 업데이트*라고 부른다. 품질 업데이트는 아래와 같이 보안 및 비보안 업데이트가 존재한다:

    <table style="width: 95%; margin: auto;"><caption style="caption-side: top;">품질 업데이트 구성</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#monthly-security-update-release">월별 보안 업데이트</a></th><th style="text-align: center"><a href="https://learn.microsoft.com/en-us/windows/deployment/update/release-cycle#optional-non-security-preview-release">선택적 비보안 미리 보기</a></th></tr></thead><tbody><tr style="text-align: center;"><td>두 번째 화요일 오전 10시 PST</td><td>네 번째 화요일 오전 10시 PST</td></tr><tr style="text-align: center;"><td>LCU (Latest Cumulative Update)</td><td>LCU Preview</td></tr><tr><td>안정성 수정 및 보안 취약성 해결; 대부분 기업 또는 조직에서는 의무적으로 설치.</td><td>다음 월별 보안 업데이트 이전에 안정성 패치를 미리 적용하여 유효성 확인 가능.</td></tr></tbody></table>

    이전 윈도우 OS 버전의 월별 업데이트는 개수가 상당하여 관심있는 업데이트만 선택적으로 설치하는 경우가 다반수였다. 이러한 행위는 마이크로소프트도 헤아릴 수 없는 수많은 업데이트 조합, 일명 "무한한 파편화(infinite fragmentation)"를 초래하여 업데이트 여부에 의한 호환성 문제 및 비정상 시스템 증상을 유발하였다. 품질 업데이트는 파편화를 방지하기 위해 이전 업데이트를 전부 포함시켜 누락된 게 없도록 조치하였다.

* **[서비스 스택 업데이트](https://learn.microsoft.com/en-us/windows/deployment/update/servicing-stack-updates)**(Servicing stack update; SSU)

* **드라이버 업데이트**(Driver updates)

* **마이크로소프트 제품 업데이트**(Microsoft product updates)
