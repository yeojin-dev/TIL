# Process Synchronization 1

## Initical Attempts to Solve Problem

```c
do {
    entry section
    critical setion
    exit section
    remainder section
} while(1)
```

* 동기화를 위한 변수가 필요함

## 프로그래 해결법의 충족 조건

### Mutual Exclusion

* 프로세스가 크리티컬 섹션 부분을 수행 중이면 다른 모든 프로세스들은 크리티컬 섹션에 들어가는 것이 불가능

### Progress

* 아무도 크리티컬 섹션에 있지 않은 상태에서 크리티컬 섹션에 들어가고자 하는 프로세스가 있으면 크리티컬 섹션에 들어갈 수 있어야 함

### Bounded Waiting

* 프로세스가 크리티컬 섹션에 들어가려고 요청한 이후부터 요청이 허용될 때까지 다른 프로세스들이 크리티컬 섹션에 들어가는 횟수에 한계가 있어야 함
    * starvation 현상이 없어야 한다는 의미

### 가정

* 모든 프로세스들의 수행 속도는 0보다 큼
* 프로세스들 간의 상대적인 수행 속도는 가정하지 않음

## Algorithm 1

```c
do {
    while (turn != 0)
    critical section
    turn = 1
    remainder section
} while (1)
```

* turn 변수는 자신이 크리티컬 섹션에 들어갈 수 있는지를 의미함

* Mutual Exclusion 만족하지만 Progress 만족하지 않음(교대로 주고받는 상황인데 하나의 프로세스만 빈번하게 들어간다면 문제)

## Algorithm 2

```c
do {
    flag[i] = true
    while (flag[j])
    critical section
    flag[i] = false
    remainder section
} while (1)
```

* i 값은 프로세스의 개수

* flag[i], flag[j] 모두 true 값을 가지고 있다면 누구도 진입하지 못 하는 문제가 있음

## Algorithm 3(Peterson's Algorithm)

```c
do {
    flag[i] = true
    turn = j
    while (flag[j] && turn == j)
    critical section
    flag[i] = false;
    remainder section
} while (1)
```

* 모든 조건을 만족함

* Bush Waiting(spinlock) 문제: `flag[j] && trun == j` 조건을 계속 검사하면서 무의미하게 CPU를 사용

## Synchronization Hardware

* 하드웨어적으로 test & modify 작업을 atomic 속성으로 할 수 있다면 바로 수행할 수 있음

```c
do {
    while (test_and_set(lock))  // test_and_set 함수는 값을 읽은 후 변수를 1로 설정해줌(atomic)
    critical section
    lock = false;
    remainder section
}
