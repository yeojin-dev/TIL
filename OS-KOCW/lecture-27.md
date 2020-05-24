# Disk Management and Scheduling 2

## Swap-Space Management

### 디스크를 사용하는 2가지 이유

1. 메모리의 휘발성
2. 프로그램 실행을 위한 메모리 공간 부족

### Swap-Space

* 가상 메모리에서는 디스크를 메모리의 연장으로서 사용
* 파일시스템 내부에 둘 수도 있으나 별도의 파티션으로 사용하는 것이 일반적
    * 공간 효율성보다 속도 효율성이 우선임
    * 일반 파일보다 훨씬 짧은 시간만 존재하고 자주 참조됨
    * 그래서 블록의 크기 및 저장 방식이 일반 파일시스템과 다름
    * 연속 할당 방식이 고려될 수 있음

## RAID

### Redundant Array of Independant Disks

* 여러 개의 디스크를 묶어서 사용

### RAID 사용 목적

* 디스크 처리 속도 향상
    * 여러 디스크에 블록을 분산 저장
    * 병렬적으로 읽어올 수 있음(interleaving, striping)

* 신뢰성 향상
    * 동일 정보를 여러 디스크에 중복 저장
    * 하나의 디스크가 고장났을 경우 다른 디스크에서 읽어옴(mirroring, shadowing)
    * 단순한 중복 저장이 아니라 일부 디스크에 패리티를 적용하여 공간의 효율성을 높임