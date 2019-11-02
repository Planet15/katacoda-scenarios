## infra.tf 구성 하기  

리소스 정의 파일을 생성 합니다.
`/terraform/infra.tf`{{open}}


한국 리전 두 개의 Availability Zone (kr-1, kr-2)에 생성을 하기 위해서 
서버 사양 과 설치되는 OS 이미지를 변수로 지정 합니다.

<pre class="file" data-filename="infra.tf" data-target="replace">
variable &#x22;ncloud_zones&#x22; {
type = &#x22;list&#x22;
default = [&#x22;KR-1&#x22;, &#x22;KR-2&#x22;]
}

variable &#x22;server_image_prodict_code&#x22; {
default = “SPSW0LINUX000032”
}

variable &#x22;server_product_code&#x22; {
default = &#x22;SPSVRSTAND000004&#x22;
}
</pre>

키를 미리 생성 해야 하며, 여기에는 키의 이름을 기입 합니다.

[CTRL+F를 통해 Step 4. 인증키 설정 참고 바랍니다.](https://docs.ncloud.com/ko/compute/compute-1-1-v2.html)

<pre class="file" data-filename="infra.tf" data-target="append">
resource &#x22;ncloud_login_key&#x22; &#x22;loginkey&#x22; {
&#x22;key_name&#x22; = &#x22;webinar&#x22;
}
</pre>

생성될 서버에 user-data.sh 를 추가 합니다.

<pre class="file" data-filename="infra.tf" data-target="append">
data &#x22;template_file&#x22; &#x22;user_data&#x22; {
template = “${file(“user-data.sh”)}”
}
</pre>

생성될 서버의 타입을 지정 하고, 서버 갯수를 2개로 설정 합니다.

<pre class="file" data-filename="infra.tf" data-target="append">
resource &#x22;ncloud_server&#x22; &#x22;server&#x22; {
&#x22;count&#x22; = &#x22;2&#x22;
&#x22;server_name&#x22; = &#x22;tf-webinar-vm-${count.index+1}&#x22;
&#x22;server_image_product_code&#x22; = &#x22;${var.server_image_prodict_code}&#x22;
&#x22;server_product_code&#x22; = &#x22;${var.server_product_code}&#x22;
&#x22;server_description&#x22; = &#x22;tf-webinar-vm-${count.index+1}&#x22;
&#x22;login_key_name&#x22; = &#x22;${ncloud_login_key.loginkey.key_name}&#x22;
&#x22;access_control_group_configuration_no_list&#x22; = [&#x22;13054&#x22;]
&#x22;zone_code&#x22; = &#x22;${var.ncloud_zones[count.index]}&#x22;
&#x22;user_data&#x22; = &#x22;${data.template_file.user_data.rendered}&#x22;
}
</pre>

Load Balancer(데모에서는 tf_webinar_lb) 생성 하며,  웹 서버 두 대 바인딩을 하게 설정 합니다.
<pre class="file" data-filename="infra.tf" data-target="append">
resource &#x22;ncloud_load_balancer&#x22; &#x22;lb&#x22; {
&#x22;load_balancer_name&#x22; = &#x22;ttf_webinar_lb&#x22;
&#x22;load_balancer_algorithm_type_code&#x22; = &#x22;RR&#x22;
&#x22;load_balancer_description&#x22; = &#x22;tf_webinar_lb&#x22;

&#x22;load_balancer_rule_list&#x22; = [

{
&#x22;protocol_type_code&#x22; = &#x22;HTTP&#x22;
&#x22;load_balancer_port&#x22; = 80
&#x22;server_port&#x22; = 80
&#x22;l7_health_check_path&#x22; = &#x22;/&#x22;
},
]

&#x22;server_instance_no_list&#x22; = [&#x22;${ncloud_server.server.*.id[0]}&#x22;,
&#x22;${ncloud_server.server.*.id[1]}&#x22;]
&#x22;internet_line_type_code” = &#x22;PUBLC&#x22;
&#x22;network_usage_type_code” = &#x22;PBLIP&#x22;
&#x22;region_no” = “1”
}
</pre>

리소스에 대한 생성 및 변경 내용 확인하며, 현재 이 과정은 실제로 생성 되는 과정은 아닙니다.

`terraform plan`{{execute}}

ncloud에 terraform 설정을 마지막으로 적용 합니다.

`terraform apply`{{execute}}

