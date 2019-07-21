# System Structure & Program Execution 1

![컴퓨터 시스템 구조](https://bournetocode.com/projects/AQA_A_Theory/pages/img/Computer-Components.png)

* 다이어그램에 없는 구성 요소들
    * mode bit: OS 프로세스인지 아닌지 여부를 알려주는 비트
    * interrupt line: CPU에 인터럽트가 발생했는지 알려주는 데이터 패스
    * DMA controller: I/O의 버퍼 데이터를 메모리에 적재해주는 컨트롤러로 작업 완료 시 CPU에 인터럽트 처리를 요청
    * timer: 특정 프로세스가 CPU를 독점하는 것을 막기 위해 CPU 할당 시간(보통 수십 ms)을 지정하고 interrupt line을 통해 CPU에게 인터럽트 처리를 요청
    * I/O controller: I/O에 속해 있는 컨트롤러
    * I/O buffer: I/O에 속해 있는 전용 메모리

## mode bit

* 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치가 필요

* mode bit 통해 하드웨어적으로 2가지 mode의 operation 지원
    1. 1-사용자 모드: 사용자 프로그램 수행
    2. 0-모니터 모드(커널 모드)): OS 코드 수행

* 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 **특권 명령**으로 규정
* 인터럽트, 예외 발생 시 하드웨어가 mode bit=0 처리
* 사용자 프로그램에게 CPU를 넘기기 전에 model bit=1 처리

## timer

* 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
* 타이머는 매 클럭 때마다 1 감소하며 0이 되면 인터럽트 발생
* CPU를 특정 프로세스가 독점하는 것으로부터 보호
* 타이머는 time sharing 구현을 위해 널리 이용되며 현재 시간을 계산하기 위해서도 사용

## device controller

* 해당 I/O 장치를 관리하는 작은 CPU
    * 자체 레지스터를 가지고 있음
* I/O는 실제 장치와 로컬 버퍼 사이에서 일어남
* device controller는 I/O 종료 시 DMA controller를 통해 CPU에 인터럽트 요청
* device driver: OS 코드 중에서 각 장치 별 처리 루틴(소프트웨어)

## 입출력의 수행

* 모든 입출력 명령은 특권 명령
* 사용자 프로그램은 어떻게 I/O를 하는가?
    * 시스템 콜: 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것(I/O도 포함됨)
    * trap을 이용하여 인터럽트 벡터의 특정 위치로 이동
    * 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    * 올바른 I/O 요청인지 확인 후 I/O 수행
    * I/O 완료 시 제어권을 시스템 콜 다음 명령으로 옮김

## 인터럽트

* 인터럽트 당한 시점의 레지스터와 program counter 정보를 저장한 후 CPU의 제어를 인터럽트 처리 루틴에 넘김

* 인터럽트의 의미
    1. interrupt: 하드웨어가 일으킨 인터럽트
    2. trap: 소프트웨어가 일으킨 인터럽트
        * exception
        * syscall

* 인터럽트 벡터: 해당 인터럽트의 처리 루틴 주소를 가지고 있음
* 인터럽트 처리 루틴: 해당 인터럽트를 처리하는 커널 함수
