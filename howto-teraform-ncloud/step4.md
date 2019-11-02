## 인프라 생성 확인을 진행 합니다.

이제 네이버 클라우드 플랫폼 로그인 후 Terraform으로 생성한 서버 두 대와 Load Balancer를 확인할 수 있습니다.

그리고 Load Balancer의 퍼블릭 도메인에 접속해보면 두 대의 웹서버 페이지가 Round Robin으로 분기되는 것을 확인할 수 있습니다.

- 서버 생성 확인
![서버생성 확인](/img/check01.png)

- 로드 밸런서 생성 확인 과  접속 테스트
![로드밸런서 확인 그리고 접속](/img/check02.png)

실습이 완료 되었으며, 실습에 사용한 구성을 삭제 합니다.

`terraform destory -auto-approve`{{execute}}
