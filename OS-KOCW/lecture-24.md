# File System Implementation 1

## Contiguous Allocation

* 스토리지에 파일을 연속해서 저장하는 방법

### 단점

* 외부 단편화가 발생
* 파일 grow 어려움
    * 파일 생성 시 얼마나 큰 공간을 할당해야 할지 결정하기 어려움
    * grow 가능 또는 공간 낭비(내부 단편화)의 문제

### 장점

* Fast I/O
    * 1번의 seek/rotation으로 많은 바이트 transfer 가능
    * realtime file 용도로 또는 이미 실행 중인 process의 swapping 용도로 가능
* Direct Access 가능

## Linked Allocation

* 파일의 첫 번재 블록 정보 디렉토리에 저장하고 각 블록마다 다음 블록을 가리키는 정보가 있음
* 마지막 블록까지 도달하면 파일 읽기 끝

### 단점

* 순차 접근만 가능한 설계
* 신뢰성 문제
    * 도중에 섹터(512바이트)가 고장나면 이후 데이터를 읽을 수 없음
    * 포인터를 위한 공간이 블록의 일부가 되어 공간 효율성을 떨어뜨림

### 장점

* 외부 단편화가 없음

### File-Allocation Table(FAT) 파일 시스템

* 포인터를 별도의 위치에 보관하여 신뢰성 문제 해결

## Indexed Allocation

* 디렉토리에 인덱스 블록 정보를 저장하고 인덱스 블록은 정보를 가진 여러 블록들을 가리키고 있음

### 단점

* 작은 파일의 경우 공간을 낭비함
* 큰 파일의 경우 하나의 블록에 인덱스를 모두 저장할 수 없음
    * linked scheme, multi-level index 기법 사용해 해결 가능

### 장점

* 외부 단편화 발생하지 않음
* Direct Access 가능

## 유닉스 파일시스템의 구조

![유닉스 파일시스템](https://kouzie.github.io/assets/OS/OS_12_8.png)

* 유닉스 파일시스템은 기본적으로 Indexed Allocation 방법을 사용하고 있음
    * Direct Index
    * Single Indirect
    * Double Indirect
    * Triple Indirect

### Boot Block

* 부팅에 필요한 정보(bootstrap loader)

### Super Block

* 파일 시스템에 관한 총체적인 정보를 담고 있음

### Inode

* 파일 이름을 제외한 파일의 모든 메타데이터를 저장하고 있음
* 파일 이름은 디렉토리 파일이 가지고 있음

### Data Block

* 파일의 실제 내용을 보관

## FAT File System

* FAT이라고 하는 테이블에 해당 블럭의 다음 블럭이 어떤 블럭인지 정보를 저장하는 구조(Linked Allocation)
* 디렉토리 파일에는 파일 이름과 시작 블럭의 정보가 있음
* FAT은 매우 중요한 정보이기 때문에 백업본을 디스크의 다른 영역에 저장하고 있음

## Free-Space Management

### Bit map or Bit vector

* 각 블럭별로 비트를 할당해 관리(0이면 빈 블럭, 1이면 사용 중)
    * 유닉스 시스템이라면 슈퍼 블록에 관리
* 연속적인 빈 블록을 찾는 데 유리함

### Linked List

* 모든 빈 블록을 링크로 연결함
* 연속적인 빈 블록을 찾는 데 어려움
* 공간의 낭비가 없음(빈 블록에 저장하면 되기 때문임)

### Grouping

* 연결 리스트의 확장
* 빈 블록에 다른 빈 블록의 포인터가 모여 있음(Indexed Allocation와 비슷함)

### Counting

* First Free Block 정보와 Contiguous Free Blocks 정보를 관리
* 프로그램들이 종종 여러 개의 연속적인 블록을 할당하고 반납한다는 점에 착안함

## Directory Implementation

### Linear List

* <파일 이름, 파일 메타데이터>의 리스트
* 구현이 간단함
* 디렉토리 내에 파일이 있는지 찾기 위해서는 linear search 필요함(비효율적)

### Hash Table

* Linear List + Hashing
* 해시 테이블은 파일 이름을 이 파일의 선형 리스트의 위치로 바꿈
* O(1)의 검색 시간 복잡도를 가질 수 있음
* 해시 충돌이 발생할 수 있음

### 파일 메타데이터 보관 위치

* 디렉토리 내에 직접 보관함
* 디렉토리에는 포인터를 두고 다른 곳에 보관
    * Inode, FAT 등

### Long File Name의 지원

* <파일 이름, 파일 메타데이터>의 리스트에서 각 엔트리는 일반적으로 고정 크기임
* 파일 이름이 고정 크기의 엔트리 길이보다 길어지는 경우 엔트리의 마지막 부분에 이름의 뒷부분이 위치한 곳의 포인터를 두는 방법
* 이름의 나머지 부분은 동일한 디렉토리 파일의 일부에 존재함

## VFS and NFS

### Virtual File System

* 서로 다른 다양한 파일 시스템에 대해 동일한 시스템 콜 인터페이스(API)를 통해 접근할 수 있게 해주는 OS layer

### Network File System

* 분산 시스템에서는 네트워클르 통해 파일이 공유될 수 있음
* NFS는 분산 환경에서의 대표적 파일 공유 방법임

## Page Cache and Buffer Cache

### Page Cache

* 가상 메모리의 페이징 시스템에서 사용하는 페이지 프레임을 캐싱 관점에서 설명하는 용어
* Memory-Mapped I/O를 쓰는 경우 파일 I/O에서도 페이지 캐시 사용

### Memory-Mapped I/O

* 파일의 일부를 가상 메모리에 매핑시킴
* 매핑시킨 영역에 대한 메모리 접근 연산은 파일 입출력을 수행하게 함

### Buffer Cache

* 파일시스템을 통한 I/O 연산은 메모리의 특정 영역인 버퍼 캐시 사용
* 파일 사용의 지역성 사용
    * 한 번 읽어온 블록에 대한 후속 요청 시 버퍼 캐시에서 즉시 전달
* 모든 프로세스가 공용으로 사용
* Replacement Algorithm 필요함(LRU, LFU 등)

### Unified Buffer Cache

* 최근 OS에서는 기존의 Buffer Cache가 Page Cache에 통함됨
