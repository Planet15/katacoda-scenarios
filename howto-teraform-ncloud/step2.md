## infra.tf, user-data, infra.tf 파일 생성 후 구성하기

Working directory (데모에서는 terraform) 생성 및 Terraform Provider(ncloud.tf) 및 리소스 정의 파일(infra.tf), user-data 파일 생성을 진행 합니다.

Provider 설정 파일 생성 합니다.
`/terraform/ncloud.tf`{{open}}

User-data 파일 을 생성 합니다.
`/terraform/user-data.sh`{{open}}

user-data에는 간단히 httpd를 설치 하여, 웹서비스가 구동 되는 것을 구성합니다.
<pre class="file" data-filename="user-data.sh" data-target="replace">
#!/bin/bash
yum install -y httpd
/etc/init.d/httpd start
echo &#x22;NCP SERVER-$HOSTNAME&#x22; > /var/www/html/index.html
</pre>

Provider 설정 파일(ncloud.tf)에 ncloud 선언을 합니다.
여기서 ncloud에서 부여 받은 ACCESS KEY 와 SECET KEY를 입력 합니다.

ncloud에서 부여 받은 ACCESS KEY 와 SECET KEY 확인 하는 방법은 아래의 사이트를 참고 합니다.

[Naver Cloud Platform에서 API 인증키를 생성하는 절차를 설명한 그림](https://blog.naver.com/casong99/221600092081)

주의: 예제로 기입한 ACCESS KEY 와 SECET KEY 에 대해서 꼭 가지고 계신 KEY로 교체 되어야 합니다. 

<pre class="file" data-filename="ncloud.tf" data-target="replace">
provider &#x22;ncloud&#x22; {
access_key = &#x22;ACCESS KEY&#x22;
secret_key = &#x22;SECET KEY&#x22;
region = &#x22;Region&#x22;
}
</pre>

terraform에 대한 Provider의 초기화를 진행 합니다.
`terraform init`{{execute}} 
