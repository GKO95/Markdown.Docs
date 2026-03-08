# APPX
> 상위호환의 [MSIX](#msix) 형식이 더 보편화되었으나, 패키징 기술 자체를 논할 때는 APPX를 지칭하여 설명한다.

[**APPX**](https://learn.microsoft.com/windows/win32/appxpkg/appx-portal)는 마이크로소프트에서 개발한 어플리케이션 패키징 형식이며, [ZIP 압축 형식](https://en.wikipedia.org/wiki/ZIP_(file_format))을 빌려 [OPC](https://en.wikipedia.org/wiki/Open_Packaging_Conventions) 컨테이너 파일 기술을 활용해 어플리케이션 설치 및 실행에 필요한 리소스들을 단일 파일로 구현한다. 특히 APPX는 다음 특징 덕분에 어플리케이션 배포에 편의성을 제공한다.

* 아키텍처에 따라 달리 패키징된 .appx들을 하나의 번들인 .appxbundle로 묶어 배포할 수 있다.
* 업데이트 시 변경된 64 KB 블록만 다운로드 받기 때문에 제한된 대역폭에서도 적합하다.
* 패키지는 손상된 파일 설치 및 무단 변경을 방지하기 위한 서명을 포함한다.

### MSIX
[**MSIX**](https://learn.microsoft.com/windows/msix/overview)는 전통적인 [Win32 데스크탑 어플리케이션](https://learn.microsoft.com/windows/win32/desktop-programming)도 함께 지원하는 APPX의 확장판으로 설치 파일인 [.msi](https://learn.microsoft.com/windows/win32/msi/windows-installer-portal) 확장자에서 이름을 딴 어플리케이션 패키징 형식이다. 즉, 기존 패키징 기술의 특징을 그대로 가지고 있는 동시에 지원하는 프로그램 유형도 많아져 APPX의 상위호환이다.
