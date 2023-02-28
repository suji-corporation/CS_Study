## CPU 스케줄링 
- `CPU 스케줄러` : CPU에서 실행될 프로세스 선택하는 역할 
- `Dispatcher` : 선택된 프로세스에게 CPU 할당하는 역할 (context switching, user mode 전환 등) 
- 언제 어떤 프로세스에 CPU를 할당할지 결정하는 작업 

### CPU 스케줄링 평가 기준 
1. **CPU 이용률(CPU Utilization)** 
   - 시간 당 CPU 사용한 시간의 비율 
   - CPU가 항상 일을 할 수 있게 해야 함 

2. **처리율(Throughput)** 
   - 시간당 처리한 작업의 비율 
   - 단위 시간 당 완료되는 작업 수 많아야 함 
   
3. **반환시간(Turnaround Time)**
   - 프로세스가 생성된 후 종료되어 사용하던 자원을 모두 반환하는 데까지 걸리는 시간
   - 작업이 준비 큐(ready queue)에서 기다린 시간부터 CPU에서 실행된 시간, I/O 작업 시간의 합

4. **대기시간(Waiting Time)** 
   - 대기열에 들어와 CPU를 할당받기 까지 기다린 시간
   - 준비 큐에서 기다린 시간의 총합

5. **반응시간/응답시간(Response Time)**
   - 대기열에서 처음으로 CPU를 얻을 때까지 걸린 시간
   - CPU를 할당받은 최초의 순간까지 기다린 시간 측정 (요청 후 응답이 올 때까지 기다린 시간) 

> CPU 이용율 + 처리율 ⬆️ 반환시간 + 대기시간 + 반응시간 ⬇️ 것이 일반적으로 좋음 

</br>

## 비선점(Non-preemtive) 스케줄링 
- 실행(running) 상태에서 대기(waiting) 상태로 전환(switching)될 때 (ex : I/O 발생)
- 종료(Terminated)될 때
- 프로세스가 스스로 CPU 소유권 포기하는 방식 (다른 프로세스가 뺏을 수 없음) 
- 강제로 프로세스 중지하지 ❌ 
- 필요한 context switching만 일어나기 때문에 오버헤드가 상대적으로 적음 
- 프로세스 배치에 따라 효율성 차이가 많이 날 수 있음 

## 선점(Preemtive) 스케줄링 
- 실행(running) 상태에서 준비(ready) 상태로 전환(switching)될 때 (ex : intterupt 발생)
- 대기(waiting) 상태에서 준비(ready) 상태로 전환(switching)될 때 (ex : I/O 완료 시)
- 현재 사용하고 있는 프로세스를 알고리즘에 의해 중단시키고 다른 프로세스에 CPU 할당하는 방식 
- CPU 처리 시간이 매우 긴 프로세스의 CPU 독점을 막을 수 있어 효율적으로 운영 가능 
- 빠른 응답시간을 요구하는 시분할 시스템에 유용 
- context switching이 자주 일어나 오버헤드가 커질 수 있음 

</br> 

## 스케줄링 알고리즘 
<img width="500" alt="image" src="https://user-images.githubusercontent.com/63537847/221754124-3d7b476c-7a8a-414a-84ec-a93ad77449a4.png">

### 1️⃣ FCFS (First Come First Served) 
![image](https://user-images.githubusercontent.com/63537847/221756959-d931c9dc-7d5f-430b-bc84-448ee9252bb1.png)

- 비선점형 
- 가장 먼저 요청한 프로세스에 CPU를 할당해주는 방식
- 작성이 간단하고 이해하기 쉬움 
- 평균 대기시간과 응답시간이 길어질 수 있음 
- `convoy effect`(준비 큐에서 오래 대기하는 현상)가 발생할 수 있음 


### 2️⃣ SJF(Shortest-Job-First)
![image](https://user-images.githubusercontent.com/63537847/221757404-25e06f80-c480-4f51-a1bd-4a75f986e12c.png)

- 비선점형 
- FCFS 보완하기 위해 만들어짐 
- CPU burst time의 길이를 고려해서 스케줄링을 결정하는 알고리즘
- 실행되고 있는 프로세스는 끝까지 실행 
- 평균 대기시간 ⬇️
- 다음 프로세스의 CPU burst time을 예측하는 것이 어려움 
- starvation(기아) 문제 발생할 수 있음

### 3️⃣ HRRN(Highest Response Ratio Next)
![image](https://user-images.githubusercontent.com/63537847/221762008-0833fa42-a8e9-4006-b44a-c5b4b2c56e44.png)

- 비선점형 
- 응답률이 가장 높은 프로세스에게 높은 우선순위를 부여하는 방식 
- SJF를 변형한 기법 
  - `aging` 기법 사용 : 대기시간이 긴 프로세스의 우선순위가 높아짐 
- starvation(기아) 문제 방지 가능 
- 다음 프로세스의 CPU burst time을 예측하는 것이 어려움


### 4️⃣ Priority 
![image](https://user-images.githubusercontent.com/63537847/221758075-0afa5553-7707-45d6-afd8-9dfeaf931b0c.png)

- 비선점형 & 선점형 둘 다 존재 
- 각각의 프로세스에는 우선순위 번호가 있고, 가장 높은 우선순위를 가진 프로세스에 CPU 할당 
- starvation(기아) 문제 발생할 수 있음 (우선순위가 낮은 프로세스는 절대 실행되지 않는 문제) 
  - **해결 방법** : `aging` (시간이 지날수록 프로세스의 우선순위를 높여주는 방식) 


### 5️⃣ Round Robin(RR)
![image](https://user-images.githubusercontent.com/63537847/221758412-d02f8a04-7513-4606-9ee7-59fe723261da.png)

- 선점형 
- 각각의 프로세스에 동일한 CPU 할당 시간(Time Quantum)을 부여해서 해당 시간 동안만 CPU를 이용하게 하는 방식 
- 할당된 시간 내에 처리하지 못하면 다음 프로세스로 넘어감 
- 응답시간 빠름
- 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가하는 공정한 스케줄링
- q(할당 시간)가 커지면 FCFS처럼 작동 
- q(할당 시간)가 매우 작아지면 process sharing (n개의 프로세스가 CPU 속도의 1/n 씩으로 작동)
  - context switching이 잦아져 오버헤드 증가 


### 6️⃣ SRTF (Shortest Remaining Time First) 
![image](https://user-images.githubusercontent.com/63537847/221757629-e9780fc5-f385-482c-adfc-ebfa7f18e4a2.png)

- 선점형 
- CPU burst time의 길이를 고려해서 스케줄링을 결정하는 알고리즘
- 현재 실행되고 있는 프로세스의 남은 burst time보다  더 짧은 CPU burst time을 가진 새로운 프로세스가 도착할 경우 CPU를 강제로 회수
- 새로운 프로세스가 도달할 때마다 스케줄링을 다시 하기 때문에 CPU 사용 시간 측정 어려움 
- 평균 대기시간 ⬇️
- starvation(기아) 문제 발생할 수 있음


### 7️⃣ Multilevel-Queue 
<img width="700" alt="image" src="https://user-images.githubusercontent.com/63537847/221759592-6ed1c72b-9a91-4f8b-95d3-fd9f0949e244.png">

- 선점형 
- 여러 개의 준비 큐가 있고, 각각 스케줄링 알고리즘(FCFS, RR 등)을 갖고 있음 
- 우선 순위가 높은 큐에는 작은 Time Quantum을, 낮은 큐에는 큰 Time Quantum을 할당
- 각 큐 사이에서 프로세스가 이동할 수 ❌
- 스케줄링 부담은 적지만 유연성이 떨어짐 
- starvation(기아) 문제 발생할 수 있음

### 8️⃣ Multilevel-Feedback-Queue
![image](https://user-images.githubusercontent.com/63537847/221760560-33ae772e-f835-4a78-b14a-461d2c694bce.png)

- Multilevel Queue와 비슷하지만 각 큐 간에 프로세스가 이동할 수 있음 
- Time Quantum을 다 사용한 프로세스는 밑으로 내려감 (우선순위 ⬇️)
- 짧은 작업에 유리하며 입출력 위주의 작업에 우선권 부여 
- 처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 반환 평균시간 ⬇️ 

</br> 
</br> 

> 참고       
> https://youtu.be/LgEY4ghpTJI      
> https://code-lab1.tistory.com/45        
> https://velog.io/@holim0/4.-CPU-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81#2-7-%EB%8B%A4%EB%8B%A8%EA%B3%84-%ED%94%BC%EB%93%9C%EB%B0%B1-%ED%81%90multi-level-feedback-queue-mfq-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81                   
> [📖면접을 위한 CS 전공지식 노트](http://www.yes24.com/Product/Goods/108887922)         
