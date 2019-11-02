

ncloud 계정 가입 후 실습이 가능합니다. 
[CreditEvent](https://www.ncloud.com/main/creditEvent)

실습 순서는 아래와 같습니다.

- **Working directory 생성** 및 **Terraform Provider 정의**
- **HCL (HashiCorp Configuration Language) 통한 네이버 클라우드 플랫폼 리소스 정의**
- 한국 리전 두 개의 **Availability Zone (kr-1, kr-2)**에 각 웹 서버 한대씩 생성
- **Load Balancer(데모에서는 tf_webinar_lb) 생성** 과 웹 서버 두 대 **바인딩**
- 리소스에 대한 생성 과 변경 내용 확인(Plan)
- 실제 인프라 적용(Apply)
