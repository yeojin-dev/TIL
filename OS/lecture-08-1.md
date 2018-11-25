# Deadlock

데드락이란 어떤 프로세스가 블록 상태인 자원의 영향으로 자신의 상태를 바꿀 수 없는 상태를 말한다.

## Deadlock의 조건

1. Mutual Exclusion: 어떤 리소스는 동시에 여러 프로세스에 자원을 할당할 수 없다.
2. Hold and Wait: 프로세스가 어떤 자원을 가지고 있는 상태에서 다른 자원을 요청할 수 있다.
3. No Preemption: OS에 의해 강제로 wait queue로 이동할 수 없다.
4. Circular Wait: 서로 순환적으로 리소스를 요청하고 있다.

## Deadlock Characterization

데드락은 resource-allocation graph를 통해 파악할 수 있다.

![resource-allocation graph](https://www.cs.odu.edu/~cs471w/spring11/lectures/Deadlocks_files/image004.jpg)

위 그래프처럼 노드간 순환 구조가 나타난다면 데드락 상태임을 알 수 있다.

## Method of Handling Deadlocks

### Deadlock Prevention

4가지 데드락의 조건이 성립하지 않도록 설계하는 방법

#### Mutual Exclusion

* 리소스의 고유한 특성이기 때문에 조정할 수 없다.

#### Hold and Wait

* 프로세스가 어떤 자원을 갖고 있는 상태에서는 다른 자원을 요청하지 못 하게 만들면 데드락 발생을 막을 수 있다.
* 하지만 그만큼 리소스 활용도가 낮아진다.

#### No Preemption

* CPU, 메인 메모리처럼 리소스가 반환되어도 쉽게 복구할 수 있는 하드웨어가 아니면 사실상 불가능하다.

#### Circular Wait

* 리소스마다 임의의 순서를 정하고 한쪽 방향으로만 리소스를 사용할 수 있도록 설계하면 막을 수 있다.
* 하지만 그만큼 리소스 활용도가 낮아진다.

### Deadlock Detection

아래와 같은 wait-for graph를 통해서 파악할 수 있다.

![wait-for graph](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter7/7_09_TwoGraphs.jpg)

위 그래프처럼 순환 구조가 나타나면 현재 데드락 상태임을 알 수 있다. 하지만 이를 파악하기 위해서 OS가 사용해야만 하는 시스템 리소스가 매우 크다.

### Deadlock Recovery

특정 프로세스를 종료시키고 자원을 빼앗는 방법. 아래와 같은 것들을 고려해야만 한다.

1. 어떤 프로세스를 종료시킬 것인가?
2. 종료시키는 프로세스의 Rollback 처리는?
3. Starvation 문제가 발생하지는 않는가?
