## **PCB (Process Control Block)**

<img src="https://user-images.githubusercontent.com/64777557/221105458-5204875e-6799-4008-b852-d40adb729515.png" width=500>

-   운영체제가 시스템 내의 프로세스들을 관리하기 위해 프로세스마다 유지해야하는 정보들을 담는 커널 내 자료구조
    -   커널 주소 공간의 data 영역에 존재
-   프로세스의 메타 데이터들이 저장되는 곳
    -   Process ID
    -   Process 상태
    -   CPU Regiter 값
    -   CPU 스케줄링 정보
    -   메모리 사용/관리 정보
    -   등등
-   Linked List 방식으로 관리
    -   PCB 생성마다 PCB List Head에 주소값으로 연결

</br>

### **PCB가 필요한 이유**

<img src="https://user-images.githubusercontent.com/64777557/221105482-bf84c249-fdb2-4692-966d-21fc18e9528b.png" width=700>

CPU에서는 프로세스의 상태에 따라 주기적으로 프로세스를 교체해주는 작업이 필요하다. 이를 **문맥 교환(Context Switching)**이라고 한다. 문맥 교환 시 현재 CPU 상에서 실행되고 있던 프로세스의 정보(문맥)을 자신의 PCB에 저장하고, 새롭게 CPU 할당을 받게되는 프로세스는 PCB로부터 예전에 저장했던 자신의 정보(문맥)을 복원시켜 이어서 실행할 수 있어야 한다. 이렇듯 PCB는 문맥 교환 과정에서 프로세스의 정보/상태/문맥을 저장하기 위해 사용된다.

</br></br>

## **문맥 교환(Context Switching)**

-   CPU/코어에서 실행중이던 프로세스/스레드가 다른 프로세스/스레드로 교체되는 것 (프로세스/스레드 -> 이하 프로세스)
-   하나의 프로세스로부터 다른 프로세스로 CPU 제어권이 이양되는 과정
-   멀티 태스킹(Multitasking), 인터럽트 핸들링(Interrupt Handling) 시 문맥 교환 발생
    -   **프로세스가 주어진 time slice(quantum)를 다 사용했다는 인터럽트가 발생했을 때**
    -   **입출력 요청(IO 작업) 등에 대한 인터럽트가 발생했을 때**
    -   **자식 프로세스 생성, 인터럽트 처리 대기 등 프로세스가 다른 리소스를 기다려야할 때**
    -   다른 리소스를 기다리지 않아도 되는 그 외 인터럽트나 시스템콜 발생 시에는 유저에서 커널로 모드만 바뀔 뿐 문맥 교환 X
-   여러 프로세스가 동시에 실행되는 것처럼 느껴지게 하기 위해 사용 (멀티 태스킹)
-   짧은 time slice 등으로 인한 잦은 문맥 교환은 오버 헤드를 발생

</br>

### **Context Switching 과정**

<img src="https://user-images.githubusercontent.com/64777557/221105568-e5b22c18-6217-4c4a-b9ae-3dfd0c50cd58.png" width=700>

1.  P1 실행 중 운영체제에서 프로세스의 스케줄러에 의해 인터럽트 발생
2.  프로세스가 실행되는 유저 모드에서 커널 모드로 전환
3.  기존에 실행되었던 현재 프로세스(P1)의 상태/정보를 자신의 PCB1에 저장
4.  PCB2로부터 다음에 실행되는 프로세스(P2)의 상태/정보 복구
5.  커널 모드에서 유저 모드로 전환
6.  새로운 프로세스 P2 실행

</br>

### **프로세스 상태 전이에 따른 문맥 교환이 필요한 상황**

| 필요 상황 | 설명 | 전이 과정 |
| --- | --- | --- |
| dispatch | 새롭게 CPU 할당 받아 실행 상태로 전이 | 준비 → 실행   (dispatch) |
| CPU 할당 시간 만료 | CPU 할당 시간 만료로 준비 상태로 전이 | 실행 → 준비   (Timeout) |
| I/O 작업 및 할당 | I/O 작업이 필요하여 작업 완료 시까지 대기 상태로 전이 | 실행 → 대기   (Sleep) |
| System Call | 또다른 서비스 호출이 필요한 경우 대기 상태로 전이 | 실행 → 대기   (Sleep |

</br>

### **문맥 교환의 오버헤드 해결 방안**

-   다중 프로그래밍 수준(time slice 주기)을 낮추어 문맥 교환 발생 빈도 감소
-   스레드를 이용해 문맥 교환 부하 최소화 (스레드는 메모리 영역을 공유하여 메모리 주소 관련 처리를 수행할 필요 X)
-   스택 이용 프로그램의 경우 스택 포인터를 활용해 문맥 교환 부하 최소화

</br></br>

### **⭐️ 참고**

-   [https://aktnfl.tistory.com/25](https://aktnfl.tistory.com/25)
-   [https://spurdev.tistory.com/13](https://spurdev.tistory.com/13)
-   [https://yoongrammer.tistory.com/53](https://yoongrammer.tistory.com/53)
-   [https://gyoogle.dev/blog/computer-science/operating-system/PCB%20&%20Context%20Switching.html](https://gyoogle.dev/blog/computer-science/operating-system/PCB%20&%20Context%20Switching.html)[https://yoongrammer.tistory.com/53](https://yoongrammer.tistory.com/53)
