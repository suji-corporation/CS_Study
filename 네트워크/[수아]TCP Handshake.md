## TCP 3-way Handshake

- TCP 통신을 이용하여 데이터를 전송하기 위해 `네트워크 연결` 설정하는 과정
    - TCP 접속을 성공적으로 성립하기 위해 반드시 필요
- 양쪽 모두 데이터 전송할 준비 되었다는 것 보장
- 데이터 `전달 전`, 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있게 함
- 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션 수립하는 과정
- `seq num은 왜 랜덤?`
    - 오류가 나서 동시에 패킷을 2개 보내는 경우, 구별이 안되기 때문에
- `3-way handshake는 왜 하는걸까?`
  - 데이터를 전송하기 전에 정확한 전송을 보장하기 위해서
  - 신뢰성을 보장하기 위해서

### A(client) 가 B(server)에 연결을 요청하는 경우

<img width="600" alt="image" src="https://user-images.githubusercontent.com/63537847/216234821-a3d8f599-3369-4de7-807e-2fab00ee6174.png">


1️⃣ **A → B : SYN ** 

- A가 연결 요청 메시지 전송 (SYN)
- (sequence number : 랜덤 숫자, SYN 플래그 비트 : 1) ➡️ 패킷 전송
- PORT 상태 : B(=LISTEN) A(=CLOSED → SYN_SENT)

2️⃣ **B → A : SYN + ACK **

- B가 요청 수락, A의 포트 열어 달라고 메시지 전송 (SYN + ACK)
- (acknowledgement num = sequence num + 1, SYN & ACK 플래그 비트 : 1) ➡️ 패킷 전송
- PORT 상태 : B(=SYN_RCV) A(=SYN_SENT)

3️⃣ **A → B : ACK  **

- PORT 상태 : B(=SYN_RCV) A(=ESTABLISHED)
- A가 수락 확인을 보내고 연결을 맺음(ACK)
- 데이터 전송
  - PORT 상태 : B(=ESTABLISHED) A(=ESTABLISHED)


</br>

## 4-way Handshake

- TCP 연결 해제하는 과정

<img width="600" alt="image" src="https://user-images.githubusercontent.com/63537847/216235090-856ad09e-9373-4497-8e54-34f8436c9af8.png">


1️⃣ A → B : FIN

- A가 연결 종료 FIN 플래그 전송
- B가 FIN 플래그로 요청하기 전까지 연결 유지
- PORT 상태 : A(=ESTABLISHED → FIN_WAIT), B(=ESTABLISHED)

2️⃣ B → A : ACK 

- B는 확인 메시지 보내고 본인 통신 끝날 때까지 대기(TIME_WAIT)
- (acknowledgement num = sequence num + 1, SYN & ACK 플래그 비트 : 1) → 패킷 전송
- **전송할 데이터가 남아있으면 계속 전송**
- PORT 상태 : A(=FIN_WAIT), B(=CLOSE_WAIT)

3️⃣ B → A : FIN

- B의 통신이 끝났으면 A에게 FIN 플래그 전송
- PORT 상태 : A(=TIME_WAIT)

4️⃣ A → B : ACK

- A는 확인했다는 메시지 전송
- PORT 상태 : A(CLOSED), B(=CLOSED)

</br>

**[PORT 상태 정보]**

- CLOSED : 포트 닫힘
- LISTEN : 포트 열린 상태로 요청 대기 중
- SYN_RCV : SYNC 요청 받고 상대 응답 기다리는 중
- ESTABLISHED : 포트 연결 상태

**[플래그 정보]**

- TCP HEADER에는 CONTROL BIT 존재
  - `URG-ACK-PSH-RST-SYN-FIN`
- SYN(Synchronize Sequence Number) : 연결 설정
- ACK(Acknowledgement) : 응답 확인. 패킷 받음
- FIN(Finish) : 연결 해제
