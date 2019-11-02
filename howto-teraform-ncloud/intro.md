실습 순서는 아래와 같습니다.

1. Working directory 생성 및 Terraform Provider 정의

2. HCL (HashiCorp Configuration Language) 이용 네이버 클라우드 플랫폼 리소스 정의

3. 한국 리전 두 개의 Availability Zone (kr-1, kr-2)에 각 웹 서버 한대씩 생성

   Load Balancer(데모에서는 tf_webinar_lb) 생성 & 웹 서버 두 대 바인딩

4. 리소스에 대한 생성 & 변경 내용 확인(Plan)

5. 실제 인프라 적용(Apply)
