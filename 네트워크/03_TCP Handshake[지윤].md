## **3-way handshake**

-   TCP 통신으로 데이터를 전송하기 위해 **네트워크 연결을 설정**하는 과정
-   양 측 모두 **데이터를 전송할 준비가 되어있음**을 보장하기 위해 사전에 **세션을 수립**함
-   실제로 데이터 전달이 시작되기 전에 수신 측이 데이터를 받을 준비가 되어 있다는 것을 알 수 있도록 함

![image](https://user-images.githubusercontent.com/64777557/216244687-d01fd30a-0ed9-41f6-9454-37cf491445e5.png)

### **3-way hanshaking 과정**

\* Host A(=클라이언트), Host B(=서버) 통신

1.  Host A는 Host B에게 접속을 요청하는 SYN 패킷 전송
    -   송신자가 최초로 데이터를 전송할 때 Sequence Number는 임의의 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트 전송
    -   포트 상태 Host A: CLOSED / Host B: LISTEN
2.  접속 요청을 받은 Host B는 Host A에게 해당 요청을 수락하며 포트를 열어달라는 SYN + ACK 전송
    -   수신자는 Acknowledgement Number를 (Sequence Number + 1)로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송
    -   포트 상태 Host A: SYN\_SENT / Host B: SYN\_RCV
3.  Host A가 Host B에게 수락을 확인했다는 ACK 전송
    -   전송할 데이터가 있다면 데이터 전송 가능
    -   포트 상태 Host A: ESTABLISHED / Host B: SYN\_RCV ➡ Host A: ESTABLISHED / Host B: ESTABLISHED

![image](https://user-images.githubusercontent.com/64777557/216245938-0f1ef09b-edf7-4e30-a23d-3e5e3c516af9.png)

</br>

### **참고 - 포트 상태 정보**

-   CLOSED: 포트가 닫힌 상태
-   LISTEN: 포트가 열린 상태로 연결 요청 대기 상태
-   SYN\_SENT: SYN 을 보내고 SYN/ACK 응답을 기다리는 상태
-   SYN\_RCV: SYN 요청을 수락하고 상대방의 응답 대기 상태
-   ESTABLISHED: 포트 연결 상태

</br></br>

## **4-way handshake**

-   TCP의 **네트워크 연결을 해제**하는 과정

![image](https://user-images.githubusercontent.com/64777557/216246257-2e5f8ef4-3139-485b-995b-037602628706.png)
### **4-way hanshaking 과정**

\* Host A(=클라이언트), Host B(=서버) 통신

1.  Host A가 Host B에게 연결을 종료하겠다는 FIN 플래그 전송
    -   Host B가 FIN 플래그로 응답하기 전까지는 연결 유지
2.  Host B는 일단 확인했다는 ACK 전송 후 자신의 통신이 끝날 때까지 대기 (CLOSE\_WAIT 상태)
    -   수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트 전송
    -   자신이 전송할 데이터가 남아있다면 마저 전송
3.  더이상 전송할 데이터가 없다면 Host B는 연결 종료 요청을 수락한다는 의미로 Host A에게 FIN 플래그 전송
4.  Host A가 Host B에게 수락을 확인했다는 ACK 전송
    1.  Host A는 CLOSED 상태 이전에 TIME-WAIT 상태를 거침

![image](https://user-images.githubusercontent.com/64777557/216246307-1ef966eb-fdda-4422-a2ce-3f402fc1933c.png)

</br>

### **※ TIME-WAIT 상태가 필요한 이유**

-   Server 측에서 늦게 도착하는 지연 패킷이나 유실로 인해 재전송 된 패킷 등을 기다리기 위해
-   Client가 ACK 패킷을 보낸 직후 바로 CLOSED 해버리면 Client의 상태는 CLOSED인데 Server의 상태가 LAST-ACK인 경우가 발생 -> 이후 재연결 시 Client와 Server의 포트 상태가 모두 CLOSED가 아니므로 연결 실패
