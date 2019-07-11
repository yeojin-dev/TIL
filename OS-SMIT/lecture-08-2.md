# Deadlock

## Deadlock Avoidance

동적으로 현재 OS가 자원을 할당해도 되는지를 판단한다.

### Example

어떤 시스템에서 한 자원의 전체 가용 숫자가 12라고 할 때, 프로세스들이 수행을 완료하기 위한 최대 자원의 개수와 현재 할당받은 개수가 아래와 같다고 가정하자.

프로세스|Max. Needs|Current Allocation
------|----------|------------------
P0|10|5
P1|4|2
P2|9|2

이 경우에서 P2가 1개의 자원을 추가로 요청했을 때 할당해도 데드락이 일어나지 않는가?

#### 데드락이 발생한다.

1. P2에게 자원을 하나 할당하면 아래과 같은 상황이 된다.

프로세스|Max. Needs|Current Allocation
------|----------|------------------
P0|10|5
P1|4|2
P2|9|3

2. 현재 남은 자원은 2개인데, 남은 자원 2개로 종료시킬 수 있는 프로세스는 P1뿐이다.
3. P1에게 남은 자원을 2개 할당하고 P2가 종료되면 자원 4개를 반환받을 수 있다.
4. 4개의 자원이 남은 상태에서는 P0, P2 어느 것도 종료시킬 수 없기 때문에 데드락이 발생한다.

### Banker's Algorithm

위와 같은 상황을 미리 예견해서 자원을 할당해도 되는지 판별하는 알고리즘.

#### 자료구조

1. m: 리소스 타입의 숫자
2. n: 프로세스의 개수
3. Available[m]: m 타입의 리소스의 가용 개수
4. Max[n][m]: n 프로세스가 종료하기 위해 필요한 최대 m 타입 리소스의 개수
5. Allocation[n][m]: n 프로세스가 현재 가지고 있는 m 타입 리소스의 개수
6. Need[n][m]: n 프로세스가 앞으로 필요한 m 타입 리소스의 개수(Max - Allocation)
7. Finish[n]: n 프로세스가 종료되는지 판단

#### Safty Check Algorithm

1. Work = Available(값 복사)
2. 모든 Finish 값들이 False
3. Finish[i] == False and Need[i] <= Work[i] 조건을 만족하는(현재 할당받아서 프로세스 종료가 가능한) 프로세스 탐색
4. 3을 만족하는 i에 대해 Work[i] += Available[i] 설정으로 프로세스가 종료 후 자원을 반환했다고 가정함.
5. 3~4번의 과정을 계속 반복한 결과 모든 Finish가 True 값을 갖게 되었다면 데드락이 일어나지 않는다는 뜻임.

#### Resource-Request Algorithm

1. Request <= Need 확인, 만약 False라면 raise
2. Request <= Available 확인, 만약 False라면 프로세스는 대기
3. Request가 반영되었다고 가정, 관련된 값(Allocation, Available, Need)를 바꾸고 Safty Check Algorithm 사용

### Banker's Algorithm 문제점

* 쉽게 구현할 수 있지만 추가비용이 큼
* 은행원(Banker)에게 통신하는 메시지가 너무 많아 은행원(Banker)이 Bottleneck이 될 수 있음
* 할당할 자원이 일정량 존재해야 함
* 최대 자원 요구량을 미리 알아야 함
* 프로세스 들은 유한한 시간 안에 자원을 반납해야 함
