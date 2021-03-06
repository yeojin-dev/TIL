# Introduction to Operating Systems

## 운영체제란 무엇인가?

### 운영체제(Operating System, OS)란?

* 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층

* 협의의 운영체제: 운영체제의 핵심 부분으로 메모리에 상주하는 부분

* 광의의 운영체제: 커널 뿐 아니라 각종 주변 시스템 유틸리티를 포함하는 개념

### 운영체제의 목적

1. 컴퓨터 시스템을 **편리하게 사용할 수 있는 환경을 제공**
    * 운영체제는 동시 사용자/프로그램들이 각각 독자적 컴퓨터에서 수행되는 것 같은 환상을 제공
    * 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행

2. 컴퓨터 시스템의 **자원을 효율적으로 관리**
    * 프로세서, 기억장치, 입출력장치 등의 효율적 관리
        * 사용자간의 형평성 있는 자원 분배
        * 주어진 자원으로 최대한의 성능을 내도록
    * 사용자 및 운영체제 자신의 보호
    * 프로세스, 파일, 메시지 등을 관리

### 운영체제의 분류

1. 동시 작업 가능 여부
    * 단일 작업(single tasking): 한 번에 하나의 작업만 처리
    * 다중 작업(multi tasking): 동시에 두 개 이상의 작업 처리

2. 사용자의 수
    * 단일 사용자(single user): Windows
    * 다중 사용자(multi user): UNIX server

3. 처리 방식
    * 일괄 처리(batch processing)
        * 작업 요청의 일정량 모아서 한꺼번에 처리
        * 작업이 완전 종료될 때까지 기다려야 함
    * 시분할 방식(time sharing)
        * 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용
        * 일괄 처리 시스템에 비해 짧은 응답 시간을 가짐
        * interactive 방식
    * 실시간(realtime) OS
        * 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 실시간 시스템을 위한 OS
        * hard realtime(강한 제약): 원자로, 공장, 미사일, 반도체, 로봇 제어
        * soft realtime(약한 제약): 스트리밍

### 몇 가지 용어

* 아래의 4개의 용어는 중요시하는 부분은 다를 수 있지만 기본적으로 같은 용어임
    1. Multitasking
    2. Multiprogramming
    3. Time sharing
    4. Multiprocess

* Multiprocessor: 하나의 시스템에 CPU가 여럿 있음을 의미

### 운영체제의 예

1. Unix
    * 대부분 C로 작성
    * 높은 이식성
    * 최소한의 커널 구조
    * 복잡한 시스템에 맞게 확장 용이
    * 소스 코드 공개
    * 프로그램 개발에 용이
    * 다양한 버전 존재: Solaris, Linux, FreeBSD

2. Windows
    * 다중 작업용 GUI 기반 운영체제
    * plug and play, 네트워크 환경 강화
    * DOS용 응용 프로그램과 호환성 제공
    * 불안정성이 큼
    * 풍부한 지원 소프트웨어

3. Handheld device를 위한 OS
    * PalmOS, TinyOS 등


### 운영체제의 구조

1. CPU
    * **누구에게 CPU를 줄까?**
    * CPU 스케줄링

2. 메모리
    * **한정된 메모리를 어떻게 쪼개어 쓰지?**
    * 메모리 관리

3. 디스크
    * **디스크에 파일을 어떻게 보관하지?**
    * 파일 관리

4. I/O device
    * **각기 다른 입출력장치와 컴퓨터 간에 어떻게 정보를 주고 받게 하지?**
    * 입출력 관리
