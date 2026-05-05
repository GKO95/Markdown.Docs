# APPX
> 상위호환의 [MSIX](#msix) 형식이 더 보편화되었으나, 패키징 기술 자체를 논할 때는 APPX를 지칭하여 설명한다.

[**APPX**](https://learn.microsoft.com/windows/win32/appxpkg/appx-portal)는 마이크로소프트에서 개발한 어플리케이션 패키징 형식이며, [ZIP 압축 형식](https://en.wikipedia.org/wiki/ZIP_(file_format))을 빌려 [OPC](https://en.wikipedia.org/wiki/Open_Packaging_Conventions) 컨테이너 파일 기술을 활용해 어플리케이션 설치 및 실행에 필요한 리소스들을 단일 파일로 구현한다. 특히 APPX는 다음 특징 덕분에 어플리케이션 배포에 편의성을 제공한다.

* 아키텍처에 따라 달리 패키징된 .appx들을 하나의 번들인 .appxbundle로 묶어 배포할 수 있다.
* 업데이트 시 변경된 64 KB 블록만 다운로드 받기 때문에 제한된 대역폭에서도 적합하다.
* 패키지는 손상된 파일 설치 및 무단 변경을 방지하기 위한 서명을 포함한다.

Windows 8에서 최초 등장한 WinRT 기반 어플리케이션의 패키징으로 APPX가 처음으로 소개되으며, 이후 Windows 10에서는 WinRT의 어플리케이션 모델인 [UWP](https://learn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) 기본 패키지로 채택되었다.

### MSIX
[**MSIX**](https://learn.microsoft.com/windows/msix/overview)는 [Win32](https://learn.microsoft.com/windows/win32/desktop-programming) 기반의 레거시 데스크탑 어플리케이션([MFC](https://learn.microsoft.com/cpp/mfc/mfc-desktop-applications), [WinForms](https://learn.microsoft.com/dotnet/desktop/winforms/overview/), [WPF](https://learn.microsoft.com/dotnet/desktop/wpf/overview/) 등)의 패키징도 함께 지원하는 APPX 확장판이다. 패키지 확장자는 .msix 혹은 .msixbundle이며 기존 [Windows Standard Installer](https://learn.microsoft.com/windows/win32/msi/windows-installer-portal)의 .msi 설치 파일 확장자에서 비롯되었다.
