Terraform을 이용한 네이버 클라우드 플랫폼 인프라 생성하기 실습이며, 
이번 실습에서 Terraform을 이용해서 생성할 아키텍처 입니다.

![logo](https://miro.medium.com/max/966/0*xc0mbOMyyCdrejpl.png)

![demo_scanario](/katacoda-scenarios/howto-teraform-ncloud/img/topology.png)

이번 실습을 통해 다음을 **달성** 할 수 있습니다.

- 두 대의 웹 서버를 서로 다른 가능한 리젼에 생성이 가능합니다.
- Public Load Balancer를 생성해서 두 대의 서버를 바인딩이 가능합니다.

## 관리 노드에 Terrafrom 설치 하기 

각 리눅스 클라이언트 마다 환경 구성이 다르기 때문에 아래 명령에서 수행될 명령어 중 unzip을 설치 합니다.

`yum install -y unzip`{{execute}}

Terraform은 단일 바이너리 파일로 배포되고 있어 간단하게 설치가 가능합니다.
Terraform 다운로드 페이지에서 실행 환경에 맞는 패키지 다운로드를 한 후 압축을 해제하고 
별도의 추가 설치 없이 바로 Terraform을 사용할 수 있습니다.

`curl -o terraform_0.11.8_linux_amd64.zip https://releases.hashicorp.com/terraform/0.11.8/terraform_0.11.8_linux_amd64.zip`{{execute}}

실습환경에서 설치한 Terraform 버전은 0.11.8 입니다.

다운 받은 terraform 압축 파일을을 해제 합니다. terraform 명령어를 /usr/bin 디렉토리로 이동합니다.

`unzip terraform_0.11.8_linux_amd64.zip`{{execute}}

terraform 명령어를 /usr/bin 디렉토리로 이동하며, 이후 어떠한 PATH에서 실행 될 수 있습니다.

`mv terraform /usr/bin/`{{execute}}

아래 명령어를 통해 terrafrom 버젼을 확인 합니다.

`terraform version`{{execute}}

