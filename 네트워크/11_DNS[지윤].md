## **DNS(Domain Name System)**

-   도메인 이름을 실제 IP 주소로 변환하여 연결해주는 시스템
-   사용자로 하여금 도메인 이름을 통해 실제 네트워크 상에서 사용하는 IP로 접속할 수 있도록 함
-   DNS는 상위 기관과 하위 기관과 같은 계층 구조를 가지는 분산 데이터베이스 구조
-   상위 기관에서 인증된 기관에게 도메인 생성 및 IP 주소로 변경할 수 있는 권한 부여

</br>

### **3가지 구성 요소**

-   **도메인 네임 스페이스(Domain Name Space)**
    -   DNS가 저장/관리하는 계층적 구조
    -   최상위에 루트 DNS 서버가 존재하고 그 하위로 연결된 모든 노드가 연속해서 이어진 계층 구조로 이루어짐

![image](https://user-images.githubusercontent.com/64777557/218669814-7288679a-39b0-46e0-8822-03653f326709.png)

-   **네임 서버(Name Server, =DNS서버)**: 권한 있는 DNS 서버
    -   문자로 이루어진 도메인 이름을 실제 네트워크 통신 시 사용되는 IP주소로 변환하기 위해서는 도메인 네임 스페이스의 트리 구조에 대한 정보가 필요한데 이러한 정보를 가지고 있는 서버
    -   도메인 이름과 IP주소를 저장하고 있는 데이터베이스 역할
    -   **Root DNS 서버**: TLD DNS 서버의 IP 주소를 관리하는 절대 존엄 서버
    -   **Top-Level Domain(TLD) DNS 서버**: 도메인 등록 기관이 관리하며 SLD DNS 서버의 IP주소를 관리
    -   **Second-Level Domain(SLD) DNS 서버(=Authoritative DNS 서버)**: 실제 도메인과 IP 주소의 관계가 저장
-   **리졸버(Resolver)**: 권한 없는 DNS 서버
    -   DNS 클라이언트의 요청을 네임서버로 전달하고 네임서버로 부터 IP주소 정보를 받아 클라이언트에게 제공
    -   수많은 네임서버에 접근하며 사용자로부터 요청받은 도메인의 IP정보를 조회하는 기능 수행

</br>

### **DNS 동작 방식**

> ※ Local DNS: Resolver Server / com DNS: TLD DNS 서버 / naver.com DNS: SLD DNS 서버

![image](https://user-images.githubusercontent.com/64777557/218669938-3d14ae5c-a041-4b2d-bd1d-0b5477976f4d.png)

1.  웹 브라우저에 `www.naver.com`을 입력하면 Local DNS에게 `www.naver.com`에 대한 DNS 조회 진행
2.  Local DNS는 Root DNS로 `www.naver.com`에 대한 질의
3.  Root DNS는 Local DNS에게 com DNS의 IP 주소 전달
4.  Local DNS는 전달받은 IP주소로 com DNS에게 `www.naver.com`에 대한 질의
5.  com DNS는 Local DNS에게 naver.com DNS의 IP 주소를 전달
6.  Local DNS는 또다시 전달받은 IP주소로 naver.com DNS에게 `www.naver.com`에 대한 질의
7.  naver.com DNS는 Local DNS에게 `www.naver.com`에 해당하는 IP주소인 `222.122.195.6` 전달
8.  Local DNS는 해당 IP주소를 캐싱한 뒤 웹 브라우저에게 전달
