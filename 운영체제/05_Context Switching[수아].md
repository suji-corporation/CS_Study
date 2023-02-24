## PCB(Process Coontrol Block) 
![image](https://user-images.githubusercontent.com/63537847/221095715-465e2363-d30e-40fb-9474-f6a9513186fa.png)

- 운영체제 커널 자료구조 (프로세스의 상태 정보 저장) 
- 운영체제가 프로세스를 표현한 것 
- 프로세스가 생성될 때 고유의 PCB 생성되고 프로세스가 완료/종료되면 PCB는 삭제됨 
- 프로세스가 CPU 점유해서 작업하다가 반환해야 할 때, 처리하던 작업을 PCB에 저장하고 이후에 다시 CPU를 점유하게 되면 저장해놨던 정보를 PCB로부터 다시 받아와서 작업 진행 
- 프로세스 `상태 관리`와 `문맥 교환(context switching)`위해서 필요 
- 빠르게 PCB에 접근하기 위해 Process Table 사용              
  <img width = "350" src=https://user-images.githubusercontent.com/63537847/221096614-24007ef6-81b3-444c-84db-90d4048eb8fd.png>

### PCB 정보     
**1️⃣ Pointer**         
프로세스의 현재 위치 저장하는 포인터 정보 

**2️⃣ Process State**            
프로세스의 상태(생성(New), 준비(Ready), 실행(Running), 대기(Waiting), 종료(Terminated)) 저장

**3️⃣ Process Number(PID)**            
프로세스 식별자 저장. 고유 ID

**4️⃣ Program Counter**              
프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장

**5️⃣ Registers**              
누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보

**6️⃣ Memory Limits**             
운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보           
ex) 페이지 테이블, 세그먼트 테이블 등

**7️⃣ Lists of Open Files**           
프로세스를 위해 열린 파일 목록


</br>

## Context Switching(문맥교환) 
- CPU/코어에서 실행 중이던 프로세스/스레드가 다른 프로세스/스레드로 교체되는 것 
  - `프로세스 → 프로세스 교체` : A프로세서의 스레드에서 B프로세서의 스레드로 교체되는 것 
- **context** : 프로세스/스레드의 상태, CPU, 메모리 등 
- 여러 프로세스/스레드 `동시에 실행`하기 위해 context switching함
- `OS 커널(kernel)`에 의해서 실행됨 

### ❓ 언제 발생하는가? 
- 멀티태스킹(Multitasking)
  - 실행 가능한 프로세스들이 운영체제의 스케줄러에 의해 조금씩 번갈아가며 수행되는 것
  - 빠르게 문맥교환이 일어나기 때문에 사용자는 프로세스가 동시에 처리된다고 느낌 
- 인터럽트 핸들링(Interrupt handling) 
  - 인터럽트가 발생한 경우에 CPU가 처리 
    - ex) I/O request, time slice expired(CPU 사용시간이 만료), fork a child(자식 프로세스 생성), wait for an interrupt(인터럽트 처리 대기)
- 사용자 모드와 커널 모드 전환(User and kernel mode switching)
  - 사용자와 커널 모드 전환은 Context Switch가 필수는 아니지만 운영체제에 따라 발생할 수 있음

### ⚙️ 진행 과정 
![image](https://user-images.githubusercontent.com/63537847/221099506-d6cddad7-d6a5-47d7-9ab6-a25e4d0240cb.png)

1. 요청 발생: 인터럽트 또는 트랩에 의한 요청이 발생.(트랩은 소프트웨어 인터럽트)
2. PCB에 저장: 운영체제는 현재 실행중인 프로세스(P0)의 정보를 PCB에 저장.
3. CPU 할당: 운영체제는 다음 프로세스(P1)의 정보를 PCB에서 가져와 CPU를 할당.


</br>
</br>

> 참고      
> https://youtu.be/Xh9Nt7y07FE       
> https://yoongrammer.tistory.com/52     
> https://spurdev.tistory.com/13           
> https://hwan-shell.tistory.com/197
