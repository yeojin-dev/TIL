# CPU Scheduling 2

## Multilevel Queue

* 우선순위에 따라 다른 큐에 등록됨

* 큐의 종류(중요도 높은 순서)
    1. system processes
    2. interactive processes
    3. interactive editing processes
    4. batch processes

* Ready Queue를 여러 개로 분할해서 사용하며, 각 큐는 독립적인 스케줄링
    * 중요한 큐는 RR, 백그라운드 잡은 FCFS

* Queue에 대한 스케줄링이 필요함
    * Fixed priority scheduling: 각 큐의 프로세스가 조정된 우선순위를 받음
    * Time Slice: 각 큐가 CPU 시간을 일정 비율만큼 할당받음

## Multilevel Feedback Queue

* 프로세스의 상태에 따라 다른 큐로 옮겨갈 수 있음
* Aging 구현할 수 있음

## 중요한 패러미터

1. 큐의 수
2. 각 큐의 스케줄링 알고리즘
3. 프로세스를 상위/하위 큐로 보내는 기준
4. 처음 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

## Multiple-Processor Scheduling

* 프로세서가 모두 같으면 큐를 하나로 만들어서 각 프로세서가 가져가게 하면 가능
* 일부 프로세서에 job이 몰리지 않도록 부하를 공유하는 매커니즘이 필요함

### 스케줄링 아키텍처

1. Symmetric Multiprocessing(SMP): 각 프로세서가 알아서 스케줄링
2. Asymmetric Multiprocessing: 하나의 프로세서가 시스템 데이터 접근, 공유 등의 스케줄링을 책임지고 나머지는 거기에 따름

## Real-Time Scheduling

* Hard real-time: 반드시 정해진 시간 안에 프로세스 완료 보장
* Soft real-time: 리얼타임 태스크는 높은 우선순위 보장

## Thread Scheduling

* Local Scheduling: 스레드 라이브러리가 직접 스케줄링
* Global Scheduling: 커널의 스케줄러가 스케줄링

## Algorithm Evaluation

### Queueing Model

* 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산

### Implementation & Measurement

* 실제 시스템에 알고리즘을 구현하여 실제 작업에 대해 성능을 측정, 비교

### Simulation

* 알고리즘을 모의 프로그램으로 작성한 후 trace를 입력으로 하여 결과 비교
