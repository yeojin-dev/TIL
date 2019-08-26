# File Systems

## File and File System

### File

* `A named collection of related information`
* 일반적으로 비휘발성의 보조기억장치에 저장
* 운영체제는 다양한 저장 장치를 파일이라는 동일한 논리적 단위로 볼 수 있게 해 줌
* Operation: create, read, write, reposition(lseek), delete, open(파일의 메타데이터를 메인 메모리 적재), close 등
* 리눅스에서는 I/O 장치도 파일로 관리

### File attribute(혹은 파일의 메타데이터)

* 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
    * 파일 이름
    * 파일 유형
    * 저장된 위치
    * 파일 사이즈
    * 접근 권한(읽기/쓰기/실행)
    * 시간(생성/변경/사용)
    * 소유자
    * 기타

### File System

* 운영체제에서 파일을 관리하는 부분
* 파일 및 파일의 메타데이터, 디렉토리 정보 등을 관리
* 파일의 저장 방법 결정
* 파일 보호 등

## Directory and Logical Disk

### Directory

* 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일
* 그 디렉토리에 속한 파일 이름 및 파일 메타데이터 등
* Operation: search for a file, create a file, delete a file, list a directory, rename a file, traverse the file system

### Partition(Logical Disk)

* 하나의 물리적 디스크 안에 여러 파티션을 두는 것이 일반적
* 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도 함
* 물리 디스크를 파티션으로 구성한 뒤 각각의 파티션에 파일 시스템을 깔거나 스와핑 등 다른 용도로 사용할 수 있음

## open("/a/b/c")

![open()](https://kouzie.github.io/assets/OS/OS_12_1.png)

* 디스크로부터 파일의 메타데이터를 메모리로 가지고 옴
* 이를 위하여 directory path를 탐색
    * 루트 디렉토리를 open하고 그 안에서 파일 a의 위치 획득
    * 파일 a를 open하고 read하여 파일 b의 위치 획득
    * 파일 b를 open하고 read하여 파일 c의 위치 획득
    * 파일 c를 open
* directory path의 탐색에 너무 많은 시간 소요
    * open을 read/write와 별도로 두는 이유임
    * 한 번 open한 파일은 read/write 시 directory search 불필요
* Open file table
    * 현재 open된 파일들의 메타데이터 보관소(in memory)
    * 디스크의 메타데이터보다 몇 가지 정보가 추가
        * open한 프로세스의 수
        * file offset: 파일 어느 위치 접근 중인지 표시(별도 테이블 필요)
* File Descriptor(file handle, file control block)
    * Open file table에 대한 위치 정보(오프셋)
    * 프로세스 별로 PCB의 per-process file descriptor table에 저장

## File Protection

* 각 파일에 대해 누구에게 어떤 유형의 접근(읽기/쓰기/실행)을 허락할 것인가?

### Access Control 방법

1. Access Control Matrix

![ACL](https://www.researchgate.net/profile/Md_Kamrul_Hasan5/publication/50346030/figure/tbl3/AS:667678335320072@1536198328582/Access-control-matrix.png)

* Access Control List(ACL): 파일 별로 누구에게 어떤 접근 권한이 있는지 표시
* Capability: 사용자 별로 자신이 접근 권한을 가진 파일 및 해당 권한 표시
* 오버헤드가 매우 큼

2. Grouping

* 전체 사용자를 owner, group, public의 세 그룹으로 구분
* 각 파일에 대해서 세 그룹의 접근 권한(rwx)를 3비트씩으로 표시
* UNIX: `rwxr--r--`
* 실제로 사용하는 방법

3. Password

* 파일마다 패스워드를 두는 방법(디렉토리 파일에 두는 방법도 가능)
* 모든 접근 권한에 대해 하나의 패스워드(all or nothing)
* 접근 권한 별 패스워드: 암기와 관리 문제

## File System의 Mounting

![Mounting](https://read.seas.harvard.edu/~kohler/class/05f-osp/notes/fig15-04.jpg)

* 하나의 파일시스템에서 다른 파일시스템의 루트 디렉토리 파일을 가져와서 연결하는 것

## Access Methods

### 순차 접근(sequential access)

* 카세트 테이프
* 읽거나 쓰면서 오프셋 증가/감소

### 직접 접근(direct access, random access)

* LP 레코드판
* 파일을 구성하는 레코드를 임의의 순서로 접근할 수 있음
