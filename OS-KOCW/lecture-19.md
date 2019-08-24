# Memory Management 3

## Multilevel Paging and Performance

* 주소 공간이 더 커지면 다단계 페이지 테이블이 필요
* 각 단계의 페이지 테이블이 메모리에 존재하므로 논리 주소의 물리 주소 변환에 더 많은 메모리 접근이 필요해짐
* TLB를 통해 메모리 접근 시간을 줄일 수 있음
* 4단계 페이지 테이블을 사용하는 경우
    * 메모리 접근 시간이 100ns, TLB 접근 시간이 20ns이고 TLB 히트 비율이 98%일 경우를 가정함
    * EAT = 0.98 * 120 + 0.02 * 520 = 128ns
    * 결과적으로 주소 변환을 위해 28ns만 소요함

## Memory Protection

### Protection bit

* 페이지에 대한 접근 권한(read/write)

## Valid/Invalid bit

* 해당 주소의 프레임에 그 프로세스를 구성하는 유효한 내용이 있다/없다는 뜻임

* 유효한 내용이 없다는 뜻은 해당 페이지가 메모리에 있지 않고 swap area에 있다는 의미임

## Inverted Page Table

* 페이지 테이블이 매우 큰 이유
    * 모든 프로세스마다 그 논리 주소에 대응하는 모든 페이지에 대해 페이지 테이블 엔트리가 존재함
    * 대응하는 페이지가 메모리에 있든 없든 간에 페이지 테이블에는 엔트리로 존재함

* Inverted Page Table
    * 기존 페이지 테이블은 프로세스마다 하나씩 존재했지만 Page Frame 하나 당 페이지 테이블에 하나의 엔트리를 둔 것(system-wide)
    * 각 페이지 테이블 엔트리는 각각의 물리적 메모리의 페이지 프레임이 담고 있는 내용 표시(PID, 프로세스의 논리 주소)
    * 테이블 전체를 탐색해야 함
    * 위에 대한 대응으로 associative register(TLB) 사용

![Inverted Page Table](https://media.geeksforgeeks.org/wp-content/uploads/33-6.png)

## Shared Page

* Shared Code
    * Re-entrant Code(=Pure Code)
    * read-only를 조건으로 프로세스 간에 하나의 code만 메모리에 올림
    * Shared Code는 모든 프로세스의 논리 주소 공간에서 동일한 위치에 있어야 함

* Private Code and Data
    * 각 프로세스들은 독자적으로 메모리에 올림
    * Private Data는 논리 주소 공간의 어떤 곳에 와도 상관이 없음

## Segmentation

* 프로그램을 의미 단위인 여러 개의 세그먼트로 구성
    * 작게는 프로그램을 구성하는 함수 하나하나를 세그먼트로 정의
    * 크게는 프로그램 전체를 하나의 세그먼트로 정의 가능
    * 일반적으로는 code, data, stack 부분이 하나씩 세그먼트로 정의됨

* 세그먼트 종류
    1. main()
    2. function
    3. global variables
    4. stack
    5. symbol table, arrays

## Segmentation Architecture

* 논리 주소는 세그먼트 번호, 오프셋 2가지로 구성

* Segment Table
    * base
    * limit

* Segment-Table Base Register(STBR)

* Segment-Table Length Register(STLR)

![Segment](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2016/02/Translation.png)

### Protection

* 각 세그먼트 별로 protection bit 존재함

### Sharing

* 세그먼트는 의미 단위이기 때문에 공유, 보안에서 페이징보다 효과적임

### Allocation

* 외부 단편화가 발생할 수 있음
* 가변 분할 방식과 동일한 문제점 발생함
