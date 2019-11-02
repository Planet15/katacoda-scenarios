Terraform을 이용한 네이버 클라우드 플랫폼 인프라 생성하기 실습
이번 실습에서 Terraform을 이용해서 생성할 아키텍처 입니다.

![demo_scanario](/img/1.png)

두 대의 웹 서버를 서로 다른 Availability Zone에 생성하고, Public Load Balancer를 생성해서 두 대의 서버를 바인딩하는 간단한 데모입니다.

## Task

Working directory 생성 및 Terraform Provider 정의

Terraform은 단일 바이너리 파일로 배포되고 있어 간단하게 설치가 가능합니다.
Terraform 다운로드 페이지에서 실행 환경에 맞는 패키지 다운로드를 한 후 압축을 해제하고 별도의 추가 설치 없이 바로 Terraform을 사용할 수 있습니다
`curl -o terraform_0.11.8_linux_amd64.zip https://releases.hashicorp.com/terraform/0.11.8/terraform_0.11.8_linux_amd64.zip`{{execute}}

데모 환경에서 설치한 Terraform 버전은 0.11.8 입니다.

압축을 해제 하며, terraform 명령어를 /usr/bin 디렉토리로 이동합니다.
`unzip terraform_0.11.8_linux_amd64.zip && mv terraform /usr/bin/`{{execute}}

아래 명령어를 통해 terrafrom 버젼을 확인 합니다.
`terraform version`{{execute}}

Working directory (데모에서는 terraform) 생성 및 Terraform Provider(ncloud.tf) 및 리소스 정의 파일(infra.tf), user-data 파일 생성을 진행 합니다.

`mkdir /root/terraform`{{execute}}

`cd /root/terraform && touch ncloud.tf infra.tf user-data.sh`{{execute}}

`cat user-data.sh`{{execute}}

Provider 설정 파일(ncloud.tf)에 ncloud 선언 및 설치(init)을 진행합니다.

`vi ncloud.tf`{{execute}}
