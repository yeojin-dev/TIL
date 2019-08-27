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
