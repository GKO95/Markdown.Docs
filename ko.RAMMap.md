---
category: 마이크로소프트
title: RAMMap
alias: null
visible: true
icon: sysinternals.png
---
# RAMMap
[RAMMap](https://learn.microsoft.com/en-us/sysinternals/downloads/rammap)는 시스템이 [물리 메모리](ko.Memory.md), 즉 [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리)을 어떻게 활용하고 있는지 분석하는 [Sysinternals](ko.Sysinternals.md) 유틸리티 프로그램이다. 일반 사용자들에게는 흔히 "RAM 정리 프로그램"으로 알려져 있으나, 이는 RAMMap에서 제공하는 기능 중 하나인 Empty Working Set으로부터 비롯된 오해이다.

![RAMMap 유틸리티 프로그램](./images/sysinternals_rammap.png)

> 본 프로그램을 이해하려면 [윈도우 NT](ko.WindowsNT.md) 운영체제를 [컴퓨터 구조론](https://ko.wikipedia.org/wiki/컴퓨터_구조) 관점에서의 개념이 확립되어야 한다.

RAMMap 유틸리티로부터 사용자는 윈도우 운영체제가 메모리를 관리하는 방식을 이해, 어플리케이션의 메모리 점유 분석, 그리고 RAM 할당에 대한 의문점을 답해 줄 수 있다. 아래는 RAMMap에서 표시하는 각 메모리 용도들을 나열하고 설명한다:

* [페이징 풀](ko.Memory.md#메모리-풀)
* [비페이징 풀](ko.Memory.md#메모리-풀)
* [Driver Locked](ko.Memory.md#driver-locked-메모리)
