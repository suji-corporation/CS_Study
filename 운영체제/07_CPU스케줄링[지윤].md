> 💡 **CPU 스케줄링 척도**  
>   
> 1. **CPU 이용률(CPU utilization)**: 시간당 CPU를 사용한 시간의 비율  
> 2. **처리율(Throughput)**: 시간당 처리한 작업의 비율  
> 3. **반환시간(Turnaround Time)**: 프로세스가 생성된 후 종료되어 사용하던 자원을 모두 반환하는 데까지 걸리는 시간  
> 4. **대기시간(Waiting Time)**: 대기열에 들어와 CPU를 할당받기까지 기다린 시간  
> 5. **반응시간(Response Time)**: 대기열에서 처음으로 CPU를 얻을 때까지 걸린 시간  
>   
> \-> CPU 이용률과 처리율은 극대화하는 것이 좋고 반환시간, 대기시간, 반응시간은 줄이는 것이 좋다

</br></br>

## **1\. CPU 스케줄링 알고리즘 - 비선점형**

-   비선점형 방식(non-preemptive)은 프로세스가 스스로 CPU 소유권을 포기하는 방식
-   강제로 프로세스를 중지하지 않음
-   다른 프로세스가 CPU를 점유하고 있다면 이를 뺏을 수 없음
-   컨텍스트 스위칭으로 인한 부하가 적지만 프로세스의 배치에 따라 효율성 차이가 많이 남
-   Ex) FCFS, SJF, 우선순위 등

</br>

### **1-1) FCFS(First Come First Served)**

![image](https://user-images.githubusercontent.com/64777557/221786862-25b2f576-7396-46a6-99a4-dbf34c7c3290.png)

-   큐에 도착한 순서에 따라 가장 먼저 요청한 프로세스에게 CPU를 할당해주는 알고리즘

**장점**

-   개발 용이
-   기아 현상 발생 X
-   반환시간(Turnaround Time) 면에서 좋을 수 있음

**단점**

-   호위 효과(Convoy Effect)
    -   소요 시간이 긴 프로세스가 먼저 도달할 경우 효율성이 낮아짐
    -   실행 시간이 짧은 작업이어도 늦게 도착하면 대기 시간이 길어짐
-   평균 대기 시간과 응답시간(Response Time)이 길어질 수 있음

</br>

### **1-2) SJF(Shortest Job First)**

![image](https://user-images.githubusercontent.com/64777557/221786925-c25bf8d2-7690-4107-828f-d108084defe0.png)


-   큐에 도착한 프로세스 중 실행 시간(Burst Time)이 가장 짧은 프로세스를 먼저 실행하는 알고리즘
-   실제로는 실행 시간을 정확히 알 수 없기 때문에 과거의 실행 시간을 바탕으로 추측해서 사용

**장점**

-   평균 대기 시간이 짧음
-   짧은 작업에 유리

**단점**

-   기아 현상(Starvation)
    -   실행 시간이 긴 프로세스의 경우 영원히 CPU 할당을 받지 못하게 될 수 있음
-   프로세스의 실제 실행 시간을 예측하기 어려움

</br>

### **1-3) 우선순위(Priority)**

![image](https://user-images.githubusercontent.com/64777557/221786949-92892876-010c-43ef-8f68-4892ff9dfdfc.png)

-   각 프로세스의 우선순위에 따라 우선순위가 가장 높은 프로세스에게 먼저 CPU를 할당하는 알고리즘
-   선점형과 비선점형으로 모두 설계 가능
-   기아 현상이 발생할 수 있지만 노화(Aging) 기법으로 해결 가능
    -   오래된 작업일수록 우선 순위를 높여줌

</br></br>

## **2\. CPU 스케줄링 알고리즘 - 선점형**

-   현재 실행 중인 프로세스를 알고리즘에 의해 중단시키고 강제로 다른 프로세스에게 CPU 소유권을 할당하는 방식
-   다른 프로세스가 CPU를 점유하고 있더라도 이를 뺏을 수 있음
-   처리 시간이 매우 긴 프로세스의 CPU 독점을 막을 수 있지만 잦은 컨텍스트 스위칭으로 부하가 발생할 수 있음
-   Ex) Round Robin, SRF, MLQ/MLFQ

</br>

### **2-1) Round Robin(RR)**

![image](https://user-images.githubusercontent.com/64777557/221787022-794d0187-2a53-4b1a-9c86-b54c69287edb.png)

-   각 프로세스에게 동일한 할당 시간(Time Quantum)을 부여해서 해당 시간 동안만 CPU를 이용하도록 하는 알고리즘
-   할당 시간 안에 끝나지 않으면 다시 준비 큐(Ready Queue)의 가장 뒤로 돌아가고 CPU는 다음 프로세스를 실행

**장점**

-   n개의 프로세스에 대해 할당 시간이 q라면 어떤 프로세스도 (n-1)q 이상의 시간을 기다리지 않아도 됨
-   응답시간(Response Time)이 빠름
-   공정한 스케줄링 기법이며 프로세스들의 CPU 사용 시간이 랜덤하게 섞여 있을 경우 효율적

**단점**

-   할당 시간 q가 커지면 FCFS처럼 동작
-   할당 시간 q가 작아지면 컨텍스트 스위칭이 찾아져서 오버헤드가 커짐

</br>

### **2-2) SRF/SRTF(Shortest Remaining Time First)**

![image](https://user-images.githubusercontent.com/64777557/221787068-b706454b-f750-4f87-be40-2ef4578986b5.png)

-   SJF의 선점형 방식
-   현재 실행 중인 프로세스의 남은 시간보다 더 짧은 실행 시간을 가진 프로세스가 도착할 경우 CPU를 강제로 회수하여 교체하는 알고리즘
-   새로운 프로세스가 도달할 때마다 스케줄링을 다시 하기 때문에 CPU 사용 시간을 측정할 수 없고, 기아 현상 발생 가능

</br>

### **2-3) Multi-Level Queue(다단계 큐)**

![image](https://user-images.githubusercontent.com/64777557/221787090-1a5b3107-579b-41f0-9a3c-2706c627c836.png)

-   여러 종류의 준비 큐(Ready Queue)를 활용하는 알고리즘
-   각 준비 큐에는 서로 다른 스케줄링 알고리즘을 적용
    -   우선순위가 낮은 큐들이 실행 못하는 걸 방지하고자 각 큐마다 다른 Time Quantum을 설정 해주는 방식
    -   우선순위가 높은 큐는 작은 Time Quantum 할당. 우선순위가 낮은 큐는 큰 Time Quantum 할당
    -   일반적으로 Foreground 프로세스들은 Round Robin 방식을 사용하고, Background 프로세스는 FCFS를 사용
-   각 큐 사이에서 프로세스 이동은 불가해 스케줄링 부담이 적음
-   낮은 순위의 큐에 위치한 프로세스가 실행되지 않는 기아 현상이 발생할 수 있음

</br>

### **2-4) MFQ/MLFQ(Multi-Level Feedback Queue)**

![image](https://user-images.githubusercontent.com/64777557/221787113-468877f5-db3c-4baf-95aa-d1743e9c5621.png)

-   Multi-Level Queue와 비슷하지만 큐 간에 프로세스들이 이동할 수 있는 알고리즘
-   Multi-Level Queue에서 자신에게 할당된 Time Quantum을 모두 사용한 프로세스는 밑으로 내려가고 Time Quantum을 다 채우지 못한 프로세스는 원래 큐 위치 그대로 둠
-   처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 반환시간(Turnaround Time)을 줄여주며 짧은 작업에 유리

</br></br>

### **⭐️ 참고**

-   [https://code-lab1.tistory.com/45](https://code-lab1.tistory.com/45)
-   [https://steady-coding.tistory.com/509](https://steady-coding.tistory.com/509)
