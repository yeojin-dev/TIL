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
