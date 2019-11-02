
## Task

Working directory (데모에서는 terraform) 생성 및 Terraform Provider(ncloud.tf) 및 리소스 정의 파일(infra.tf), user-data 파일 생성을 진행 합니다.

Provider 설정 파일 생성 합니다.
`/terraform/ncloud.tf`{{open}}

리소스 정의 파일을 생성 합니다.
`/terraform/infra.tf`{{open}}

User-data 파일 을 생성 합니다.
`/terraform/user-data.sh`{{open}}

user-data에는 간단히 httpd를 설치 하여, 웹서비스가 구동 되는 것을 구성합니다.
<pre class="file" data-filename="user-data.sh" data-target="replace">
#!/bin/bash
yum install -y httpd
/etc/init.d/httpd start
echo “NCP SERVER-$HOSTNAME” > /var/www/html/index.html
</pre>

Provider 설정 파일(ncloud.tf)에 ncloud 선언을 합니다.
여기서 ncloud에서 부여 받은 ACCESS KEY 와 SECET KEY를 입력 합니다.

ncloud에서 부여 받은 ACCESS KEY 와 SECET KEY 확인 하는 방법은 아래의 사이트를 참고 합니다.

<pre class="file" data-filename="ncloud.tf" data-target="replace">
provider “ncloud” {
access_key = “ACCESS KEY”
secret_key = “SECET KEY”
region = “Region”
}
</pre>

terraform에 대한 Provider의 초기화를 진행 합니다.
`terraform init`{{execute}} 
