# Process Synchronization 2

## Semaphores

* 앞의 방식들을 추상화시킴

### Semaphore S

* integer variable
* 아래 두 atomic 연산에 의해서만 접근 가능

```
P(S):   while (S<=0) do no-op;  // wait
        S--;

V(S):   S++;
```

## Critical Section of n Processes

```c
semaphore mutex;

do {
    P(mutex);
    critical section
    V(mutex);
    remainder section
} while (1)
```

## Block/Wakeup Implementation

* 세마포어 정의

```
typedef struct
{
    int value;  // 세마포어
    struct process *L;  // 프로세스 대기 큐
} semaphore
```

* block, wakeup 정의

1. block

* 커널은 block 호출한 프로세스를 suspend, 이 프로세스의 PCB를 세마포어의 대기 큐에 넣음

2. wakeup

* block 프로세스 P를 wakeup(PCB를 ready queue로 옮김)

## Implementation

```
P(S):   S.value--;
        if (S.value < 0)
        {
            block();
        }

V(S):   S.value++;
        if (S.value <= 0)  // 0보다 작으면 대기 큐가 프로세스가 있다는 뜻이고 0보다 크면 자원에 여유가 있다는 의미
        {
            wakeup();
        }
```

* spinlock 현상이 없음


## Busy-wait vs. Block/wakeup

* 각자의 오버헤드가 존재함
* 크리티컬 섹션이 크면 Block/wakeup 방식이 효율적, 작으면 컨텍스트 스위칭이 더 비효율적일 수 있음
* 멀티코어 방식에서는 Busy-wait 방식이 더 나을 수도 있음(프로세스 시간의 일부라도 CPU 사용이 가능해짐)
* 일반적으로는 Block/wakeup 방식이 더 좋음

## 세마포어와 뮤텍스

* 자원이 하나이면 뮤텍스
* 자원이 여럿이면 세마포어
* 뮤텍스, 세마포어는 본질적으로 같은 것

## Deadlock and Starvation

### Deadlock

* 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상

* 자원(세마포어)을 얻는 순서를 지정하면 데드락 현상을 예방할 수 있음

### Starvation

* 프로세스가 suspend 이유에 해당하는 세마포어 큐에서 빠져나올 수 없는 현상
