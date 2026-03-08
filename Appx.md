# APPX
[**APPX**](https://learn.microsoft.com/windows/win32/appxpkg/appx-portal)는 마이크로소프트에서 개발한 어플리케이션 패키징 형식이며, [ZIP 압축 형식](https://en.wikipedia.org/wiki/ZIP_(file_format))을 빌려 [OPC](https://en.wikipedia.org/wiki/Open_Packaging_Conventions) 컨테이너 파일 기술을 활용해 어플리케이션 설치 및 실행에 필요한 리소스들을 단일 .appx 혹은 .appxbundle 확장자 파일로 구현한다. 특히 APPX는 다음 특징 덕분에 어플리케이션 배포에 편의성을 제공한다.

* 여러 패키지를 하나의 번들로 묶어, 하나의 파일만으로 장치에 가장 적합한 버전으로 배포할 수 있다.

현재는 APPX의 특징들을 그대로 가지며 지원 프로그램이 확장된 상위호환의 [MSIX](#msix)로 대체되고 있다. 다만, 어플리케이션 패키징 기술 그 자체를 논할 때에는 [Get-AppxPackage](https://learn.microsoft.com/powershell/module/appx/get-appxpackage)와 같이 APPX를 특정하여 부르기도 한다.

### MSIX
[**MSIX**](https://learn.microsoft.com/windows/msix/overview)는 전통적인 [Win32 데스크탑 어플리케이션](https://learn.microsoft.com/windows/win32/desktop-programming)도 함께 지원하는 APPX의 확장판으로 설치 파일인 [.msi](https://learn.microsoft.com/windows/win32/msi/windows-installer-portal) 확장자에서 이름을 딴 어플리케이션 패키징 형식이다. APPX와 유사하게 .msix 혹은 .msixbundle 확장자 파일을 가진다.
