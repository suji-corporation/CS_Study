# DNS (Domain Name System) 
## Domain이란? 
- IP 주소를 대신해서 사용하는 주소 
- 외우기도 어렵고 한 눈에 파악하기 쉽지 않은 IP 주소보다 쉽고 분명하게 나타낼 수 있음 

</br> 

## DNS란? 
- `도메인 이름`을 실제 네트워크 상에서 사용하는 `IP 주소`로 변환해주고 접속해주는 시스템 
- `DNS 서버`로 도메인 이름과 매칭된 IP 주소 확인 
- DNS는 **데이터베이스 시스템**
- 상위 기관에서 인증된 기관에게 도메인을 생성하거나 IP 주소로 변경할 수 있는 ‘권한’을 부여
  - 계층 구조를 갖는 분산 데이터베이스 구조 
  - 도메인에 점(dot) == 계층 
  - 도메인은 계층적으로 관리          

![image](https://user-images.githubusercontent.com/63537847/218657023-d040db15-bef1-4707-8422-f1d9f39b81a3.png)

</br>

## 🧩 DNS 구성 요소 
1. 도메인 네임 스페이스(Domain Name Space)  
    - 도메인 이름 저장 분산 
2. 네임 서버(Name Server) = 권한 있는 DNS 서버  
    - = DNS 서버
    - 해당 도메인 이름의 IP 주소 찾음 
3. 리졸버(Resolver) = 권한 없는 DNS 서버 
    - DNS 클라이언트 요청을 네임 서버로 전달 
    - 찾은 정보를 클라이언트에 제공 
    - Resolver = Recursive DNS Server = Local Server(of ISP) = Recursor

### 1️⃣ 도메인 네임 스페이스(Domain Name Space) 
- DNS는 전세계적인 거대한 분산 시스템 
- `도메인 네임 스페이스`는 DNS가 저장 관리하는 **계층적 구조** 
- 최상위 == Root DNS 서버 
- 하위로는 연결된 모든 노드가 연속해서 이어지는 계층 구조 

<img width="370" alt="image" src="https://user-images.githubusercontent.com/63537847/218660010-3036cbce-1ee0-4268-8c90-15030916ccc8.png">

**Fully Qualified Domain Name(FQDN)**
- 전체 도메인 이름 
- 도메인 이름 : naver.com / 호스트 이름 : www
- FQDN : www.naver.com

### 2️⃣ 네임 서버(Name Server = DNS Server)
- 도메인 네임 스페이스의 트리 구조에 대한 정보는 갖고 있는 서버 
- 데이터베이스 역할(저장, 관리), 찾아주는 역할, 요청 처리 응답 구현
- 권한 있는 DNS 서버 
  - IP 주소와 도메인 이름 매핑 

1. Root DNS server 
    - ICANN이 직접 관리하는 존엄 서버 
    - TLD DNS 서버 IP 주소를 저장하고 안내하는 역할
2. Top-Level Domain(TLD) DNS server
    - 도메인 등록 기관이 관리하는 서버 
    - Authoritative DNS 서버의 주소를 저장하고 안내하는 역할
    - 도메인 판매 업체의 DNS 설정 변경되면 도메인 등록 기관으로 전달됨 
      - 어떤 도메인이 어떤 업체에서 구매했는지 파악 가능 
3. Second-Level Domain(SLD) DNS server (= Authoritative DNS server)
    - 실제 개인 도메인과 IP 주소의 관계가 기록(저장, 변경)되는 서버
    - 일반적으로 도메인/호스팅 업처의 네임서버를 지칭 
    - 개인 DNS 구축했을 경우 

### 3️⃣ 리졸버(Resolver) = Recursive DNS Server = Local DNS Server
- 권한 없는 DNS 서버 
- 질의를 통해 IP 주소 알아내거나 캐시 
- DNS 클라이언트(웹 브라우저)의 요청 네임 서버로 전달 ⇒ 네임 서버로부터 정보(도메인 이름 & IP 주소) 받아 클라이언트에 제공
- 하나의 네임 서버에게 DNS 요청을 전달하고 해당 서버에 정보가 없으면 다른 네임 서버에게 요청

</br> 

## ⚙️ DNS 동작 방식 
![image](https://user-images.githubusercontent.com/63537847/218657591-428c78bd-5aa0-402f-be33-a83987eb74f9.png)

1. 웹 브라우저에 도메인 주소가 입력되면 이전에 방문한 적 있었는지 캐시 확인
    - 브라우저 캐시
    - OS 캐시
    - 라우터 캐시
2. 웹 브라우저는 Recursive DNS 서버에게 도메인(www.naver.com)의 IP 주소 요청 
3. Recursive는 최상위 기관에서 관리하는 네임 서버인 Root DNS 서버에 요청(.com) 
4. `.com`도메인은 .com 서버(TLD server)로 가라고 응답 
5. Recursive는 .com 서버(TLD server)에 naver.com요청
6. Authoritative에 가라는 응답 
7. Authoritative에 www.naver.com 있으면 IP주소 리턴 
8. 웹 브라우저에 IP주소 전달됨 

![image](https://user-images.githubusercontent.com/63537847/218665781-5a838104-e813-45c0-91e2-952427f21d44.png)

### DNS Query
- DNS 클라이언트와 DNS 서버는 DNS query 교환
- Recursive or Iterative로 구분 

**Recursive Query** 
- 결과(IP)를 돌려주는 작업 
- Recursive 쿼리를 받은 Recursive 서버는 Iterative 하게 권한 있는 네임 서버로 Iterative 쿼리를 보내서 결과적으로 IP 주소를 찾게 되고 해당 결과물을 응답

**Iterative Query** 
- Recursive DNS 서버가 다른 DNS 서버에게 쿼리를 보내어 응답을 요청하는 작업
- Recursive 서버가 권한 있는 네임 서버들에게 반복적으로 쿼리를 보내서 결과물(IP 주소)를 알아냄
- Recursive 서버에 이미 IP 주소가 캐시 되어있다면 이 과정은 skip


</br>
</br>

> 참고                         
> https://hanamon.kr/dns%EB%9E%80-%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%84%A4%EC%9E%84-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D%EA%B9%8C%EC%A7%80/                                
> https://velog.io/@dustjs159/DNS        
