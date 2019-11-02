
## Task

Working directory (데모에서는 terraform) 생성 및 Terraform Provider(ncloud.tf) 및 리소스 정의 파일(infra.tf), user-data 파일 생성을 진행 합니다.

`mkdir /root/terraform`{{execute}}

`cd /root/terraform && touch ncloud.tf infra.tf user-data.sh`{{execute}}

<pre>
"files": [
    "ncloud.tf", "infra.tf", "user-data.sh"
]
</pre>

`vim user-data.sh`{{execute}}
`i`{{execute}}

<pre class="file" data-filename="user-data.sh" data-target="replace">
#!/bin/bash
yum install -y httpd
/etc/init.d/httpd start
echo “NCP SERVER-$HOSTNAME” > /var/www/html/index.html
</pre>

`:wq`{{execute}}

Provider 설정 파일(ncloud.tf)에 ncloud 선언 및 설치(init)을 진행합니다.

`vi ncloud.tf`{{execute}}

`i`{{execute}}

<pre class="file" data-filename="infra.tf" data-target="replace">
provider “ncloud” {
access_key = “ACCESS KEY”
secret_key = “SECET KEY”
region = “Region”
}
</pre>

`:wq`{{execute}}

`terraform init && terraform version`{{execute}}
