# Deadlock 1

## The Deadlock Problem

### Deadlock

* 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태

### Resource

* 하드웨어, 소프트웨어 등을 포함하는 개념
* I/O 기기, CPU, 메모리, 세마포어 등
* 프로세스가 자원을 사용하는 절차: Request, Allocate, Use, Release

## Deadlock 발생의 4가지 조건

1. Mutual Exclusion

* 매 순간 하나의 프로세스만이 자원을 사용할 수 있음

2. No Preemption

* 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음

3. Hold and Wait

* 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음

4. Circular Wait

* 자원을 기다리는 프로세스 사이에 사이클이 형성되어야 함

![Deadlock](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2015/06/deadlock.png)

## Resource-Allocation Graph

![Resource-Allocation Graph](https://www.cs.odu.edu/~cs471w/spring10/lectures/Deadlocks_files/image002.jpg)

* 그래프에 cycle이 없으면 데드락이 아님
* 그래프에 cycle이 존재한다면
    1. if only one instace per resource type, then deadlock
    2. if several instances per resource type, possibility of deadlock
* 위 그래프에서 P3 프로세스가 R2 자원을 기다린다면 데드락이 발생함

## 데드락의 처리 방법

1. Deadlock Prevention

* 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

2. Deadlock Avoidance

* 자원 요청에 대한 부가적인 정보를 이용해서 데드락의 가능성이 없는 경우에만 자원을 할당
* 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원 할당

3. Deadlock Detection and Recovery

* Deadlock 발생은 허용하되 그에 대한 datection 루틴을 두어 발견 시 회복

4. Deadlock Ignorance

* 데드락 발생을 그대로 놔둠
* 대부분의 현대 OS가 채택하고 있음
* 위 3가지 방법이 일으키는 오버헤드가 크기 때문

## Deadlock Prevention

1. Mutual Exclusion

* 공유해서는 안 되는 자원의 경우 반드시 성립해야만 함

2. Hold and Wait

* 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 함
* 프로세스가 시작 시 모든 필요한 자원을 할당받도록 하는 방법
* 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청

3. No Preemption

* 프로세스가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
* 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작됨
* state를 쉽게 save하고 restore할 수 있는 자원에서 주로 사용(CPU, 메모리)

4. Circular Wait

* 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당

### 문제점

* utilization, throughput 감소
* starvation 문제


## Deadlock Avoidance

* 자원 요청에 대한 부가정보를 이용해서 자원 할당이 데드락으로부터 안전한지를 동적으로 조사해서 안전한 경우에만 할당

### safe state

* 시스템 내의 프로세스들에 대한 safe sequence가 존재하는 상태

### safe sequence

* 프로세스의 sequence<P1, ..., Pn>이 safe하려면 Pi(1<=i<=n)의 자원 요청이 "가용 자원 + 모든 Pj(j<i)의 보유 자원"에 의해 충족되어야 함
* 조건을 만족하면 다음 방법으로 모든 프로세스의 수행을 보장
    * Pi의 자원 요청에 즉시 충족될 수 없으면 모든 Pj(j<1)가 종료될 때까지 기다림
    * Pi-1이 종료되면 Pi의 자원 요청을 만족시켜 수행함

### Resource-Allocation Graph 알고리즘

![Resource-Allocation Graph Algorithm](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter7/7_09_TwoGraphs.jpg)

* cycle이 생기는지 파악하고 생길 것으로 파악되면 자원을 할당하지 않음
* O(n^2) 시간복잡도를 가짐

### Banker's Algorithm

* 리소스 타입 별로 인스턴스가 복수일 경우 적용
* 어떤 프로세스에 가용 자원을 할당했을 때 데드락이 발생할지 판단하는 알고리즘
* 시간복잡도는 n개의 프로세스, m개의 자원 타입의 경우에 O(n*n*m)

![Banker's Algorithm](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2016/01/safety.png)

* 위 경우에 `P3 -> P1 -> P2 -> P4 -> P0` 세이프 시퀀스가 존재하기 때문에 데드락이 발생하지 않음

* 실제 OS 환경에서는 프로세스와 자원의 개수가 매우 많기 때문에 오버헤드가 큼
