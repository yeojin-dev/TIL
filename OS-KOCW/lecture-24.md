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
