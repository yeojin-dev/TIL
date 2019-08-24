# Virtual Memory 1

## Demand Paging

* 실제로 필요할 때 page를 메모리에 올리는 것
    1. I/O 요청의 감소
    2. 메모리 사용량 감소
    3. 빠른 응답 시간
    4. 더 많은 사용자 수용

* Valid/Invalid bit 사용
    * Invalid 의미
        * 사용되지 않는 주소 영역인 경우
        * 페이지가 물리적 메모리에 없는 경우
    * 처음에는 모든 페이지 엔트리가 invalid 초기화
    * address translation 시점에 invalid bit가 세팅되어 있다면 page fault

![Page Table](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSfEpXD7vtLSKyjNnidezctFGc0hEdkTP9TSd_58_BC25OER5KG)

## Page Fault

* Invalid Page 접근 시 MMU가 trap 발생(page fault trap)
* 커널 모드로 들어가서 page fault handler가 invoke됨

### Page Fault 처리 순서

1. Invalid Reference(e.g. bad address, protection violation) -> abort process
2. Get an empty page frame(없으면 replace)
3. 해당 페이지를 disk에서 memory로 읽어옴
    1. 디스크 I/O 끝날 떄까지 이 프로세스는 CPU를 preempt(block)
    2. 디스크 읽기가 끝나면 page table entry 기록 후 valid bit 켬
    3. ready queue에 있는 프로세스를 insert -> dispatch later
4. 이 프로세스가 CPU를 잡고 다시 run
5. 중단되었던 인스트럭션 재개

## Performance of Demand Paging

* 페이지 폴트 비율 p
* EAT = (1-p) * 메모리 접근 + p * 오버헤드

### 오버헤드

1. OS, HW 오버헤드
2. 스왑 페이지 아웃
3. 스왑 페이지 인
4. OS, HW 재시작 오버헤드

## Free Frame이 없는 경우

### Page Replacement

* 어떤 프레임을 빼앗아올지 결정해야만 함
* 곧바로 사용되지 않을 page를 쫓아내는 것이 좋음
* 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있음

### Replacement Algorithm

* 페이지 폴트 비율을 최소화하는 것이 목표
* 알고리즘의 평가
    * 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사
* reference string의 예: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

![Page Replacement](https://basicittopic.com/wp-content/uploads/2018/11/page-replacement-390x205.png)

## Optimal Algorithm(Belady's Algorithm)

![OPT](https://media.geeksforgeeks.org/wp-content/uploads/optimal_page.png)

* 가장 먼 미래에 참조되는 page를 replacement
* 미래의 참조를 알 수 없기 때문에 구현은 불가능하지만 다른 알고리즘의 성능에 대한 upper bound 제공

## FIFO Algorithm

![FIFO](https://www.researchgate.net/profile/Gyanendra_Kumar12/publication/319467661/figure/fig1/AS:536152845152258@1504840207987/FIFO-page-replacement-algorithm-with-3-memory-frames.png)

* 가장 먼저 들어온 페이지를 replace

### FIFO Anomaly

* 많은 프레임이 있다고 해서 페이지 폴트가 적은 것이 아님

## LRU Algorithm

![LRU](https://www.researchgate.net/profile/Gyanendra_Kumar12/publication/319467661/figure/fig2/AS:536152847417344@1504840208056/LRU-page-replacement-algorithm-with-3-memory-frames.png)

* 가장 오래 전에 참조된 페이지를 replace

## LFU(Least Frequently Used) Algorithm

* 참조 횟수가 가장 적은 페이지를 지움
    * 최저 참조 횟수인 페이지가 여럿인 경우에는 LRU 적용 가능

* 특징
    * LRU보다 장기적인 시간 규모를 보기 때문에 더 OPT에 가까워질 수 있음
    * 참조 시점의 최근성을 반영하지 못 함
    * LRU보다 구현이 복잡함

## LRU, LFU 알고리즘의 구현

### LRU

* 참조 시간을 기준으로 Double Linked List 구현
* 새로운 참조가 발생하면 가장 앞의 element로 이동
    * 참조 발생한 페이지의 앞뒤의 포인터 연결
    * 가장 앞 포인터를 자신에게 붙이고 자신의 뒷 포인터를 기존 1번째와 연결
* O(1) 시간복잡도

### LFU

* 참조가 발생해도 다른 페이지의 참조 횟수를 조회해야만 정렬 가능
* heap 이용해서 O(logn) 시간복잡도

## 다양한 캐시 환경

### 캐시

* 한정된 빠른 공간에 요청된 데이터를 저장해 두었다가 후속 요청 시 캐시로부터 직접 서비스하는 방식
* 페이징 시스템 외에도 캐시 메모리, 버퍼 스트링, 웹 캐싱 등 다양한 분야에서 사용

### 캐시 운영 시간의 제약

* 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
* 버퍼 캐싱, 웹 캐싱의 경우 O(logn) 복잡도까지 허용
* 페이징 시스템의 경우
    * 페이지 폴트의 경우에만 OS가 관여함
    * 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알 수 없음
    * O(1)인 LRU조차 적용이 어려움
