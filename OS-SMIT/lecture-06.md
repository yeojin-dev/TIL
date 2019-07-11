# Synchronization

Producer Process, Consumer Process가 있다고 할 때 어떤 공유 변수가 있다고 가정해보자.

어떤 변수에 값을 넣는다고 했을 때 메모리까지 다 쓰지 않았을 수 있다. 

```
Synchronization 문제는 일반적이지 않고 디버깅하기 어렵다.
일주일간 잘 돌다가 갑자기 죽으면 대개는 이쪽 문제이다.
```

## race condition

* 공유 데이터를 동시에 접근하려고 하면 생긴다. 공유 데이터가 완전히 처리 완료될 때까지 독점하고 있어야만 한다.
* OS 입장에서는 다양한 형태의 race condition이 있을 수 있다.

## race condition 해결 방법

### Critical Section

하나의 프로세스만 동작할 수 있는 코드 영역.

1. Mutual Exclusion
2. Progress
3. Bounded Waiting

#### Peterson's Solution

```cpp
do {
    flag[i] == true;  // OS가 atomic하게 처리해야 한다.
    turn = j;  // 여기도
    while (flag[i]) && trun == j) {
        // critical section(preventing interrupts)
    }
    flag[i] = false;
} while (true)
```

* 이 방법은 싱글 프로세서 환경에서만 가능하다.
* flag, turn 변수를 모든 코어가 공유하는데 시간이 걸린다.
* 하드웨어에서 제공하는 atomic(함수 이름 및 데이터타입) 관련 인스트럭션이 존재함.

```cpp
// atomic function
boolean TestAndSet(boolean *target) {
    boolean rv = *target;
    *target = true;
    return rv;
}

do {
    while (TestAndSetLock(&lock))
        ;
    
    // critical section

    lock = false;  // lock은 서로 공유하는 변수

    // remainder section

} while (true)
```

### Mutex

acquire(), release(): 둘 다 system call

```cpp
acquire() {
    while (!available)
        ;
    available = false;
}

release() {
    available = true;
}
```

* spinlock: 아무 것도 하지 않고 루프를 돌면서 대기하는 상태, 싱글 프로세서에서는 의미가 없고 멀티 프로세서 환경에서 좋다. 다른 프로세서가 점유하고 있을 때는 그냥 spinlock을 쓰는 것이 좋다. 굳이 큐를 쓰려고 하면 자원의 낭비가 더 심하다.

* acquire(), release() 시스템 콜은 반드시 atomic - acquire() 도중에 preemption 당하면 큰 문제가 생긴다.

### Semaphores

wait(), signal(): 둘 다 system call, 원하는 숫자만큼 동시에 수행시킬 수 있다.

```cpp
wait() {
    while S <= 0
        ;
    S--;
}

signal() {
    S++;
}
```

* 만약 웹 서버에 1GB 파일을 달라는 요청이 100만개가 온다면? -> 세마포어 사용해서 한번에 처리하는 요청의 개수를 조절할 수 있다.

```
뮤텍스, 세마포어 버그는 재현이 어럽다.
```

## deadlock

P0|P1
--|--
wait(S);|wait(Q);
wait(Q);|wait(S);

* 서로의 뮤텍스를 기다라기다 영원히 기다리게 됨.

## Priorty Inversion Problem

낮은 우선순위의 프로세스가 뮤텍스를 쥐고 있으면 우선순위가 역전될 수 있음.

### Priory Inheritance Protocol

낮은 우선순위의 프로세스가 뮤텍스를 쥐고 있을 때 더 높은 우선순위가 왔지만 wait() 상태가 되었다면 낮은 우선순위의 프로세스는 더 높은 우선순위의 우선순위를 상속받음.

## Classic Problems of Synchronization

### Bounded Buffer Problem

뮤텍스, 세마포어로 처리 가능함.

### Reader-Writer Problem

DB의 경우에는 데이터 읽기가 쓰기보다 훨씬 많다.

쓰기 순간에는 어떤 작업도 병행되면 안된다. (읽기는 자유임.)

#### rw_mutex

쓰기 프로세스를 관리하는 뮤텍스

1. 쓰기는 읽기가 모두 끝날 때 수행 시작할 수 있다.
2. 읽기는 쓰기가 없으면 자유롭게 들어오지만, 쓰기가 있으면 절대 못 들어온다.

```
프로세스의 성격에 따라 다양한 뮤텍스, 세마포어를 설정할 수 있어야만 한다.
```

### Dining-Philosophers Problem

홀수 번째는 왼쪽부터, 짝수 번째는 오른쪽부터 할당받기(deadlock은 해결되지만 bounded는 해결되지 않음)

`monitor`: 자료구조. 구조 안의 프로세스는 동시에 하나만 수행될 수 있다.

## 실제 OS 사례

### Windows Programing

`CRITICAL_SECTION`: 윈도우에서 유저 영역에서 크리티컬 섹션을 사용할 수 있게 해주는 윈도우 제공 변수(OS가 개입하지 않으나 일정 퀀텀 타임을 넘기면 block)

`nonsignaled`, `signaled`: 뮤텍스, acquire() / release() 시스템 콜로 관리한다.

### Linux

`preempt_disable()`, `preempt_eable()`: 싱글 프로세서
`atomic_`: 멀티 프로세서 환경에서 사용하는 세마포어 관리 프로그램

### Solaris

`Adaptive mutex`: 같은 프로세서 환경에서는 block, 다른 프로세서 환경에서는 spinlock
