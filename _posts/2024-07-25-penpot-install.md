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
![images_1.JPG](/assets/images/penpot/images_1.JPG)

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
![images_2.JPG](/assets/images/penpot/images_2.JPG)

2. Docker Desktop 실행후 작업 표시줄의 숨겨진 아이콘 메뉴에서 Docker 아이콘을 선택합니다. 아이콘을 마우스 오른쪽 단추로 클릭하여 Docker 명령 메뉴를 표시하고 "설정"을 선택합니다.
![images_3.JPG](/assets/images/penpot/images_3.JPG)

3. 설정>일반에서 "WSL 2 기반 엔진 사용"이 선택되어 있는지 확인합니다.
![images_4.JPG](/assets/images/penpot/images_4.JPG)

4. 설정>리소스>WSL 통합으로 이동하여 Docker 통합을 사용하도록 설정하려는 설치된 WSL 2 배포판에서 선택합니다.
![images_5.JPG](/assets/images/penpot/images_5.JPG)

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
    ![images_6.JPG](/assets/images/penpot/images_6.JPG)

    * 실질적으로 ubuntu의 파일경로는 window에서 가상시스템을 뛰운거기때문에 파일탐색기를 열어보면 확인하실수있습니다.(Ubuntu 실행명령어에서 'll'을 쳤을때 실제 경로예요~!)
    ![images_7.JPG](/assets/images/penpot/images_7.JPG)

2. penpot 서비스 실행하기 
    * 항상 docker compose를 통하여 명령어(cli)를 실행할땐 'Docker Desktop'파일이 실행되고있어야합니다.
    * Ubuntu에서 명령어를 펜팟 서비스를 실행시키는 명령어를 실행시키고, http://localhost:9001 에 접속을합니다 
    * 포트 '9001'은 wget으로 설치한 'docker-compose.yaml'파일에 기본으로 설정되어있고, 변경해서 사용할수 있어요~!
    ```
   docker compose -p penpot -f docker-compose.yaml up -d
    ```
    ![images_8.JPG](/assets/images/penpot/images_8.JPG)
3. penpot 서비스 실행 중지
    ```
   docker compose -p penpot -f docker-compose.yaml down
    ```

### Penpot 구성하기
penpot의 서비스를 활성화할때는 'Docker Compose'를 사용하여 펜팟의 컨테이너의 이미지, 환경 변수, 볼륨, 네트워크 설정 등을 명시하는 파일인 'docker-compose.yaml'을 통해 실행시켜서 서비스를 활성화했습니다. <br>
[공식사이트](https://help.penpot.app/technical-guide/developer/) 가이드에 따라 penpot의 구성을 gitlab을 사용하여 인증을 구성하여 로그인할수 있게 옵션을 설정하여 사용하겠습니다.

#### docker-compose.yaml 파일 설정하기
저는 wget을 통해 'docker-compose.yaml'을 받았습니다. 파일경로는 '\\wsl.localhost\Ubuntu\home\gluesys' 하위에속해있구요. <br>
해당파일의 확장자는 'yaml'파일이기때문에 'docker-compose.yaml'파일의 속성에 들어가서 연결프로그램을 '메모장'으로 변경후 파일내용을 수정을 했습니다.

* 수정된 내용은 'penpot-frontend'와 'penpot-backend'입니다. 
    * 첫번째로 'penpot-frontend'밑에줄에보면 PENPOT_FLAGS라는 부분이있습니다. 여기서 'disable-secure-session-cookies disable-login enable-login-with-gitlab disable-registration'로 정의했고 
    * 두번째는 'penpot-backend'밑에줄에 PENPOT_FLAGS에다가 'disable-secure-session-cookies disable-login enable-login-with-gitlab' 로 정의했구요 
    * 세번째는 Gitlab에 인증관련된 내용 'PENPOT_GITLAB_BASE_URI, PENPOT_GITLAB_CLIENT_ID, PENPOT_GITLAB_CLIENT_SECRET'를 설정해주었습니다.

[docker-compose.yaml](/assets/images/penpot/docker-compose.yaml)

* 이렇게 옵션을 설정하였다 계정생성 버튼은 없어지고 '깃랩(GitLab)' 버튼이 생겼을겁니다.
![images_9.JPG](/assets/images/penpot/images_9.JPG)

#### 깃랩에 penpot 인증할 애플리케이션 생성후 docker-compose.yaml 인증 등록
1. 깃랩 로그인 - 사용자 설정 - 어플리케이션 - [새 애플리케이션 추가]
![images_10.JPG](/assets/images/penpot/images_10.JPG)
    * 이름은 : Penpot
    * Redirect URL은 : http://localhost/api/auth/oauth/gitlab/callback
    * 옵션 체크(선택) : read_user, openid, profile, email
    ![images_11.JPG](/assets/images/penpot/images_11.JPG)
    여기서 Redirect URL는 깃랩을통해 로그인후 파라미터로 접속할 URI입니다. <br>
    센스있는 사용자라면 'localhost'가아닌 자신의 펜팟설치한 IP로 기입하셔야한다는걸 눈치채셨을겁니다.<br>

2. 인증키 복사
* 애플리케이션 ID 와 비밀키를 복사를 합니다.
 ![images_12.JPG](/assets/images/penpot/images_12.JPG)
* 여기서 복사한 내용은 'docker-compose.yaml'파일에서 'PENPOT_GITLAB_CLIENT_ID, PENPOT_GITLAB_CLIENT_SECRET' 에 입력후 저장한후 penpot 서비스를 재실행합니다.

    ```
    # Backend & Frontend
    PENPOT_FLAGS="[...] enable-login-with-gitlab"

    # Backend only
    PENPOT_GITLAB_BASE_URI=https://gitlab.com
    PENPOT_GITLAB_CLIENT_ID=<client-id>
    PENPOT_GITLAB_CLIENT_SECRET=<client-secret>
    ```

* 마지막으로 서비스 접속후 - [깃랩(GitLab)] 클릭 - 인증 - 로그인하면 끝 ~!
![images_13.JPG](/assets/images/penpot/images_13.JPG)


## '펜팟(Penpot)' 활용하기
이제는 '펜팟(Penpot)'에서 제공하는 라이브러리를 다운받고 해당 디자인 템플릿을 활용해 간단한 로그인페이지의 목업작업을 하는 방법을 소개하도록 하겠습니다. <br>
라이브러리를 다운받고, 라이브러리안에 마음에 드는 템플릿을 공유해 개인 프로젝트에서 해당 디자인을 활용해서 작업을 할 것입니다. <br>
[펜팟에서 제공되는 라이브러리](https://penpot.app/libraries-templates) 중에서 저는 'Firefox mockup'와 'Penpot Design System' 이라는 라이브러리를 사용하겠습니다.

### [라이브러리](https://penpot.app/libraries-templates) 다운로드
1. 해당 홈페이지에서 'Firefox mockup'와 'Penpot Design System' 를 다운받습니다.
![images_14.JPG](/assets/images/penpot/images_14.JPG)
![images_15.JPG](/assets/images/penpot/images_15.JPG)

2. 다운로드한 라이브러리를 추가를 해줍니다.
* 로그인 - [초안] - 상단 왼쪽 설정 - [Import Penpot files]를 클릭해줍니다.
![images_16.JPG](/assets/images/penpot/images_16.JPG)
* 다운받은 라이브러리를 선택후 'Import'를 해줍니다.
 * 인내심을갖고 등록된후 허가까지해주셔야해요~!
![images_17.JPG](/assets/images/penpot/images_17.JPG)
![images_18.JPG](/assets/images/penpot/images_18.JPG)
![images_19.JPG](/assets/images/penpot/images_19.JPG)
* 해당 라이브러리가 추가가 되었다면 추가된 라이브러리가 나타날것입니다.
![images_20.JPG](/assets/images/penpot/images_20.JPG)

3. 디자인 템플릿을 공유하기
개인 프로젝트에서 사용하기위해 'Firefox mockup'에서 인터넷창 디자인을 선택해서 공유를 해보겠습니다.

* 'Firefox mockup'라이브러리를 선택합니다.(더블클릭을 해주셔야해요~!)
![images_21.JPG](/assets/images/penpot/images_21.JPG)
![images_22.JPG](/assets/images/penpot/images_22.JPG)

* [LAYERS] - [templates] - [light] 우클릭 - [Create component] 클릭을 해줍니다.
저는 하얀색을 좋아하기때문에 밝은색 인터넷창의 디자인을 선택할게요~!
![images_23.JPG](/assets/images/penpot/images_23.JPG)

* [ASSETS] - 좌측하단[COMPONENTS] 에서 방금 선택한 디자인이 추가가 되었는지 확인합니다.
![images_24.JPG](/assets/images/penpot/images_24.JPG)

자 이제는 로그인폼에 대한 디자인을 선택해서 공유를 해보겠습니다. 똑같은 방법이니 빠르게 넘어가셔도되요~! <br>

* 'Penpot Design System' 라이브러리를 선택합니다.
![images_25.JPG](/assets/images/penpot/images_25.JPG)
![images_26.JPG](/assets/images/penpot/images_26.JPG)

* [LAYERS] - [PAGES] - [Login] 우클릭 - [Create component] 클릭을 해줍니다.
![images_27.JPG](/assets/images/penpot/images_27.JPG)
* [ASSETS] - 좌측하단[COMPONENTS] 에서 방금 선택한 디자인이 추가가 되었는지 확인합니다.
![images_28.JPG](/assets/images/penpot/images_28.JPG)

* 대쉬보드 페이지 - [초안] - [해당 라이브러리 우클릭] - [공유 라이브러리로 추가하기] 클릭을 해줍니다.
![images_29.JPG](/assets/images/penpot/images_29.JPG)

4. 개인 프로젝트에서 공유로 설정한 디자인으로 목업작업
자 다시그러면 대시보드(매인페이지) 페이지로 돌아와 프로젝트를 생성해보겠습니다.
* [NEW PROJECT] 버튼 클릭후 생성된 프로젝트를 클릭해줍니다.(저는 'Login' 이라 할게요!)
![images_30.JPG](/assets/images/penpot/images_30.JPG)
* [ASSETS] - [LIBRARIES] - 'Firefox mockup'와 'Penpot Design System' 추가를 해줍니다.
![images_31.JPG](/assets/images/penpot/images_31.JPG)
* 공유된 라이브러리를 확인합니다.
![images_32.JPG](/assets/images/penpot/images_32.JPG)
* 'Firefox mockup' 라이브러리에 생성한 인터넷창의 디자인 컴포넌트를 가운데 화면으로 드래그해줍니다.
![images_33.JPG](/assets/images/penpot/images_33.JPG)
자 이러면 인터넷창의 디자인 UI 목업이 추가를 하였으니 penpot의 로그인 디자인도 입혀보겠습니다.
* 'Penpot Design System' 라이브러리에 생성한 로그인폼 디자인 컴포넌트를 가운데 화면으로 드래그해줍니다.
![images_34.JPG](/assets/images/penpot/images_34.JPG)

로그인폼 디자인과 인터넷창디자인의 규격이 안맞을텐데 이부분은 잘 사이즈맞춰서 해주셔야해요~!
* 로그인폼 목업작업
![images_35.JPG](/assets/images/penpot/images_35.JPG)

# 마치면서
이 글에서 다루지 못한 'pnepot'의 기능과 디자인작업에 대한 내용들이 많이 있습니다. <br>
