# 디자인 도구 '펜팟(Penpot)' 설치부터 활용까지

안녕하세요?<br>
글루시스에 웹개발팀에서 근무하고있는 이현태입니다.<br>
프로젝트 개발과정중 애플리케이션 또는 웹페이지의 레이아웃과 요소를 보여주기 위해 사용되는 디자인의 시각적 표현 또는 프로토타입을 목업이라합니다. <br>
목업은 개발로 넘어가기 전에 애플리케이션 또는 웹사이트의 제품이나 디자인을 시각적으로 표현하는 것이므로 
이러한 디자인작업을 하는 도구인 '펜팟(Penpot)'을 소개하고자 합니다.<br>
펜팟은 무료 오픈소스로, Figma와 유사한 기능을 제공하며, 사용자들 사이에서 Figma보다 더 흥미로운 도구로 평가되기도 합니다. <br>
디자인 도구로써 다양한 기능을 제공하며, 디자인 프로젝트를 효율적으로 관리할수 있습니다.<br>
저는 완전한 제어와 보안을 위해 툴킷을 내부적으로 호스팅하는 것을 선호기때문에 펜팟을 자체적으로 호스팅하고 사용하면서 느꼈던 장단접들과 활용 방법을 소개하도록 하겠습니다.

## 셀프호스팅
### Docker로 설치
penpot은 Elestio와 Docker로 설치가 가능합니다.<br>
저는 윈도우(Window)환경에서 개발을하기때문에 도커 데스크탑(Docker Desktop)에서 WSL2(Linux용 Windows 하위 시스템, 버전 2)를 사용하도록 설정하여 Linux 컨테이너를 실행하는 환경에서 설치를하였습니다.<br>
WSL2에서 별도로 Docker 인스턴스를 설치하고 실행할수 있지만, 저는 초보자이기 때문에 Docker와 관련된 모든 설정을 수동으로 해야하며 도커 데스크탑의 GUI를 통하여 관리하는 기능보다 부족하고 모든 작업을 CLI를 통해 작업할 자신이없었습니다.<br>
그러므로 Windows용 Docker Desktop에서 지원되는 WSL2를 사용하여 Linux기반 개발 환경에서 작업을 해야하기때문에 WSL2 활성화 후에 Windows에 Linux를 설치하는 방법을 알아보겠습니다.<br>

| **항목** | **설명** |
| :---: | :--- |
| **Docker** | 소프트웨어를 실행하는 데 필요한 모든 파일, 라이브러리, 설정 파일 등을 포함하는 패키지를 컨테이너라하는데, <br>컨테이너 기반의 오픈소스 가상화 시스템이다.|
| **Docker Desktop** | Windows 및 macOS 운영 체제에서 Docker를 쉽게 설치하고 사용할 수 있게 해주는 애플리케이션 |
| **WSL** | Windows 운영 체제에서 Linux 커널을 실행할 수 있게 해주는 기능 |


#### wls2 활성화후 Windows에 Linux를 설치
1. window - PowerShell - 관리자로 실행을 시켜줍니다.
<img src= "/assets/images/penpot/images_1.JPG">

2. Powershell
    * WSL 기능 활성화
    ```
    wsl --install
    ```
    * 시스템 재부팅
    ```
    Restart-Computer
    ```
    * WSL 버전을 WSL2로 설정
    ```
    wsl --set-default-version 2
    ```
    * 사용 가능한 배포판 목록 확인
    ```
     wsl --list --online
    ```
    * 원하는 Linux 배포판 설치
    ```
    wsl --install -d Ubuntu
    ```
    * WSL에 설치된 배보판 목록 
    ```
    wsl --list --verbose
    ```
    * 설치한 Ubuntu 배포판 실행
    ```
    wsl
    ```

#### Docker Desktop설치 및 wls2 설정
1. [docker desktop](https://www.docker.com/products/docker-desktop/) 공식홈페이지에서 [Download for Windows]를 클릭하여 다운로드하고 설치합니다.
<img src="/assets/images/penpot/iamges_2.JPG">

2. Docker Desktop 실행후 작업 표시줄의 숨겨진 아이콘 메뉴에서 Docker 아이콘을 선택합니다. 아이콘을 마우스 오른쪽 단추로 클릭하여 Docker 명령 메뉴를 표시하고 "설정"을 선택합니다.
<img src="/assets/images/penpot/iamges_3.JPG">

3. 설정>일반에서 "WSL 2 기반 엔진 사용"이 선택되어 있는지 확인합니다.
<img src="/assets/images/penpot/iamges_4.JPG">

4. 설정>리소스>WSL 통합으로 이동하여 Docker 통합을 사용하도록 설정하려는 설치된 WSL 2 배포판에서 선택합니다.
<img src="/assets/images/penpot/images_5.JPG"></img>

5. Docker가 설치되었는지 확인하려면 PowerShell에서 확인을 합니다.
    ```
    docker --version
    ```
6. Docker 이미지를 실행하여 올바르게 설치되었는지 테스트하기위해 PowerShell에서 확인을 합니다.
     ```
    docker run hello-world
    ```

#### [penpot 패치후 시작하기](https://help.penpot.app/technical-guide/getting-started/#start-penpot)
Penpot은 Docker compose(v2)라는 Docker 컨테이너들을 정의하고 실행하기 위한 도구를 통해 Penpot 저장소에서 'docker-compose.yaml'파일을 다운로드하여 실행해야합니다.<br>
그러러면 Docker compose(v2)이라는 플러그인도 설치가 필요하겠죠?? wsl에서 docker 엔진으로 설치했더라면, Docker compose도 따로 명령어를 통해 설치하고 복잡한 과정을 걸처야합니다.<br>
그치만 Docker Desktop을 통해서 따로 설치할 필요없이 배포되어있기때문에 명령어를 'docker-compose.yaml'파일만 다운로드 하면 됩니다.<br>

1. 설치한 Ubuntu에서 'docker-compose.yaml'파일을 다운로드
    * window의 WSL2에서 설치한 Ubuntu를 실행하여 다운로드합니다.
    * docker-compose.yaml를 설치할 파일경로는 따로 신경쓰실필요없어요~!
    * wget:인터넷에서 파일을 다운로드할 때 사용되는 명령줄 도구
    ```
   wget https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml
    ```
    <img src="/assets/images/penpot/iamges_6.JPG">

    * 실질적으로 ubuntu의 파일경로는 window에서 가상시스템을 뛰운거기때문에 파일탐색기를 열어보면 확인하실수있습니다.(Ubuntu 실행명령어에서 'll'을 쳤을때 실제 경로예요~!)
    <img src="/assets/images/penpot/iamges_7.JPG">

2. penpot 서비스 실행하기 
    * 항상 docker compose를 통하여 명령어(cli)를 실행할땐 'Docker Desktop'파일이 실행되고있어야합니다.
    * Ubuntu에서 명령어를 펜팟 서비스를 실행시키는 명령어를 실행시키고, http://localhost:9001 에 접속을합니다 
    * 포트 '9001'은 wget으로 설치한 'docker-compose.yaml'파일에 기본으로 설정되어있고, 변경해서 사용할수 있어요~!
    ```
   docker compose -p penpot -f docker-compose.yaml up -d
    ```
    <img src="/assets/images/penpot/iamges_8.JPG">
3. penpot 서비스 실행 중지
    ```
   docker compose -p penpot -f docker-compose.yaml down
    ```