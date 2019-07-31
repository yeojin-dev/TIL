# Process Management 2

### COW(Copy-On-Write)

* 부모 프로세스가 자식 프로세스를 만들 때 자식 프로세스와 부모 프로세스가 같은 데이터를 가지고 있을 때는 부모의 데이터를 그대로 공유
* 자식 프로세스의 내용이 수정(다른 함수 호출, 변수 입력 변경 등)되었을 때 자식 프로세스의 데이터를 메모리에 새로 올림

## fork() 시스템 콜

* A process is created by the fork() system call.
    * creates a new address space that is a duplicate of the caller.

```c
int main()
{
    int pid;
    pid = fork();
    // fork() 함수로 생성된 자식 프로세스는 여기서부터 실행됨(부모 프로세스의 context를 그대로 가져오기 때문)
    if (pid == 0)  // 자식 프로세스의 pid 값은 0
        printf("\nHello, I am child!\n");
    else if (pid > 0)
        printf("\nHello, I am parent!\n");
}
```

## exec() 시스템 콜

* A process can execute a different program by the exec() system call.
    * replaces the memory image of the caller with a new program.

```c
int main()
{
    int pid;
    pid = fork();
    // fork() 함수로 생성된 자식 프로세스는 여기서부터 실행됨(부모 프로세스의 context를 그대로 가져오기 때문)
    if (pid == 0)  // 자식 프로세스의 pid 값은 0
    {
        printf("\nHello, I am child!\n");
        execlp("/bin/date", "/bin/date", (char*) 0);
        print("\nHello!\n");  // 이 코드는 실행되지 않음
    }
    else if (pid > 0)
        printf("\nHello, I am parent!\n");
}
```

## wait() 시스템 콜

* 프로세스 A가 wait() 시스템 콜을 호출하면 커널은 child가 종료될 때까지 프로세스 A를 sleep, child process 종료되면 커널은 프로세스 A를 깨움(ready)

```c
int main()
{
    int pid;
    pid = fork();
    // fork() 함수로 생성된 자식 프로세스는 여기서부터 실행됨(부모 프로세스의 context를 그대로 가져오기 때문)
    if (pid == 0)  // 자식 프로세스의 pid 값은 0
    {
        printf("\nHello, I am child!\n");
        execlp("/bin/date", "/bin/date", (char*) 0);
        print("\nHello!\n");  // 이 코드는 실행되지 않음
    }
    else if (pid > 0)
    {
        wait();  // 자식 프로세스가 종료되기까지 기다림
        printf("\nHello, I am parent!\n");
    }
}
```

### 프로세스의 종료

1. 자발적 종료
    * 마지막 statement 수행 후 exit() 시스템 콜을 통해
    * 프로그램에 명시적으로 적어주지 않아도 main() 함수가 리턴되는 위치에 컴파일러가 넣어줌

2. 비자발적 종료
    * 부모 프로세스가 자식 프로세스를 강제 종료시킴
        * 자식 프로세스가 한계치를 넘어서는 자원 요청
        * 자식에게 할당된 태스크가 더 이상 필요하지 않음
    * 키보드로 kill, break 등을 친 경우
    * 부모가 종료하는 경우
        * 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

## 프로세스 간 협력

### 독립적 프로세스(Independent Process)

* 프로세스는 각자의 주소 공가늘 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못 함

### 협력 프로세스(Cooperating Process)

* 프로세스 협력 매커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

#### 프로세스 간 협력 매커니즘(IPC, InterProcess Communication)

1. 메시지를 전달하는 방법
    * message passing: 커널을 통해 메시지 전달

2. 주소 공간을 공유하는 방법
    * shared memory: 서로 다른 프로세스 간에도 일부 주소 공간을 공유하는 shared memory 매커니즘 존재
    * thread: thread들은 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 프로세스를 구성하는 스레드 간에는 주소 공간을 공유하므로 협력이 가능

## Message Passing

* Message System: 프로세스 사이에 공유 변수룰 일체 사용하지 않고 통신하는 시스템

1. Direct Communication
    * 통신하려는 프로세스를 명시함: Send(Process, Message) -> Receive(Process, Message)

2. Indirect Communication
    * mailbox(또는 port)를 통해 메시지를 직접 전달

## CPU and I/O Bursts in Program Execution

![CPU and I/O Bursts in Program Execution](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_01_CPU_BurstCycle.jpg)

* CPU bound job, I/O bound job 2개가 서로 섞여있기 때문에 적절한 스케줄링이 필요함
    * I/O bound job은 유저 인터랙션이 빈번한 경우가 많기 때문에 효과적인 스케줄링이 중요

## CPU Scheduler & Dispatcher

* CPU Scheduler: Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 선택

* Dispatcher: CPU의 제어권을 CPU Scheduler에 의해 선택된 프로세스에게 넘김(context switching)

* CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우

    1. Running -> Blocked(I/O 요청 시스템 콜)
    2. Running -> Ready(timer interrupt)
    3. Blocked -> Ready(I/O 완료)
    4. Terminate
    * 1~3 스케줄링은 non-preemptive, 4번은 preemptive
