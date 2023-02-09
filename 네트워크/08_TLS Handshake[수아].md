## TLS/SSL 인증서 
- 클라이언트와 서버 간의 통신을 제3자가 보증해주는 전자화된 문서 
- 공개키, 발급자, 유효 기간 등의 정보 포함하고 있음
- `CA(Certificate Authority)` : 보증해주는 제 3자
- 통신 내용이 노출되거나 변경되는 것 방지 
- 해당 서버가 신뢰할 수 있는 서버임을 보장 

</br>

## TLS 통신 과정 
- 공개키 방식은 많은 컴퓨터 자원을 사용하는 단점이 있음
- 대칭키 방식은 효율적이지만 수신측과 공신측이 동일한 키를 갖게 되어서 보안상 문제가 발생한다는 단점이 있음 
- TLS는 암호화된 데이터를 전송하기 위해 `공개키`와 `대칭키` 혼합해서 사용
- 실제 데이터 : 대칭키 
- 대칭키의 키 : 공개키
- 클라이언트와 서버가 주고 받는 실제 정보는 `대칭키 방식`으로 암호화                
  → 대칭키 방식으로 암호화된 실제 정보를 복호화할 때 사용할 `대칭키`는 `공개키 방식`으로 암호화 
- 통신의 3단계 : 1️⃣handshake 2️⃣transmit 3️⃣session closed

</br>

## TLS Handshake 
- 상대방이 존재하는지, 상대방과 데이터를 주고 받기 위해서는 어떤 방법을 사용해야 하는지 파악하는 단계 
- TLS 인증서 주고 받음 

🚩**목표**
1. 클라이언트와 서버가 주고 받을 데이터의 암호화 알고리즘 결정
2. 데이터의 암호화를 위한 동일한 대칭키(데이터를 암호화하는 키)를 얻음

![image](https://user-images.githubusercontent.com/63537847/217736563-b00d7ddd-9aa2-4ce2-842c-c859cee1c12e.png)
BLUE : TCP Handshake / YELLOW : TLS Handshake 

### 1️⃣ Client Hello (client → server)
- client에서 server 연결 시도 
- 전송되는 패킷의 내용 
  1. 자신(client)이 사용 가능한 cipher suite 목록
      - cipher suite : 어떤 프로토콜(TLS/SSL)을 사용할지, 인증서 검증 또는 데이터를 어떤 방식으로 암호화할지 등 표시 
  2. session id 
  3. ssl protocol verion            
  ... 등등  
  
### 2️⃣ Server Hello (server → client)
- 1️⃣에 대한 서버의 응답 
- 내용 
  1. 클라이언트의 cipher suite 중 선택한 1개 
  2. ssl protocol version              
  ... 등등 

### 3️⃣ Certificate, Server Key Exchange, Server Hello Done (server → client) 
- `certificate` 내용의 패킷 전송 
  - 서버의 ssl 인증서 내용이 들어있음 
- certificate 내의 TLS 인증서에 서버의 `공개키`가 존재하지 않으면 서버가 직접 공개키 전달 (Server Key Exchange)
- 서버 행동 마쳤다는 의미로 server hello done 전송 

### 4️⃣ client에서 server의 ssl 인증서 검증 
- 클라이언트는 인증서를 발급한 인증기관(CA)의 공개키 구함
- 공개키를 이용해 server의 ssl 인증서 복호화 
- 복호화 성공 == 인증서 검증 성공 

### 5️⃣ Client Key Exchange, Change Ciper Spec (client → server) 
- 전달하는 데이터 암호화해야 함 
- 클라이언트는 데이터 암호화하기 위한  `대칭키(세션키)` 생성 
- 대칭키를 서버만 볼 수 있도록 서버의 공개키로 암호화해서 서버에 전달 (Client Key Exchange)

### 6️⃣ Handshake Finished 
- 서버가 본인의 비밀키로 암호화된 대칭키 복호화 
- 서버와 클라이언트 모두 `동일한 대칭키` 갖고 있으므로 통신 준비 끝
- 통신 준비 완료 의미의 Change Ciper Spec 전달 (client → server & server → client) 



> 참고             
> https://opentutorials.org/course/228/4894                       
> https://nuritech.tistory.com/25
