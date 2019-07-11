# Process

## Process Concept

* 요즘은 멀티프로세스보다 멀티스레드

1. Shared Memory
2. Socket

### Stack

* 프로세스 메모리에서 스택은 위에 위치한다. 지역 변수들이 스택에 저장된다.
* 스택은 크기가 정해져 있다.

### State of Process

1. new: being created
2. running: being executed
3. waiting: waiting for some events to occur
4. ready: waiting to be assigned to a processor
5. terminated: being finished

### 좀비 프로세스

* 부모 프로세스는 끝났는데 자식 프로세스는 살아있는 경우
* OS가 init process 생성 후 새로운 부모 프로세스로 활용
* 그 후 자식 프로세스가 끝나면 init process도 종료

##### 윈도우에서 실제 사용하고 있는 창의 priorty가 7배 정도 높다

### PCB(Process Control Block)

`OS는 PCB만 본다!`

* 일종의 메모리 구조
* PCB를 만든다는 것이 곧 프로세스의 OS 등록
* 연결 리스트로 구현되어 있음
	* PC에서는 괜찮은 설계
	* 대형 컴퓨터에서는 PCB가 매우 많아서 부적절한 설계

1. Process State
2. Program Counter
3. CPU Register: 정확하게는 해당 스택의 스택포인터(데이터 자체는 스택에 존재)
4. CPU Scheduling Info: Priorty, Prev/Next PCB pointers, etc.
5. Memory Mgnt. Info
6. Accounting Info
7. I/O State Info

#### OS마다 어떤 PCB 구조를 가지고 있을까?

## Features of Process

### Process Scheduling

* ready queue: CPU 사용을 기다리는 큐
* device queue: I/O를 기다리는 큐
* running queue는 없다: 하나만 존재
* CPU Bound, I/O Bound 프로세스를 적절히 섞으면 컴퓨터 퍼포먼스가 좋아진다.

#### Process Scheduler는 빨라야 한다.

* long-term(or job) scheduler: 요즘에는 잘 쓰지 않는다
* short-term scheduler: CPU Scheduling
* medium-term scheduler: 메모리 스왑을 위한 스케쥴러
	* Swapping Space
	* 메모리 스왑 알고리즘은 다소 시간이 걸려도 효율적인 것이 좋음

### Context Switching

* CPU가 프로세스를 바꾸는 것
* sleep(0) 러닝 프로세스를 바로 레디 큐로 보내기(다른 스레드에 양보)

### HyperThreading

* CPU안에 레지스터가 1set이 아니라 여러 set - Context Switching 로드가 없음
* 하드웨어적으로 콘텍스트 스위칭을 해주는 것
* 코어가 많으면 캐시 문제가 생김 - 하이퍼스레딩을 끄면 더 빨라질 수도 있다?

### Process Creation

* pid < 0 -> 포크 실패
* pid == 0 -> 내가 포크한 자식 프로세스
* pid > 0 -> 원래 프로세스 상태

* 부트 로더가 처음 올린 프로세스가 계속 포크를 하면서 OS 프로세스가 만들어진다.

* wait(NULL) -> 자식 프로세스가 처리되고 나서 실행하겠다는 의미

#### Windows

* CreateProcess: 프로세스 만들기
* WaitForSingleObject(pi.hProcess, INFINITE): 자식 프로세스(pid는 hProcess) 종료를 무한히 기다림
* PROCESS_INFOMATION: 프로세스 정보가 담긴 구조체

## Inter-Process Communication

### IPC model

* Shared Memory: 프로세스 A가 프로세스 메모리 중 일부를 공유한 것. 다른 프로세스는 공유 메모리의 포인터를 받아서 접근할 수 있다. 대용량에 좋다.
* Message Passing: OS가 프로세스에 정보를 전달하는 것(system call)
	* Direct passing: 직접 보내기. 받을 때까지 기다린다. TCP(파일 트랜스퍼). (Synchronous-Blocked)
	* Indirect passing(메일박스): 버퍼를 이용하기. 언제 받아갈지는 모르지만 기다리지 않고 다음 연산 수행함. UDP(옛날 아프리카, 원격제어 프로그램). (Asynchronous-Unblocked)
* Automatic, Explicit Buffering: 버퍼를 정해놓느냐, 아니면 OS에 위임하느냐
* 요즘에는 프로세스간 통신 자체를 잘 안씀 - 멀티스레드로 처리

#### POSIX

* POSIX는 UNIX간 표준 시스템 콜
	* shm_open(): POSIX로 공유 메모리 할당

#### Windows - ALPC(Advanced Local Procedure Call), RPC(Remote Procedure Call)

* RPC: 원격 시스템 콜을 부르는 프로토콜
* ALPC: 프로세스간 메시지 통신

### Socket

```
Socket is an end point of communication.
```
