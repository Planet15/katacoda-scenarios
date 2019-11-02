
## Task

Working directory (데모에서는 terraform) 생성 및 Terraform Provider(ncloud.tf) 및 리소스 정의 파일(infra.tf), user-data 파일 생성을 진행 합니다.

`ncloud.tf`{{open}}
`infra.tf`{{open}}
`user-data.sh`{{open}}

<pre class="file" data-filename="user-data.sh" data-target="replace">
#!/bin/bash
yum install -y httpd
/etc/init.d/httpd start
echo “NCP SERVER-$HOSTNAME” > /var/www/html/index.html
</pre>

Provider 설정 파일(ncloud.tf)에 ncloud 선언 및 설치(init)을 진행합니다.


<pre class="file" data-filename="nclolud.tf" data-target="replace">
provider “ncloud” {
access_key = “ACCESS KEY”
secret_key = “SECET KEY”
region = “Region”
}
</pre>

`terraform init`{{execute}} 
`terraform version`{{execute}}
