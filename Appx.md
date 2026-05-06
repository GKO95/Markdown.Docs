# APPX
> 상위호환의 [MSIX](#msix) 형식이 더 보편화되었으나, 패키징 기술 자체를 논할 때는 APPX를 지칭하여 설명한다.

[**APPX**](https://learn.microsoft.com/windows/win32/appxpkg/appx-portal)는 마이크로소프트에서 개발한 어플리케이션 패키징 형식이며, [ZIP 압축 형식](https://en.wikipedia.org/wiki/ZIP_(file_format))을 빌려 [OPC](https://en.wikipedia.org/wiki/Open_Packaging_Conventions) 컨테이너 파일 기술을 활용해 어플리케이션 설치 및 실행에 필요한 리소스들을 단일 파일로 구현한다. 특히 APPX는 다음 특징 덕분에 어플리케이션 배포에 편의성을 제공한다.

1. 아키텍처에 따라 달리 패키징된 .appx들을 하나의 번들인 .appxbundle로 묶어 배포할 수 있다.
1. 업데이트 시 변경된 64 KB 블록만 다운로드 받기 때문에 제한된 대역폭에서도 적합하다.
1. 패키지는 손상된 파일 설치 및 무단 변경을 방지하기 위한 서명을 포함한다.

Windows 8에서 최초 등장한 WinRT 기반 어플리케이션의 패키징으로 APPX가 처음으로 소개되으며, 이후 Windows 10에서는 WinRT의 어플리케이션 모델인 [UWP](https://learn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) 기본 패키지로 채택되었다.

### MSIX
[**MSIX**](https://learn.microsoft.com/windows/msix/overview)는 [Win32](WinAPI.md) 기반의 레거시 [데스크탑 어플리케이션](https://learn.microsoft.com/windows/win32/desktop-programming)([MFC](https://learn.microsoft.com/cpp/mfc/mfc-desktop-applications), [WinForms](https://learn.microsoft.com/dotnet/desktop/winforms/overview/), [WPF](https://learn.microsoft.com/dotnet/desktop/wpf/overview/) 포함)의 패키징도 함께 지원하는 APPX 확장판이다. 즉, WinRT에 한정된 기존 APPX 패키징 기술에 Win32 지원이 본격적으로 확장된 게 MSIX이다. 패키지 확장자는 .msix 혹은 .msixbundle이며 기존 [Windows Standard Installer](https://learn.microsoft.com/windows/win32/msi/windows-installer-portal)의 .msi 설치 파일 확장자 명칭에서 비롯되었다.

아래 구조적 변경은 Win32 데스크탑 어플리케이션 패키징을 지원할 수 있도록 한다.

<table style="width: 80%; margin-left: auto; margin-right: auto;"><caption style="caption-side: top;">APPX와 <a href="https://learn.microsoft.com/windows/msix/msix-containerization-overview">MSIX 컨테이너 모델</a> 비교</caption><colgroup><col style="width: 50%;"/><col style="width: 50%;"/></colgroup><thead><tr><th style="text-align: center;">APPX</th><th style="text-align: center;">MSIX</th></tr></thead><tbody><tr style="text-align: center;"><td>통제된 WinRT 환경만을 고려</td><td>WinRT + Win32 환경 모두 고려</td></tr><tr><td>오로지 하나의 <a href="https://learn.microsoft.com/windows/win32/secauthz/appcontainer-isolation">AppContainer</a> 신뢰 수준만으로 패키징된다.</td><td><ul><li>Full trust: Win32 & WinUI 3 데스크탑 앱</li><li>AppContainer: UWP 앱</li></ul></td></tr><tr><td>엄격한 <a href="https://en.wikipedia.org/wiki/Sandbox_(computer_security)">샌드박스</a> 환경으로 <a href="Registry.md">레지스트리</a>와 임의의 <a href="FileSystem.md">파일 시스템</a> 접근이 제한된다.</td><td><a href="https://en.wikipedia.org/wiki/Virtualization">가상화</a>를 통해 <a href="Registry.md">레지스트리</a>와 임의의 <a href="FileSystem.md">파일 시스템</a>을 간접적으로 접근을 허용한다.</td></tr></tbody></table>

MSIX의 가상화는 패키징된 어플리케이션이 본래 행하던 OS 레벨의 레지스트리 혹은 파일 시스템 편집이 실제로는 각 사용자의 패키지 폴더에 반영되도록 한다: APPX의 앱 데이터가 위치하는 %LocalAppData%\Packages 디렉토리가 MSIX에게는 가상화된 상태가 저장되는 공간이다.

Hyper-V 관리자는 "Windows 10 MSIX packaging environment" 옵션의 가상 머신 셋업을 제공한다. 기존 Win32 어플리케이션을 해당 가상 머신에 설치하면서 변경된 레지스트리 및 파일 시스템을 포착하여 가상화하는 작업으로 MSIX 패키징이 이루어진다.
