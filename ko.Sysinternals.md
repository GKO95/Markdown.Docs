---
category: 소프트웨어
title: Sysinternals
---
# Sysinternals
[Sysinternals](https://aka.ms/sysinternals)는 [Mark Russinovich](https://ko.wikipedia.org/wiki/마크_러시노비치) [소프트웨어 엔지니어](https://ko.wikipedia.org/wiki/소프트웨어_공학자)가 1996년부터 호스팅한 웹사이트이며 [윈도우](ko.Windows.md) 및 어플리케이션을 관리, 검사, 그리고 트러블슈팅 하는데 유용한 리소스 및 유틸리티 프로그램을 소개한다. 지금도 꾸준히 사용되고 있는 Sysinternals는 분야와 목적에 따라 70개가 넘는 개별 프로그램들이 존재한다.

> Sysinternals [소프트웨어 라이선스 약관](https://learn.microsoft.com/en-us/sysinternals/license-terms)에 명시된 바로는 [마이크로소프트 기술지원](https://support.microsoft.com/) 대상이 아니므로 유의해서 사용하도록 합니다!

## 다운로드
Sysinternals는 필요한 개별 프로그램만 [다운로드](https://learn.microsoft.com/en-us/sysinternals/downloads/)하여 설치없이 곧바로 실행하여 사용할 수 있다. 외부 인터넷망과 분리되어 있거나 타 프로그램을 함부로 설치하면 안되는 기업용 컴퓨터 혹은 서버에서도 원활한 접근성을 제공한다. 각 유틸리티는 독립적으로 사용될 수 있어 Sysinternals에서 제공하는 모든 프로그램을 설치할 필요가 없다. 더 이상 필요하지 않는 유틸리티는 단순히 삭제하여 제거할 수 있다.

### Sysinternals Suite
[Sysinternals Suite](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)는 여러 70개가 넘는 유틸리티 프로그램을 한꺼번에 제공한다. 비록 압축파일에 불과하지만, 이는 위에서 언급한 접근성을 고려한 결정이다. 단, 일반 클라이언트 OS 사용자들은 2021년 10월에 공개된 [마이크로소프트 스토어](https://apps.microsoft.com/store/detail/sysinternals-suite/9P7KNL5RWT25) 버전을 설치하는 게 관리하는 면에서 더 편리할 것이다.

### Sysinternals Live
[Sysinternals Live](https://live.sysinternals.com/)는 원활한 인터넷 접속만 유지된다면 다운로드 없이 유틸리티를 사용할 수 있도록 한다. [명령 프롬프트](https://ko.wikipedia.org/wiki/Cmd.exe)에 다음 명령어를 입력하여 실행한다.

```terminal
\\live.sysinternals.com\tools\<프로그램명>.exe
```

## 유틸리티 목록
다음은 시스템 구성요소 대상에 따라 Sysinternals 유틸리티 프로그램들을 분류한 목록이다.

<table style="table-layout: fixed; width: 100%; margin: auto;">
<caption style="caption-side: top;">Sysinternals 구성 유틸리티 프로그램</caption>
<thead><tr><th style="text-align: center;">파일 및 디스크</th><th style="text-align: center;"><a href="ko.Network.md">네트워크</a></th><th style="text-align: center;"><a href="ko.Process.md">프로세스</a></th><th style="text-align: center;">보안</th><th style="text-align: center;">시스템 정보</th><th style="text-align: center;">기타</th></tr></thead>
<tbody>
<tr style="vertical-align: top; overflow-wrap: break-word;">
<td><ul><li>AccessChk</li><li>AccessEnum</li><li>CacheSet</li><li>Contig</li><li>Disk2vhd</li><li>DiskExt</li><li>DiskMon</li><li>DiskView</li><li>Disk Usage</li><li>EFSDump</li><li>FindLinks</li><li>LDMDump</li><li>MoveFile</li><li>NTFSInfo</li><li>PendMoves</li><li>SDelete</li><li>Sigcheck</li><li>Streams</li><li>Sync</li><li>VolumeID</li></ul></td>
<td><ul><li>AdExplorer</li><li>AdInsight</li><li>AdRestore</li><li>PipeList</li><li>PsFile</li><li>PsPing</li><li>ShareEnum</li><li>TCPView</li><li>WhoIs</li></ul></td>
<td><ul><li>Autoruns</li><li>Handle</li><li>ListDLLs</li><li>PortMon</li><li><a href="ko.ProcDump.md">ProcDump</a></li><li><a href="ko.Process_Explorer.md">Process Explorer</a></li><li><a href="ko.Process_Monitor.md">Process Monitor</a></li><li>PsExec</li><li>PsGetSid</li><li>PsKill</li><li>PsList</li><li>PsService</li><li>PsSuspend</li><li>PsTools</li><li>ShellRunas</li><li><a href="ko.VMMap.md">VMMap</a></li></ul></td>
<td><ul><li>Autologon</li><li>LogonSessions</li><li>PsLoggedOn</li><li>PsLogList</li><li>Sysmon</li></ul></td>
<td><ul><li>ClockRes</li><li>Coreinfo</li><li>LiveKD</li><li>LoadOrder</li><li>PsInfo</li><li><a href="ko.RAMMap.md">RAMMap</a></li><li>WinObj</li></ul></td>
<td><ul><li>BgInfo</li><li>BlueScreen</li><li>CpuStres</li><li>Ctrl2Cap</li><li>DebugView</li><li>Desktops</li><li><a href="ko.Hex2dec.md">Hex2dec</a></li><li>Junction</li><li><a href="ko.NotMyFault.md">NotMyFault</a></li><li>PsPasswd</li><li>PsShutdown</li><li>RDCMan</li><li>RegDelNull</li><li>RegHide</li><li>RegJump</li><li>Registry Usage</li><li>Strings</li><li>Testlimit</li><li>ZoomIt</li></ul></td>
</tr>
</tbody>
</table>

# 같이 보기
* [Sysinternals - Windows Sysinternals &#124; Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/)
