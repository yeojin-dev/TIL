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
