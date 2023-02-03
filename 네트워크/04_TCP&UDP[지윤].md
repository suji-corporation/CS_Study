  **TCP**와 **UDP**는 OSI 표준모델과 TCP/IP 모델의 전송계층(Transports Layer)에서 사용되는 프로토콜이다. 다시말해, 송신자와 수신자를 연결하여 데이터 전달을 위해 사용된다. TCP와 UDP 각각의 개념 및 특징에 더해 두 프로토콜의 공통점과 차이점을 알아보자.

</br>

## **TCP**

-   데이터를 메세지 형태(세그먼트 단위)로 보내기 위해 IP와 함께 사용되는 프로토콜
-   TCP/IP에서 IP가 데이터 전달을 처리한다면, TCP는 패킷을 추적 및 관리함
-   연결형 서비스로 클라이언트가 연결 요청 시 서버가 이를 수락해야 1대1 통신 가능 (가상회선 패킷 교환 방식)
    -   3-way handshaking을 통해 연결을 설정, 4-way handshaking을 통해 연결 해제
-   흐름 제어와 혼잡 제어 제공
    -   **흐름 제어**: 데이터 송신지와 수신지의 데이터 처리 속도 조절을 통해 수신지의 버퍼 오버플로우 방지
    -   **혼잡 제어**: 네트워크 내의 패킷 수를 넘치게 증가하지 않도록 방지
-   높은 신뢰성을 보장하지만 (연결형 서비스이기 때문) UDP보다 속도가 느림 (흐름제어와 혼잡제어 때문)
-   전이중(Full-Duplex), 점대점(Point to Point) 방식
    -   **전이중**: 양방향으로 전송이 일어날 수 있음
    -   **점대점**: 각 연결이 전확히 두 개의 종단점을 가지고 있음
    -   멀티태스킹이나 브로드캐스팅 지원 X
-   연속성보다 신뢰성 있는 전송이 중요한 서비스에 사용


#### **TCP 헤더 정보**

<img src="https://user-images.githubusercontent.com/64777557/216592199-a8ff10ff-145d-426c-8cfa-60bc42007096.png" width="800">
<img src="https://user-images.githubusercontent.com/64777557/216592242-301cff92-f906-4801-a96f-6b88dcf3751d.png" width="800">


#### **TCP 제어 비트 (Flag Bit) 정보**

<img src="https://user-images.githubusercontent.com/64777557/216593152-86314459-8e76-4a0b-a49d-d1293718a743.png" width="700">

</br>

## **UDP**

-   데이터를 데이터그램 단위로 처리하는 프로토콜
-   비연결형 서비스로 연결 절차 없이 발신자가 일방적으로 데이터를 보냄 (데이터그램 패킷 교환 방식)
    -   연결을 위해 할당되는 논리적 경로가 없음
    -   때문에 각 패킷은 다른 경로로 전송되고, 독립적인 관계를 지님
    -   데이터를 서로 다른 경로로 독립적으로 처리
-   데이터를 주고 받을 때 정보를 보내거나 받는다는 신호 절차 없음
-   UDP 헤더의 Checksum 필드를 통해 최소한의 오류만을 검출
-   신뢰성이 낮지만 (비연결형 서비스이기 때문) TCP보다 속도가 빠름 (별다른 제어가 없기 때문)
-   신뢰성보다 연속성이 중요한 서비스에 사용  ex) 실시간 서비스 (streaming)

#### **UDP 헤더 정보**

<img src="https://user-images.githubusercontent.com/64777557/216594066-4b5891e6-4335-40c2-b4af-5660ab89449d.png" width="800">
<img src="https://user-images.githubusercontent.com/64777557/216594250-6536345c-5bc0-4d16-8b6c-9d9b8f288127.png" width="800">

</br>

## **TCP와 UDP 비교 정리**

![](https://blog.kakaocdn.net/dn/bAaY2x/btrrDNHQN9j/lKNSDkXHV0NYGjSH7T4flK/img.png)

**공통점**

-   포트 번호를 이용하여 주소 지정
-   데이터 에러 검출을 위한 체크썸 존재

**차이점**

<img src="https://blog.kakaocdn.net/dn/bk3RqH/btrX1lrQ5yz/mTvu5QKVtn1bgHZc3Ku97k/img.png" width="800">

결론적으로 TCP와 UDP는 포트 번호를 이용하여 주소를 지정하는 점과 데이터 에러 검출을 위한 체크썸이 존재한다는 두 가지 공통점이 있지만 정확성과 신뢰성을 추구할 것인지, 신속성과 연속성을 추구할 것인지에 따라 TCP와 UDP로 나누어 사용한다.
