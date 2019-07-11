# Memory Management

## Background

* 메모리는 매우 큰 바이트 배열과 그것에 대한 주소로 이루어져 있다.
* 메모리는 용량, 속도, 가격에 따라 계층 구조를 가지고 있다.
1. L1 Cache: CPU 2~4 사이클에 접근 가능
2. L2 Cache: 10~20 사이클
3. Main Memory: 100~200 사이클

### Memory Protection

* 사용자 프로세스와 커널 프로세스, 사용자간 프로세스의 메모리 프로텍션을 위해 base, limit 레지스터라는 특수한 메모리가 필요하다.
* 프로세스가 요구하는 메모리의 주소가 base, limit 레지스터의 메모리값 밖이면 메모리 접근을 할 수 없다.

### Address Binding

* 프로그램의 소스에서 지정하는 메모리 주소는 상징적인(symbolic) 주소
* 컴파일러가 소스를 컴파일하면서 상징적인 주소를 논리적 주소(시작 주소와 오프셋)로 바꾼다.
* OS의 Loader가 프로그렘을 프로세스로 만들 때 논리적 주소를 실제 메모리 주소로 바꾼다.

### Memory Management Unit(MMU)

* 프로그램 소스를 컴파일하면 그 소스 내의 메모리 주소는 논리적 주소
* 그렇기 때문에 CPU의 주소 관련 연산은 논리적 주소를 연산하는 것
* CPU의 메모리 관련 연산의 결과(논리적 주소)를 실제 주소로 바꿔주는 장치가 MMU

### Dynamic Loading and Dynamic Linking and Shared Libraries(DLL)

* 프로그램이 처음 로드될 때 모든 라이브러리를 로드하지 않고 동적으로 로딩
* 불필요한 메모리 사용을 줄일 수 있다.

## Swapping

* 메모리 공간의 물리적인 한계를 극복하기 위해 디스크 공간을 스왑 메모리로 설정해 놓음
* 스왑 메모리는 디스크에 존재하지만 파일 시스템으로 관리되지 않음
* 메모리 공간이 모자랄 경우, 메모리 내의 어떤 프로세를 스왑 메모리로 이동시켜놓고 남은 메모리 공간을 사용
* 메모리 스왑 시간은 메모리 접근에 비해 시간이 매우 많이 걸림

## Contiguous Memory Allocation

* Multiple Partition Method: 고전적인 메모리 할당 방식으로 통째로 메모리에 올린다.
* Variable Partition Method: 메모리를 몇몇 부분으로 나누어 할당한다.

### Memory Fragmentation

* External: 프로세스 할당을 위한 메모리 공간이 있지만 공간이 파편화되어 있다.
* Internal: 프로세스에 할당된 메모리 공간이 실제 프로세스가 사용하는 메모리 공간보다 크다.

### Compaction

* 메모리 파편화를 막기 위해서 동적으로 메모리를 이동시키는 방법이 있지만 오버헤드가 매우 크다.
