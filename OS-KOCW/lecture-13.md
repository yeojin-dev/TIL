# Process Synchronization 3

## Bounded-buffer Problem

![](https://i2.wp.com/www.codingalpha.com/wp-content/uploads/2016/07/Producer-and-Consumer-Problem-in-C-Programming-e1469612206954.png?zoom=2.625&resize=364%2C227)

### Producer

1. 빈 버퍼가 있는지 확인하고 없으면 대기
2. 공유 데이터에 lock
3. 빈 버퍼에 데이터 입력 및 버퍼 조작
4. lock 제거
5. 데이터가 있는 버퍼 수 증가

### Consumer

1. 데이터 버퍼가 있는지 확인하고 없으면 대기
2. 공유 데이터에 lock
3. 데이터 버퍼에서 데이터 꺼내고 버퍼 조작
4. lock 제거
5. 빈 버퍼 수 증가

### Synchronization variables

1. mutual exclusion 위한 mutex
2. 데이터 버퍼, 빈 버퍼에 대한 semaphore

## Implementation

```c
semephore full = 0, empty = n, mutex = 1;

// Producer
do {
    produce item x
    P(empty);
    P(mutex);
    add item
    V(mutex);
    V(full);
} while (1)

// Consumer
do {
    P(full);
    P(mutex);
    remove item y
    V(mutex);
    V(empty);
} while (1)
```

## Readers-Writers Problem

* DB 읽기/쓰기 상황에서 적용

### Solution

#### 설계

* 하나의 프로세스가 DB에 쓰는 중일 때 다른 데이터가 접근하면 안됨
* read는 동시에 여럿이 해도 됨
* 정리
    1. writer가 DB에 접근 허가를 아직 얻지 못 한 상태에서는 대기하되 reader들은 모두 접근 가능
    2. writer는 대기 중인 reader가 없을 때 DB 접근 가능
    3. 일단 writer가 DB에 접근 중이면 reader 접근 금지
    4. writer가 DB 수정을 마쳐야 reader들의 접근 가능

#### Shared data

1. DB 자체(커넥션)
2. readcount: DB에 연결된 reader 수

#### Synchronization variables

1. mutex
2. db

## Implementation

```c
// shared data, semaphores
int readcount = 0;
semaphore mutex = 1, db = 1;

// writer
P(db);
write DB
V(db);

// reader
P(mutex);
readcount++;
if (readcount == 1) P(db);  // 최초 접근한 reader일 경우
V(mutex);
read DB
P(mutex);
readcount--;
if (readcount == 0) V(db);  // 마지막으로 읽고 있었던 reader의 경우
V(mutex);
```

* writer에서는 기아 현상 발생할 수 있음
    * 우선순위 큐를 만들어서 writer가 너무 오래 기다렸으면 reader를 기다리게 할 수 있음
