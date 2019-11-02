## infra.tf 구성 하기  

리소스 정의 파일을 생성 합니다.
`/terraform/infra.tf`{{open}}

Ncloud terraform API 사용법에 대한 문서는 아래의 내용을 참조 바랍니다.

[Ncloud Provider 사용법](https://www.terraform.io/docs/providers/ncloud/index.html)

한국 리전 두 개의 Availability Zone (kr-1, kr-2)에 생성을 하기 위해서 
서버 사양 과 설치되는 OS 이미지를 변수로 지정 합니다.

SPSVRSTAND000004 : vCPU 2EA, Memory 4GB, Disk 50GB

SPSW0LINUX000032 : centos-6.3-32

[서버 사양 및 서버 이미지 코드](http://bit.ly/2ZimVtS)

<pre class="file" data-filename="infra.tf" data-target="replace">
variable &#x22;ncloud_zones&#x22; {
type = &#x22;list&#x22;
default = [&#x22;KR-1&#x22;, &#x22;KR-2&#x22;]
}

variable &#x22;server_image_prodict_code&#x22; {
default = &#x22;SPSW0LINUX000032&#x22;
}

variable &#x22;server_product_code&#x22; {
default = &#x22;SPSVRSTAND000004&#x22;
}
</pre>

key를 새로 생성하는 부분이며, key 이름은 webinar로 지정합니다.

<pre class="file" data-filename="infra.tf" data-target="append">
resource &#x22;ncloud_login_key&#x22; &#x22;loginkey&#x22; {
&#x22;key_name&#x22; = &#x22;webinar&#x22;
}
</pre>

생성될 서버에 user-data.sh 를 추가 합니다.

<pre class="file" data-filename="infra.tf" data-target="append">
data &#x22;template_file&#x22; &#x22;user_data&#x22; {
template = &#x22;${file(&#x22;user-data.sh&#x22;)}&#x22;
}
</pre>

생성될 서버의 타입을 지정 하고, 서버 갯수를 2개로 설정 합니다.

주의 : access_control_group_configuration_no_list 의 값은 생성 하거나 혹은 있는 ACG ID 번호로 입력 해야 합니다.

[ACG 서비스 설명서](https://docs.ncloud.com/ko/compute/compute-2-3.html)

<pre class="file" data-filename="infra.tf" data-target="append">
resource "ncloud_server" "server" {
&#x22;count&#x22; = &#x22;2&#x22;
&#x22;name&#x22; = &#x22;tf-webinar-vm-${count.index+1}&#x22;
&#x22;server_image_product_code&#x22; = &#x22;${var.server_image_prodict_code}&#x22;
&#x22;server_product_code&#x22; = &#x22;${var.server_product_code}&#x22;
&#x22;description&#x22; = &#x22;tf-webinar-vm-${count.index+1}&#x22;
&#x22;login_key_name&#x22; = &#x22;${ncloud_login_key.loginkey.key_name}&#x22;
&#x22;access_control_group_configuration_no_list&#x22; = [&#x22;ACG ID&#x22;]
&#x22;zone&#x22; = &#x22;${var.ncloud_zones[count.index]}&#x22;
&#x22;user_data&#x22; = &#x22;${data.template_file.user_data.rendered}&#x22;
}
</pre>

Load Balancer(데모에서는 tf_webinar_lb) 생성 하며, 웹 서버 두 대 바인딩을 하게 설정 합니다.

<pre class="file" data-filename="infra.tf" data-target="append">
resource &#x22;ncloud_load_balancer&#x22; &#x22;lb&#x22; {
&#x22;name&#x22; = &#x22;ttf_webinar_lb&#x22;
&#x22;algorithm_type&#x22; = &#x22;RR&#x22;
&#x22;description&#x22; = &#x22;tf_webinar_lb&#x22;

&#x22;rule_list&#x22; = [
{
&#x22;protocol_type&#x22; = &#x22;HTTP&#x22;
&#x22;load_balancer_port&#x22; = 80
&#x22;server_port&#x22; = 80
&#x22;l7_health_check_path&#x22; = "/"
},
]

&#x22;server_instance_no_list&#x22; = [&#x22;${ncloud_server.server.*.id[0]}&#x22;,&#x22;${ncloud_server.server.*.id[1]}&#x22;]
&#x22;internet_line_type&#x22; = &#x22;PUBLC&#x22;
&#x22;network_usage_type&#x22; = &#x22;PBLIP&#x22;
&#x22;region&#x22; = &#x22;KR&#x22;
}
</pre>

리소스에 대한 생성 및 변경 내용 확인하며, 현재 이 과정은 실제로 생성 되는 과정은 아닙니다.

`terraform init`{{execute}}

`terraform plan`{{execute}}

ncloud에 terraform 설정을 마지막으로 적용 합니다.

`terraform apply -auto-approve`{{execute}}


