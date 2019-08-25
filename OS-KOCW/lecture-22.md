# Virtual Memory 2

## 페이징 시스템에서 LRU, LFU 가능한가?

* 하드웨어의 지원 없이는 불가능
    * 하드웨어에서 페이지 참조 횟수, 시간을 기록해야만 하는데 구현이 어려움
    * 만약 구현한다면 해당 페이지 테이블 크기가 너무 커짐
    * 멀티코어 환경의 경우 코어에 따라 횟수, 시간을 기록해야 함

## Clock Algorithm

![Clock Algorithm](https://s3.amazonaws.com/classconnection/671/flashcards/7166671/png/2nd_chance_page_replace_algorithm-14C34195AD47E1B40E0-thumb400.png)

* LRU의 근사 알고리즘
* NRU(Not Recently Used) 알고리즘이라고도 불림
* Reference Bit 이용해서 교체 대상 페이지 선정
* Reference Bit 0인 페이지를 찾을 때까지 포인터를 앞으로 이동
* 포인터를 이동하는 중에 Reference Bit 1인 값들은 모두 0으로 바꿈(하드웨어 지원)
* Reference Bit 0인 페이지 찾으면 그 페이지를 교체
* 한 바퀴 되돌아와서도(second chance) 0이면 그때는 교체당함
* 자주 사용되는 페이지라면 second chance가 올 때 다시 1로 바뀌어 있을 것임
* Cicular Linked List 이용해 구현

### 개선

* Modified Bit(Dirty Bit) 사용
* Dirty Bit 1이면 최근에 변경된 페이지(페이지 교체 시 I/O 동반한다는 의미)

## Page Frame의 Allocation

### Allocation Problem

* 각 프로세스에 얼마만큼의 페이지 프레임을 할당할 것인가?

### Allocation 필요성

* 메모리 참조 명령어 수행 시 명령어, 데이터 등 여러 페이지 동시 참조하기 때문에 명령어 수행을 위해 최소 할당해야만 하는 프레임의 수가 있음
* loop 구성 페이지들은 한꺼번에 allocate되는 것이 유리함(만약 그렇지 않을 경우 매 loop마다 페이지 폴트)

### Allocation Scheme

* Equal Allocation: 모든 프로세스에 똑같은 개수 할당
* Propotional Allocation: 프로세스 크기에 비례하여 할당
* Priority Allocation: 프로세스의 priority에 따라 다르게 할당

## Global vs. Local Replacement

### Global Replacement

* 교체 시 다른 프로세스에 할당된 프레임을 빼앗음
* 프로세스 별로 할당량을 조절하는 효과
* FIFO, LRU, LFU 등의 알고리즘을 global replacement로 사용 시에 해당
* Working set, PFF 알고리즘 사용

### Local Replacement

* 자신에게 할당한 프레임 내에서만 교체
* FIFO, LRU, LFU 등의 알고리즘을 프로세스 별로 운영

## Thrashing

![Thrashing](https://www.studytonight.com/operating-system/images/thrashing.png)

* 프로세스의 원활한 수행에 필요한 최소한의 페이지 프레임 수를 할당받지 못 한 경우
* 페이지 폴트 비율이 매우 높아짐
* CPU utilization 낮아짐
* OS는 MPD를 높여야 한다고 판단
* 또다른 프로세스가 시스템에 추가됨
* 프로세스 당 할당된 프레임 수가 더욱 감수
* 프로세는 page의 swap in/out으로 매우 바쁨
* 대부분의 시간에 CPU는 놀고 있음
* low throughput

## Working set model

### Locality of reference

* 프로세스는 특정 시간 동안 일정 장소만을 집중적으로 참조함
* 집중적으로 참조되는 해당 페이지들의 집합을 locality set이라고 부름

### Working set model

* Locality에 기반하여 프로세스가 일정 시간 동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 페이지들의 집합을 워킹 셋이라고 정의함
* 워킹 셋 모델에서는 프로세스의 워킹 셋 전체가 메모리에 올라와 있어야 수행되고 그렇지 않을 경우 모든 프레임들을 반납한 후 swap out(suspened)
* Thrashing 방지함
* Multiprogramming Degree 결정함

## Working set Algorithm

![Working set](https://2.bp.blogspot.com/-OGiwfO6wNDc/WycoX23lOsI/AAAAAAAAA00/vJ6YXiBtHvolssuhNxM92yFZYifLRhIQQCLcBGAs/s1600/workingsetmodel.png)

### 워킹 셋의 결정

* Working set window를 통해 알아냄
* window size가 d인 경우(d는 시간)
    * 시각 t에서의 워킹 셋 WS(t)
        * d만큼의 시간 간격 사이에 참조된 서로 다른 페이지들의 집합
        * WS(t)에 속한 페이지는 유지, 속하지 않는 것은 버림(즉, 참조된 후 d시간 동안 해당 페이지를 메모리에 유지한 후 버림)
        * 만약 메모리 상태가 WS(t)를 올릴 수 없는 상태라면 모든 페이지를 내리고 프로세스는 suspended

### 워킹 셋 알고리즘

* 프로세스들의 워킹 셋 사이즈 합이 페이지 프레임의 수보다 큰 경우
    * 일부 프로세스들을 swap out시켜 남은 프로세스의 워킹 셋을 우선적으로 충족(MPD 줄임)
* 작은 경우
    * swap out되었던 프로세스에게 워킹 셋을 할당(MPD 증가)

### window size

* 워킹 셋을 제대로 탐지하기 위해서는 window size를 잘 결정해야만 함
* 너무 작으면 locality set을 모두 수용할 수 없음
* 값이 크면 여러 규모의 locality set 수용
* 값이 무한대이면 전체 프로그램 구성 페이지를 워킹 셋으로 간주함

## PFF(Page-Fault Frequency) Scheme

![PFF](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter9/9_21_PageFaultFrequency.jpg)

* 페이지 폴트 비율의 상한값과 하한값을 설정
    * 상한을 넘으면 프레임 더 할당
    * 하한보다 밑이면 프레임 수를 줄임

* 빈 프레임이 없으면 swap out

## Page 사이즈의 결정

* 페이지를 감소시키면
    * 페이지 수 증가
    * 페이지 테이블 크기 증가
    * 내부 단편화 감소
    * 디스크 활용(disk transfer) 효율성 감소
    * 필요한 정보만 메모리에 올라와 메모리 이용이 효율적(locality의 활용 측면에서는 좋지 않음)

* 트렌드는 페이지 사이즈를 키우고 있음
