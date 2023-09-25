# 리눅스용 윈도우 서브시스템
리눅스용 윈도우 서브시스템(Windows Subsystem for Linux), 일명 [WSL](https://ko.wikipedia.org/wiki/리눅스용_윈도우_하위_시스템)은 아래에 서술된 기존 [POSIX](https://ko.wikipedia.org/wiki/POSIX) [환경 서브시스템](Subsystem.md)이 가진 기술적 문제들을 극복하기 위해 새롭게 설계된 서브시스템이다.

1. 어플리케이션이 POSIX 서브시스템에서 실행되어야 하는 걸 윈도우가 인지하기 위해 [PE 포맷](https://ko.wikipedia.org/wiki/PE_포맷)으로 새롭게 빌드되어야 한다.
2. 타 운영체제 프로그램을 윈도우 서브시스템으로 [래핑](https://ko.wikipedia.org/wiki/래퍼_라이브러리)(wrapping)하는 방식을 택하였으나, 알아채기 힘든 호환성 결함이 야기될 수 있다.

[윈도우 10](Windows.md) 모바일 운영체제에서 안드로이드 앱을 실행할 수 있도록 추진된 [아스토리아 프로젝트](https://en.wikipedia.org/wiki/Windows_10_Mobile#Project_Astoria)(이후 Windows Subsystem for Android로 소개)를 발판으로 POSIX/UNIX가 아닌 "순수" [리눅스](https://ko.wikipedia.org/wiki/리눅스) 어플리케이션을 지원하고자 [우분투](https://ko.wikipedia.org/wiki/우분투)(Ubuntu) 배포판을 개발한 [캐노니컬](https://ko.wikipedia.org/wiki/캐노니컬)과 합작한 결과물이다.

![WSL2로 설치된 <a href="https://ko.wikipedia.org/wiki/데비안">데비안</a> 배포판의 터미널과 파일 탐색기](./images/wsl_terminal_debian.png)

WSL은 두 종류의 아키텍처 설계가 있어 각각 장단점을 가진다. 자세한 내용은 [마이크로소프트 문서](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)를 참고한다.

<table style="table-layout: fixed; width: 95%; margin: auto;">
<caption style="caption-side: top;">WSL 버전 비교</caption>
<colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup>
<thead><tr><th style="text-align: center;">WSL1</th><th style="text-align: center;">WSL2</th></tr></thead>
<tbody style="text-align: center;"><tr><td>리눅스 시스템 서비스를 처리할 Pico 제공자(Pico provider)란 특수한 커널 모드 드라이버를 사용하지만, 완전한 호환성을 제공하지 못한다.</td><td>경량화된 가상머신을 사용하여 완전한 리눅스 커널 및 GUI 지원이 가능하나, 윈도우 NTFS 접근에는 다소 성능 저하가 나타난다.</td></tr></tbody>
</table>
