# Disk Management and Scheduling 1

## Disk Structure

### Logical Block

* 디스크의 외부에서 보는 디스크의 단위 정보 저장 공간들
* 주소를 가진 1차원 배열처럼 취급
* 정보를 전달하는 최소 단위

### Sector

* Logical Block이 물리적인 디스크에 매핑된 위치
* Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번째 섹터

## Disk Scheduling

### Access Time의 구성

* Seek Time: 헤드를 해당 실린더로 움직이는 데 걸리는 시간
* Rotational Latency: 헤드가 원하는 섹터에 도달하기까지 걸리는 회전지연시간
* Transfer Time: 실제 데이터의 전송 시간

### Disk Bandwidth

* 단위 시간 당 전송된 바이트의 수

### Disk Scheduling

* Seek Time 최소화가 목표
* Seek Time 값이 Seek Distance를 의미하는 것은 아님

## Disk Management

### physical fotmatting(Low-Level formatting)

* 디스크를 컨트롤러가 읽고 쓸 수 있도록 섹터들로 나누는 과정
* 각 섹터는 헤더 + 실제 데이터 + 트레일러로 구성되어 있음
* 헤더와 트레일러는 섹터 번호, ECC(Error-Correcting Code) 등의 정보가 저장되며 디스크 컨트롤러가 직접 접근, 운영

### partitioning

* 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
* OS는 이것을 독립적인(논리적인) 디스크로 취급함

### logical formatting

* 파일시스템을 만드는 것
* FAT, Inode, 빈 공간 등의 구조 포함

### Booting

* ROM에 있는 small bootstrap loader 실행
* 섹터 0(boot block)을 로드하여 실행
* 섹터 0은 full bootstrap loader program
* OS를 디스크에서 로드하여 실행함

## Disk Scheduling Algorithm

* 해당 알고리즘은 디스크 컨트롤러에 존재함

### FCFS

### SSTF(Shortest Seek Time First)

* 가장 가까운 실린더로 이동함
* 기아 현상 발생할 수 있음

### SCAN

* 디스크 암이 한 쪽 끝에서 다른 쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
* 다른 한 쪽 끝에 도달하면 역방향으로 이동하며 오는 길목에 있는 모든 요청을 처리하고 다시 반대쪽 끝으로 이동함
* 실린더 위치에 따라 대기 시간이 달라짐
    * 실린더 가운데 있는 부분은 상대적으로 대기 시간이 짧은 경향이 있고 실린더 양쪽 끝에는 시간이 길어짐

### C-SCAN

* 디스크 암이 한 쪽 끝에서 반대쪽으로 이동하며 가는 길목에 있는 모든 요청을 처리
* 반대쪽 끝에 도달했으면 되돌아오면서 요청을 처리하지 않고 다시 출발점으로 바로 돌아옴
* SCAN보다 균일한 대기시간을 가질 수 있음

### Ohters

* N-SCAN: 디스크 암이 출발한 이후에 도달한 요청에 한해 되돌아오면서 처리함

* C-LOOK: 디스크 암이 이동하며 처리하다가 이후 섹터에 요청이 없을 경우 끝까지 가지 않고 바로 되돌아감

## Disk-Scheduling Algorithm의 결정

* SCAN, C-SCAN 종류의 알고리즘과 LOOK 종류의 알고리즘은 디스크 I/O가 많은 시스템에서 효율적인 것으로 알려짐
* 파일 할당 방법에 따라 디스크 요청이 영향을 받음
* 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는 것이 바람직함
