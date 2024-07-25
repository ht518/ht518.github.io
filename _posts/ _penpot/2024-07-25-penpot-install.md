# 디자인 도구 '펜팟(Penpot)' 설치부터 활용까지

안녕하세요?
글루시스에 웹개발팀에서 근무하고있는 이현태입니다.
프로젝트 개발과정중 애플리케이션 또는 웹페이지의 레이아웃과 요소를 보여주기 위해 사용되는 디자인의 시각적 표현 또는 프로토타입을 목업이라합니다. 
목업은 개발로 넘어가기 전에 애플리케이션 또는 웹사이트의 제품이나 디자인을 시각적으로 표현하는 것이므로 이러한 디자인작업을 하는 도구인 '펜팟(Penpot)'을 소개하고자 합니다.
펜팟은 무료 오픈소스로, Figma와 유사한 기능을 제공하며, 사용자들 사이에서 Figma보다 더 흥미로운 도구로 평가되기도 합니다. 디자인 도구로써 다양한 기능을 제공하며, 디자인 프로젝트를 효율적으로 관리할수 있습니다.
저는 완전한 제어와 보안을 위해 툴킷을 내부적으로 호스팅하는 것을 선호기때문에 펜팟을 자체적으로 호스팅하고 사용하면서 느꼈던 장단접들과 활용 방법을 소개하도록 하겠습니다.

## 셀프호스팅
### Docker로 설치
penpot은 Elestio와 Docker로 설치가 가능합니다.
저는 윈도우(Window)환경에서 개발을하기때문에 도커 데스크탑(Docker Desktop)에서 WSL2(Linux용 Windows 하위 시스템, 버전 2)를 사용하도록 설정하여 Linux 컨테이너를 실행하는 환경에서 설치를하였습니다.
WSL2에서 별도로 Docker 인스턴스를 설치하고 실행할수 있지만, 저는 초보자이기 때문에 Docker와 관련된 모든 설정을 수동으로 해야하며 GUI를 통한 관리 기능이 부족해 모든 작업을 CLI를 통해 작업할 자신이없었습니다.
우선 Windows용 Docker Desktop에서 지원되는 WSL2를 사용하여 Linux기반 개발 환경에서 작업을 해야하기때문에 WSL2 활성화 후에 Windows에 Linux를 설치하는 방법을 알아보겠습니다.

- 키워드
* Docker - 소프트웨어를 실행하는 데 필요한 모든 파일, 라이브러리, 설정 파일 등을 포함하는 패키지를 컨테이너라하는데, 컨테이너 기반의 오픈소스 가상화 시스템이다.
* Docker Desktop - Windows 및 macOS 운영 체제에서 Docker를 쉽게 설치하고 사용할 수 있게 해주는 애플리케이션
* WSL -  Windows 운영 체제에서 Linux 커널을 실행할 수 있게 해주는 기능

#### wls2 활성화후 Windows에 Linux를 설치
1. window - PowerShell - 관리자로 실행
2. Powershell
 - wsl --install // WSL 기능 활성화
 - Restart-Computer // 시스템 재부팅
 - wsl --set-default-version 2 // WSL 버전을 WSL2로 설정
 - wsl --list --online // 사용 가능한 배포판 목록 확인
 - wsl --install -d Ubuntu // 원하는 Linux 배포판 설치
 - wsl --list --verbose // WSL에 설치된 배보판 목록 
 - wsl // 설치한 Ubuntu 배포판 실행

#### docker 설치 및 wls2 설정
