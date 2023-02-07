## HTTP
- Hyper Text Transfer Protocol
- 웹상에서 클라이언트와 서버 간에 요청/응답으로 데이터 주고 받을 수 있는 프로토콜
- 포트 번호 `80`번 사용
- 애플리케이션 레벨의 프로토콜 ⇒ TCP/IP 위에서 작동 
- 비연결(connectionless)
    - 클라이언트가 요청한 응답을 서버에서 보내면 바로 연결이 끊김
- 무상태(statelss)
    - 연결 끊으면 서버의 통신은 끝나고 상태 정보를 유지하지 않음
- Method, Path, Version, Headers, Body로 구성됨            
  <img width="350" alt="image" src="https://user-images.githubusercontent.com/63537847/217150672-e8a01e92-c15f-4828-9e5f-a76fa7070646.png">


**단점**
1. 평문 통신이기 때문에 도청이 가능
2. 통신 상대를 확인하지 않기 때문에 위장 가능
3. 완전성을 증명할 수 없어서 변조 가능

**HTTP Method**
|method|기능|설명|
|---|---|---|
|`POST`|create|body부분에 데이터 담김. 서버값 변경|
|`GET`|read|header부분에 url 담겨져 전송. `?`로 data 실어서 query 작성. 브라우저에서 caching 가능|
|`PUT`|update|리소스 전체 수정할 때 사용|
|`DELETE`|delete|어떤 data 삭제할지 url에 드러나게 전달|
|`PATCH`|update|리소스 일부 수정|

</br>

## HTTP 종류 
- HTTP/0.9 ~ HTTP/2.0 : `TCP` 사용 
- HTTP/3.0 : `UDP` 사용

### 1️⃣ HTTP/0.9
- 단일 라인으로 구성된 요청 
- `GET`이 유일한 method 
- HTML 파일만 전송 가능 

### 2️⃣ HTTP/1.0
- Header, Status code, Content-type 포함  
- 1 request + 1 response / connection
- 매번 새로운 연결로 성능 저하 
- 서버 부하 비용 증가 

### 3️⃣ HTTP/1.1
- persistent connection : 지정한 timeout 동안 connection을 닫지 않는 방식 
- pipelining 기법 도입
  - 하나의 connection에서 응답을 기다리지 않고 순차적인 여러 요청을 연속으로 보내 그 순서에 맞춰 응답을 받는 방식
  - 지연 시간 감소 

**단점** 
  1. Head of line blocking : 1번째 요청이 서버에서 시간이 너무 오래 걸리는 경우 뒤의 작업들도 늦어지는 것 
  2. Header 구조의 중복 : 연속된 요청의 경우 같은 값이 많아도 중복되게 보내서 데이터의 크기가 커짐

### 4️⃣ HTTP/2.0
- 기존 HTTP/1.X 버젼의 성능 향상에 초점을 둔 프로토콜
- 표준의 대체가 아닌 **확장** 
- 여러 개의 HTTP stream을 하나의 TCP connection으로 처리해 HOLB 문제를 해결되지 ❌
- HTTP 메시지 전송 방식의 변화 
  - 바이너리 프레이밍 계층 사용 
  - 파싱 & 전송 속도 ⬆️
  - 오류 발생 가능성 ⬇️
  <img width ="350" src="https://user-images.githubusercontent.com/63537847/217159603-da539819-13a9-48be-99e0-8358bd7c91f8.png">
 
 
**특징**
1. request and reponse multiplexing (다중화 가능)    
    - Head of line blocking(HTTP/1.1) 해결
    - **multiplexing** : 한 개의 TCP 연결에서 여러 개의 데이터를 섞이지 않게 보내는 방법 
      - **stream** : 각각의 데이터 흐름 
2. stream prioritization     
    - 리소스 간 전송 우선 순위 설정할 수 있음 
    - 먼저 전송하고자 하는 stream에 가중치 
3. server push      
    - 클라이언트에서 요청하지 않은 데이터를 서버가 알아서 전송
4. header compression 
    - 헤더의 크기를 줄여 페이지 로드 시간 감소 
    - 중복되는 부분 보내지 ❌

### 5️⃣ QUIC
- Quick UDP Internet Connection
- 전송 계층 프로토콜 (TCP나 UDP 같은 프로토콜)
- TCP handshake 과정을 최적화하는데 초점을 맞춰 설계됨 
  - TCP가 handshake할 때 오버헤드와 HOLB의 문제를 피할 수 없기 때문에 
- `UDP` 기반
  - 각각의 패킷 간 순서가 존재하지 않는 독립적인 패킷 (데이터그램 방식)
  - 데이터 전송에 집중한 설계 
  - 별도의 기능 ❌ 
    - 원하는 기능 구현(Header) ⭕
    - 커스터마이징으로 신뢰성 확보 
    - TCP의 지연은 줄이면서 TCP만큼 신뢰성 확보 가능 

**특징** 
1. 전송 속도 향상 
    - 첫 연결 설정에서 필요한 정보와 데이터 함께 전송
    - 연결 성공 시 설정 캐싱해서 다음 연결 때 바로 성립 가능 (handshake 필요 ❌)
2. connection UUID라는 고유 식별자로 서버와 연결 
    - connection 재수립 할 필요 ❌
    - 클라이언트의 IP 바뀌어도 연결 유지
3. TLS 기본 적용 
4. IP Spoofing, Replay Attack 방지 
5. 독립 스트림 ⇒ 향상된 멀티플렉싱 기능  


### 6️⃣ HTTP/3.0
- `UDP` 사용 
- `QUIC` 프로토콜 위에서 돌아가는 HTTP 


</br>

<img width="500" src="https://user-images.githubusercontent.com/63537847/217154519-0562f1ef-a52d-4fba-9995-18180c7dec71.png">

## HTTPS
- Hyper Text Transfer Protocol Secure
- 웹 통신 프로토콜인 HTTP의 보안이 강화된 버젼
- HTTP에 **데이터 암호화**가 추가된 프로토콜
- 통신하는 소켓 부분을 `SSL` 또는 `TLS` 프로토콜로 교체
- 포트 번호 `443`번 사용
- 암호화, 증명서, 안전성 보호 이용할 수 있음
- `대칭키 암호화` 방식과 `비대칭키 암호화` 방식 모두 사용 
- 암호화 통신은 CPU나 메모리 등의 리소스를 평문 통신(HTTP)보다 많이 요구
    - BUT 하드웨어의 발달로 이제는 속도 저하 거의 일어나지 않음
- 데이터의 적절한 보호 보장
    - 구현 정확도, 서버 소프트웨어, 암호화 알고리즘 → 보호 성능 결정
- 원리
    - 공개키 알고리즘 방식
    - 암호화, 복호화시킬 수 있는 서로 다른 키를 이용한 암호화
        - 공개키 : 모두에게 공개
        - 개인키(비공개키) : 개인에게만 공개. 서버가 갖는 비공개키(클-서 구조에서)
    - 클라이언트 → 서버
        1. 사용자의 데이터 공개키로 암호화 
        2. 서버로 전송 (복호화 불가능) 
        3. 서버의 개인키로 복호화 

**장점** 

1. 네트워크 상에서 열람, 수정이 불가능해 안전함 

**단점**

1. 암호화 하는 과정이 웹 서버에 부하를 줌 
2. HTTP에 비해 느림 
3. HTTPS 설치 및 인증서 유지하는데 추가 비용 발생 
4. 인터넷 연결 끊긴 경우 재인증 시간 소요

### HTTPS 동작 과정 
- Handshaking 
  - 서버와 클라이언트 간의 세션키 교환 
  - **세션키** : 주고 받는 데이터를 암호화하기 위해 사용되는 `대칭키`. 빠른 연산 속도 필요 
  - 처음 연결을 성립하여 안전하게 게견키를 공유하는 과정에서 `비대칭키` 사용 
  <img width ="500" src="https://user-images.githubusercontent.com/63537847/217152565-aea564d6-48a0-4250-96c4-85068d3bbdf0.png">
- **흐름**
  1. 클라이언트(브라우저)가 서버로 최초 연결 시도를 함
  2. 서버는 공개키(엄밀히는 인증서)를 브라우저에게 넘겨줌
  3. 브라우저는 인증서의 유효성을 검사하고 세션키를 발급함
  4. 브라우저는 세션키를 보관하며 추가로 서버의 공개키로 세션키를 암호화하여 서버로 전송함
  5. 서버는 개인키로 암호화된 세션키를 복호화하여 세션키를 얻음
  6. 클라이언트와 서버는 동일한 세션키를 공유하므로 데이터를 전달할 때 세션키로 암호화/복호화를 진행함   
  <img width ="500" src="https://user-images.githubusercontent.com/63537847/217153254-d628b1b3-1ea8-4c31-8002-99c373e2ec56.png">

</br>

## SSL
- Secure Sockets Layer 
- 클라이언트와 서버가 서로 데이터를 암호화해 통신할 수 있도록 돕는 보안 계층 
- HTTP와 같은 계층의 프로토콜(FTP, SMTP 등)과 조합해 사용 가능 
- **TLS**
  - Transport Layer Security 
  - SSL2.0에 취약점이 발견되어 구조를 재설계해서 SSL 3.0 발표 ⇒ 이전 버젼과 구분하기 위해 이름을 `TLS`로 변경! 


</br>

> 참고     
> https://mangkyu.tistory.com/98           
> https://brunch.co.kr/@swimjiy/47      
> https://youtu.be/xcrjamphIp4          
> https://velog.io/@kcwthing1210/HTTP-1.1-vs-HTTP-2.0-vs-HTTP-3.0       
> https://woojinger.tistory.com/85    
