# CPU 스케줄링

## Basic Concepts

프로세스는 대개 2~5ms 정도 CPU를 사용하다가 wait queue로 이동함.

* CPU 인스트럭션 중 load, store 명령어는 OS가 강제적으로 프로세스를 wait queue로 보내버림.

**효율적으로 프로세스 관리를 하려면 어떻게 해야할까?**

### burst

* CPU burst: CPU 사용시간
* I/O burst: I/O 대기시간, CPU를 사용하지 않지만 프로세스가 놀고 있는 시간이라고 볼 수는 없음

## CPU 스케줄러

### Preemptive Scheduling(선점 스케줄링)

더 높은 순위의 프로세스가 발생했을 때, OS가 현재 실행 중인 프로세스로부터 CPU 사용 권한을 강제로 빼앗아오는 스케줄링(현재 대부분의 OS가 선점 스케줄링)

### 스케줄러가 결정하는 것

1. running -> wait: I/O 요청 또는 wait() 시스템 콜
2. running -> ready: 인터럽트, 퀀텀 타임 만료
3. wait -> reday: I/O 완료 또는 wait() return
4. termination

`1, 4번만 가능한 스케줄러는 Non-preemptive, 1~4번까지 모두 가능하면 Preemptive`

### Synchronization Problem

```
I/O 요청으로 system call 발생(파일 읽기 요청은 수십억 사이클이 걸릴 수도 있음)
```

1. 어떤 프로세스가 연산을 한다. 연산의 결괏값은 10이다.
2. 10을 메모리(주소 A)에 써야 하는데 쓰기 전에 퀀텀 타임이 만료되어 wait queue로 이동한다.
3. 다음 차례의 프로세스 역시 연산을 한다. 연산의 결괏값은 5이다.
4. 이 프로세스는 퀀텀 타임 이전에 메모리(주소 A)에 쓰기까지 완료되었다.
5. 다시 이전 프로세스를 CPU가 수행할 때 이전에 쓰지 못 했던 10을 메모리에 쓴다.

같은 주소의 메모리를 두 프로세스가 사용할 경우 동기화 문제가 발생할 수 있음.

```
동기화 문제의 해결책

1. Mutex
2. Semaphore
3. Critical Section
```

### Dispatcher

* Context Switching
* Switching to user mode
* Jumping to the proper location in the newly loaded program

#### dispatch latency

디스패치하는데 걸리는 시간. conflicts 부분과 dispatch 부분으로 나눌 수 있다.

* conflicts: 이전 프로세스가 사용하는 리소스(Mutex 포함)를 반환 처리하는 작업

## CPU 스케줄러를 평가하는 기준

1. throughtput: 단위 시간 당 프로세스 처리 수
2. waiting time: ready queue에 머무르는 시간
3. turnaround time: 하나의 프로세스가 끝날 때까지 걸리는 시간
4. response time: 지정한 프로세스가 응답하기까지 걸리는 시간

```
기준은 시스템의 성격마다 다를 수 있다.
```

## 스케줄링 알고리즘

### 1. FCFS

* 먼저 올라온 프로세스를 먼저 실행
* waiting time이 좋지 않다.
* Non-preemptive

### 2. Shortest Job First

* 가장 짧은 CPU burst를 가진 프로세스부터 실행하기
* 가장 짧은 AWT(Average of Waiting Time)를 갖고 있지만 오버헤드가 있다.
* Non-preemptive, Preemptive 둘 다 가능

#### 어떻게 미래의 CPU burst를 추측할 수 있을까?

exponential average를 계산한다.

t <sub>n+1</sub> = a * T <sub>n</sub> + (1 - a) * t <sub>n</sub>

* T <sub>n</sub>: n번째 CPU burst
* t <sub>n</sub>: 예측된 n번째 CPU burst
* a: weight for actual CPU brust

### 3. Priortiy Scheduling

* a more general case of SJF
* priority 값이 작을수록 먼저 실행함
* 프로세스의 남은 시간의 역수를 priority로 하면 SJF와 같아짐
* 모든 프로세스의 priority 값이 같으면 Round Robin

```
Video Viewer

1. Video CODEC
2. Audio CODEC

처리량은 1번이 더 크기 때문에 더 높은 priority를 주는 프로그래밍을 해야만 한다.
```

### 4. Round Robin

* 모든 프로세스가 퀀텀 타임만큼 CPU를 사용
* 일반적으로 퀀텀 타임은 10~100ms 정도임
* 퀀텀 타임이 매우 크면 FIFO와 같아짐
* 서버와 같은 시분할 시스템에서 중요하게 사용함

#### Context Switching

RR을 사용하고 있지만 프로세스마다 Dispatch Latency가 다르면 실제 CPU burst가 다를 수 있다.

### 5. Multilevel Queue Scheduling

* 프로세스들을 카테고리화하여 관리함
* 구현 방법은 다양하지만 리눅스의 경우 각 큐는 RR을 기본으로 하되, 중요한 큐일수록 퀀텀 타임을 길게 주는 방법을 사용함
* aging: 기아 현상을 방지하기 위해 오랜 시간 기다린 프로세스의 경우 높은 priority를 주거나 더 상위의 큐로 올려줌

#### MQS에서 고려해야만 하는 사항

1. The number of queue
2. The scheduling algorithm of each queue
3. The methods to upgrade or demote processes from one queue to another
4. The methods to determine which queue a process enters initially

## Thread Scheduling

* 프로세스 스케줄러는 커널 스레드만을 스케줄링한다.
* 현재는 커널 스레드와 스레드가 1:1 매핑되어 있어서 특별히 더 다른 점음 없음
* context switching에서 스레드가 커널 스레드보다 더 가볍다.

```
리눅스에서는 프로세스, 스레드 구별 없이 그냥 task라고 표현한다.
```