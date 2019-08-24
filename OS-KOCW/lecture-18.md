# Memory Management 2

## Paging

![Paging](https://qph.fs.quoracdn.net/main-qimg-32f8b581fefa46f0a7efb70656e65b03)

* 프로세스의 가상 메모리를 동일한 사이즈의 페이지로 나눔
* 가상 메모리의 내용이 페이지 단위로 non-contiguous 저장
* 일부는 backing storage에, 일부는 physical memory에 저장됨

### Basic Method

* 물리 메모리를 동일한 크기의 프레임으로 나눔
* 논리 메모리를 동일한 크기의 페이지로 나눔(프레임과 페이지는 같은 크기)
* 모든 가용 프레임을 관리
* 페이지 테이블을 이용하여 논리 주소를 물리 주소로 변환
* 외부 단편화 발생하지 않음
* 내부 단편화는 발생할 수 있음

## Implementation of Page Table

* 페이지 테이블은 메인 메모리에 상주
* Page-Table Base Register(PTBR)가 페이지 테이블을 가리킴
* Page-Table Length Register(PTLR)가 테이블 크기를 보관
* 모든 메모리 접근 연산에는 2번의 메모리 접근이 필요함
* Page Table 1번, 실제 데이터 접근 1번
* 속도 향상을 위해 CPU 내 별도의 하드웨어(캐시)를 구성하기도 함(Translation Lookaside Buffer)
    * 페이지 테이블이 index(offset) 개념으로 프레임 번호를 찾는 것에 비해 TLB 실제 페이지 번호와 프레임 번호를 쌍으로 저장하고 있어야 함
    * TLB는 병렬 탐색이 하드웨어적으로 가능하도록 설계하는 것이 일반적임
    * TLB는 context switch 시점에 flush

![TLB](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6e/Translation_Lookaside_Buffer.png/373px-Translation_Lookaside_Buffer.png)

## Effective Access Time

* TLB 접근 시간을 e, 메모리 사이클 시간을 1, TLB hit 비율은 a이라고 가정
* Effective Access Time(EAT): 2 + e - a
* TLB 설계가 페이지 테이블만 있는 설계보다 효과적임

## Two-Level Page Table

![Two-Level Page Table](https://i.ytimg.com/vi/hhKRbc1QAjs/maxresdefault.jpg)

* 현대의 컴퓨터는 address space가 매우 큼
    * 32비트 주소 사용 시 4GB의 공간을 사용함
    * 페이지 크기가 4KB일 경우 1M개의 페이지 테이블 엔트리가 필요함
    * 각 엔트리가 4B 시 프로세스 당 4M의 페이지 테이블 필요
    * 대부분의 프로그램은 4G의 주소 공간 중 지극히 일부분만 사용하므로 페이지 테이블 공간이 낭비됨

* 페이지 테이블 자체를 페이지로 구성함
* 사용되지 않는 주소 공간에 대한 outer page table의 엔트리 값은 null(대응하는 inner table 값이 없음)

## Two-Level Paging Example

* logical address 구성을 기준
    * 20비트의 페이지 번호
    * 12비트의 페이지 오프셋(실제 데이터가 있는 페이지의 오프셋)

* 페이지 테이블 자체가 페이지로 구성되기 때문에 페이지 번호(20비트)는 다음과 같이 나뉨
    * 10비트의 페이지 넘버(outer 페이지 테이블의 번호)
    * 10비트의 페이지 오프셋(outer 페이지 테이블의 오프셋)
    * 위 2개의 값으로 실제 데이터가 있는 페이지 번호를 알 수 있음
