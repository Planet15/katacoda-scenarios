
## infra.tf 구성 하기  

한국 리전 두 개의 Availability Zone (kr-1, kr-2)에 생성을 하기 위해서 
서버 사양 과 설치되는 OS 이미지를 지정 합니다.

<pre class="file" data-filename="infra.tf" data-target="replace">
variable “ncloud_zones” {
type = “list”
default = [“KR-1”, “KR-2”]
}

variable “server_image_prodict_code” {
default = “SPSW0LINUX000032”
}

variable “server_product_code” {
default = “SPSVRSTAND000004”
}
</pre>

키를 미리 생성 해야 하며, 여기에는 키의 이름을 기입 합니다.
<pre class="file" data-filename="infra.tf" data-target="append">
resource “ncloud_login_key” “loginkey” {
“key_name” = “webinar”
}
</pre>

생성될 서버에 user-data.sh 를 추가 합니다.
<pre class="file" data-filename="infra.tf" data-target="append">
data “template_file” “user_data” {
template = “${file(“user-data.sh”)}”
}
</pre>

생성될 서버의 타입을 지정 하고, 서버 갯수를 2개로 설정 합니다.
<pre class="file" data-filename="infra.tf" data-target="append">
resource “ncloud_server” “server” {
“count” = “2”
“server_name” = “tf-webinar-vm-${count.index+1}”
“server_image_product_code” = “${var.server_image_prodict_code}”
“server_product_code” = “${var.server_product_code}”
“server_description” = “tf-webinar-vm-${count.index+1}”
“login_key_name” = “${ncloud_login_key.loginkey.key_name}”
“access_control_group_configuration_no_list” = [“13054”]
“zone_code” = “${var.ncloud_zones[count.index]}”
“user_data” = “${data.template_file.user_data.rendered}”
}
</pre>

Load Balancer(데모에서는 tf_webinar_lb) 생성 하며,  웹 서버 두 대 바인딩을 하게 설정 합니다.
<pre class="file" data-filename="infra.tf" data-target="append">
resource “ncloud_load_balancer” “lb” {
“load_balancer_name” = “ttf_webinar_lb”
“load_balancer_algorithm_type_code” = “RR”
“load_balancer_description” = “tf_webinar_lb”

“load_balancer_rule_list” = [

{
“protocol_type_code” = “HTTP”
“load_balancer_port” = 80
“server_port” = 80
“l7_health_check_path” = “/”
},
]

“server_instance_no_list” = [“${ncloud_server.server.*.id[0]}”,
“${ncloud_server.server.*.id[1]}”]
“internet_line_type_code” = “PUBLC”
“network_usage_type_code” = “PBLIP”
“region_no” = “1”
}
</pre>

리소스에 대한 생성 및 변경 내용 확인하며, 현재 이 과정은 실제로 생성 되는 과정은 아닙니다.
`terraform plan`{{execute}}
