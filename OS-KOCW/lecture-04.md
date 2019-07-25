# Process 1

## 프로세스의 개념

* **Process is a program in execution.**

* 프로세스의 문맥(context)
    * 현대 컴퓨팅 환경은 멀티 프로세스 환경이기 때문에 문맥을 관리하는 것이 필수
    * CPU 수행 상태를 나타내는 하드웨어 문맥(PC 등 각종 레지스터)
    * 프로세스의 주소 공간(code, data, stack)
    * 프로세스 관련 자료구조(PCB, kernel stack)

## 프로세스의 상태

* 프로세스는 상태를 바꾸어가며 실행
    1. running: CPU 자원을 받아서 인스트럭션이 실행되고 있는 상태
        * user mode: 사용자 프로세스 수행
        * monitor mode: 시스템 콜, 인터럽트 수행을 기다림(완료 후에는 다시 user mode)
    2. ready: CPU 자원을 기다리는 상태
    3. blocked: CPU 자원을 받는다고 해도 당상 실행될 수 없는 상태(I/O 대기 등)
        * 각 자원들에 대한 대기 큐(커널 메모리의 data 영역에 존재)에 등록됨
    4. new: 프로세스 생성 중
    5. terminated: 수행이 끝난 상태
    6. suspended: 외부 이유(사용자의 일시 정지, 메모리에 너무 많은 프로세스 적재 등)로 프로세스의 수행이 중단된 상태로 프로세스가 통째로 swap됨
        * blocked는 프로세스가 요청한 이벤트가 만족되면 ready로 이동하나, suspended는 외부에서 직접 resume해주어야 running/blocked 상태로 올 수 있음

## 프로세스 상태도

![프로세스 상태도](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_02_ProcessState.jpg)

## PCB

![PCB](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/process-table.jpg)

* 운영체제가 각 프로세스를 관리하기 위해 프로세스 당 유지하는 정보

1. OS 관리를 위해 사용하는 정보
    * 프로세스 상태
    * PID
    * 스케줄링 정보
    * priority

2. CPU 수행 관련 정보
    * PC
    * 레지스터

3. 메모리 관련
    * code, data, stack 위치 정보

4. 파일 관련
    * file descriptors

## 문맥 교환

* CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
* CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
    1. CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
    2. CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴

* 시스템 콜이나 인터럽트 발생 시 반드시 문맥 교환이 일어나는 것은 아님 - 시스템 콜, 인터럽트 종료 후 원래 프로세스로 돌아온다면 문맥 교환-cache memory flush-은 필요하지 않음(CPU 수행 정보 등의 저장은 일어남)

## 프로세스를 스케줄링하기 위한 큐

* job queue
* ready queue
* device queues

## 스케줄러

1. long-term scheduler(job scheduler)
    * 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
    * 프로세스에 자원을 주는 문제 담당
    * degree of muliprogramming 제어
    * time sharing system에는 보통 장기 스케줄러가 없음(무조건 ready queue로 이동)

2. short-term scheduler(CPU scheduler)
    * 어떤 프로세스를 다음에 running 상태로 만들지 결정
    * 프로세스에 CPU를 할당해주는 문제
    * 충분히 빨라야 함

3. medium-term scheduler(swapper)
    * 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
    * 프로세스에게서 메모리를 빼앗는 문제(suspended 상태로 만듦)
    * degree of muliprogramming 제어
    * long-term scheduler 대신 역할을 함
