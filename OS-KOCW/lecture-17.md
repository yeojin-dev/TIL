# Memory Management 1

## Logical vs. Physical Address

### Logical

* 프로세스마다 독립적으로 가지는 주소 공간
* 각 프로세스마다 0번지부터 시작
* CPU가 보는 주소는 logical address

### Physical

* 메모리에 실제 올라가는 위치

### 주소 바인딩

* symbolic address -> logical address -> physical address

## Address Binding

### Compile Time Binding

* 물리적 메모리 주소가 컴파일 시 결정됨
* 시작 위치 변경 시 재컴파일
* 컴파일러는 절대 코드를 생성함

### Load Time Binding

* Loader의 책임 하에 물리적 메모리 주소 부여
* 컴파일러가 재배치 가능 코드를 생성함

### Run Time Binding

* 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
* CPU가 주소를 참조할 때마다 binding을 점검(Address Mapping Table)
* 하드웨어적인 지원이 필요함(e.g. base and limit registers, MMU)

## Memory Management Unit(MMU)

* MMU: 논리적 메모리 주소를 물리적 메모리 주소로 변환시켜주는 하드웨어 장치

* MMU Scheme: 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 base register(=relocation register)의 값을 더함

* user program: logical address만 다루며 physical address 값은 볼 수 없고 알 필요도 없음

## Dynamic Relocation

![Dynamic Relocation](https://worldfullofquestions.files.wordpress.com/2014/07/memory-management-unitmmu.jpg)

## Hardware Support for Address Translation

![Hardware Support for Address Translation](https://loonytek.files.wordpress.com/2015/11/add.png)

* relocation register: 접근할 수 있는 물리적 주소의 최솟값

* limit register: 논리적 주소의 범위

## Dynamic Loading

* 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 메모리에 로드하는 것
* memory utilization 향상
* 가끔씩 사용되는 많은 양의 코드의 경우 유용함(e.g. 오류 처리 루틴)
* 운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능함(OS는 라이브러리를 통해 지원)

## Overlays

* 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림
* 프로세스의 크기가 메모리보다 클 때 유용
* 운영체제의 지원 없이 사용자에 의해 구현
* 작은 공간 메모리를 사용하던 초창기 시스템에서 수작업으로 개발자가 구현

## Swapping

### Swapping

* 프로세스를 일시적으로 메모리에서 backing store로 보내는 것

### Backing Store

* 디스크의 일부 공간으로 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간

### Swap in/Swap out

* 일반적으로 중기 스케줄러(swapper)에 의해 swap out시킬 프로세스 선정
* prioriry-based CPU scheduling algorithm에서는 우선순위 기준으로 swap out
* Compile Time, Load Time Binding에서는 원래 메모리 위치로 Swap in해야 함
* Execution Time Binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
* swap time은 대부분 transfer time(swap 양에 비례하는 시간)임

### Schematic View of Swapping

![Schematic View of Swapping](https://image.slidesharecdn.com/osppt4unit-120825101537-phpapp02/95/os-swapping-paging-segmentation-and-virtual-memory-3-728.jpg?cb=1345890022)

## Dynamic Linking

* Linking을 실행 시간까지 미루는 기법

### Static Linking

* 라이브러리가 프로그램의 실행 파일 코드에 포함됨
* 실행 파일의 크기가 커짐
* 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비

### Dynamic Linking

* 라이브러리가 실행 시 연결
* 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
* 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
* 운영체제의 도움이 필요

## Allocation of Physical Memory

* 메모리는 일반적으로 두 영역으로 나뉘어 사용
    1. OS 영역: interrupt vector와 함께 낮은 주소 영역 사용
    2. 사용자 프로세스 영역: 높은 주소 영역 사용

* 사용자 프로세스 영역의 할당 방법
    1. Contiguous Allocation: 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
    2. Non-Contiguous Allocation: 하나의 프로세스가 여러 영역에 분산되어 올라갈 수 있음

## Contiguous Allocation

### 고정 분할 방식

* 물리적 메모리를 몇 개의 영구적 분할(파티션)으로 나눔
* 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재
* 분할 당 하나의 프로그램 적재
* 융통성이 없음
    * 통시에 메모리에 로드되는 프로그램의 수가 고정됨
    * 최대 수행 가능 프로그램 크기 제한
* 외부, 내부 단편화 발생

### 가변 분할 방식

* 프로그램의 크기를 고려해서 할당
* 분할의 크기, 개수가 동적으로 변형
* 기술적 관리 기법 필요
* 외부 단편화 발생

### Hole

* 가용 메모리 공간
* 다양한 크기의 Hole들이 메모리 여러 곳에 흩어져 있음
* 프로세스가 도착하면 수용 가능한 Hole 할당
* 운영체제는 할당 공간과 가용 공간 정보를 저장

### Dynamic Storage-Allocation Algorithm

1. First-Fit
    * 최초의 Hole에 할당
2. Best-Fit
    * 가능한 Hole 중 가장 작은 Hole에 할당
    * 모든 Hole 정보를 탐색해야만 함
    * 많은 수의 아주 작은 Hole 발생
3. Worst-Fit
    * 가장 큰 Hole 할당
    * 모든 Hole 탐색
    * 상대적으로 아주 큰 Hole들이 생성

* 실험적으로 First-Fit, Best-Fit이 효과적임

### Compaction

* 외부 단편화 문제를 해결하는 방법
* 사용 중인 메모리 영역을 한 곳에 몰고 Hole들을 한 곳으로 몰아넣는 방법
* 매우 비용이 큰 작업
* 동적으로 메모리 재배치가 가능할 경우에만 사용할 수 있음
