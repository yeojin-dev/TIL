## Page Peplacement Algorithm

### LRU-Approximation

1. Second-Chance Algorithm
* 하드웨어의 지원(TLB)으로 LRU를 근사
* 페이지 테이블에 대해 하드웨어에서 reference bit를 지원해줌
* reference bit: 프레임을 한번 읽으면 켜지고, FIFO로 자기 차례가 오면 꺼짐
* FIFO 알고리즘을 적용하되 reference bit가 꺼져 있으면 빼고, 켜져 있으면 끄고 다음 차례로 넘어감

2. Enhanced Second-Chance Algorithm
* Second-Chance Algorithm에 dirty bit 고려한 버전

class|reference bit|dirty bit|설명
-----|-------------|---------|---
1|0|0|최근에 사용하지도, 데이터가 바뀌지도 않음
2|0|1|최근에 사용하지는 않았지만 데이터가 바뀜
3|1|0|데이터가 바뀌지 않았지만 최근에 사용됨
4|1|1|최근에 사용되었고, 데이터도 바뀜

* FIFO 알고리즘을 적용하되, class 1을 먼저 찾고 이어서 4까지 찾아나가는 형식

## Page Buffering

* dirty bit가 켜져 있는 프레임을 교체해야 할 때 바로 디스크에 접근하는 것이 아니라 OS가 할당한 버퍼 메모리에 임시로 저장하고 I/O 등 남은 시간에 버퍼의 프레임을 스왑시키는 방법
* 페이지 교체를 했지만 다시 해당 프레임을 요청할 수도 있기 때문에 임시 버퍼를 사용함
* 버퍼 설정을 위한 메모리 오버헤드는 존재함

## Allocation of Frames

* 처음 프로세스가 생성될 때 얼머나 프레임을 할당할 것인가?

### Global Allocation

* 전체 할당된 상황을 보고 처음 줄 프레임 수를 결정
* 처음에는 많은 수의 프레임을 주다가 Page Fault가 발생하면 숫자를 줄이고 프레임을 회수함

### Local Allocation

* 처음에 할당받은 프레임 수 내에서 교체해가며 프로세스 진행

## Trashing

* 프로세스가 너무 많아지면 OS가 프레임 교체만 하고 실제 프로세스 수행은 하지 않게 됨

## Working set

* Working set은 어떤 시간동안 사용된 프레임들
* 시간과 Page Fault의 그래프를 보면 어떤 시간동안 Page Fault가 급증하다가 어느 정도의 시간동안 잠잠함
* 이 간격 동안의 프레임의 개수가 Working set
* 프로세스가 시작할 때 Working set만큼의 페이지를 할당하는 것이 이상적

### Windows

* 50~345개의 페이지를 프로세스에 할당
* 처음에는 345개를 가지고 시작하지만 프로세스가 많아지만 숫자를 줄여나감
* 처음 많이 받은 프로세스의 페이지를 빼앗아 다른 프로세스에 주는 형식
* 만약 메모리를 많이 사용하는 프로세스라면 big size page(4mb)를 할당해줌
