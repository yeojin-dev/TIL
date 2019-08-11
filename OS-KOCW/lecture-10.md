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

# Process Synchronization 1

## Race Condition

![Race Condition](https://www.objc.io/images/issue-2/race-condition@2x-8b11b31d.png)

* 메모리를 공유하는 프로세스가 여럿 있을 경우 발생할 수 있음

* 사용자 프로세스가 아닌 커널 프로세스의 경우 큰 문제가 발생할 수 있음

## OS에서 Race Condition은 언제 발생하는가?

* 커널 수행 중 인터럽트 발생 시
* 프로세스가 시스템 콜을 하여 커널 모드로 수행 중인데 컨텍스트 스위칭이 일어날 경우
* 멀티프로세서 환경에서 공유 메모리를 사용할 경우

## Process Synchronization 문제

* 공유 데이터의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다.
* 일관성 유지를 위해서는 협력 프로세스 간의 실행 순서를 정해주는 매커니즘이 필요
* Race Condition: 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황, 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐
* **Race Condition**을 막기 위해서는 concurrent process는 동기화되어야만 함

## Critical Section Problem

* 공유 데이터를 접근하는 코드를 Critical Section이라고 부름
* n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
* 각 프로세스의 코드에는 공유 데이터 접근하는 코드인 Critical Section 존재
    * 하나의 프로세스가 Critical Section에 있을 때 다른 모든 프로세는 Critical Section에 들어갈 수 없어야 함
