# System Structure & Program Execution 1

![컴퓨터 시스템 구조](https://bournetocode.com/projects/AQA_A_Theory/pages/img/Computer-Components.png)

* 다이어그램에 없는 구성 요소들
    * mode bit: OS 프로세스인지 아닌지 여부를 알려주는 비트
    * interrupt line: CPU에 인터럽트가 발생했는지 알려주는 데이터 패스
    * DMA controller: I/O의 버퍼 데이터를 메모리에 적재해주는 컨트롤러로 작업 완료 시 CPU에 인터럽트 처리를 요청
    * timer: 특정 프로세스가 CPU를 독점하는 것을 막기 위해 CPU 할당 시간(보통 수십 ms)을 지정하고 interrupt line을 통해 CPU에게 인터럽트 처리를 요청
    * I/O controller: I/O에 속해 있는 컨트롤러
    * I/O buffer: I/O에 속해 있는 전용 메모리
