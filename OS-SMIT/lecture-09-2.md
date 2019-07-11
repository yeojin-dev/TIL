# Segmentation

* 논리적 메모리 주소들은 각 세그먼트의 집합이다.
* Segments: subroutine, stack, data, function, code, etc.
* Contiguous Memeory Allocation의 단점을 해결하기 위해 메모리 할당을 프로세스 단위가 아니라 세그먼트 단위로 할당함 - Segmentation
* 이를 위해 세그먼트별로 base 레지스터가 별도로 존재하고 이를 관리하는 segment table 설정해 놓음

# Paging

* Page: Fixed size block
* 메모리 공간은 같은 사이즈의 메모리 공간으로 나눠(이 공간을 프레임이라고 부름) 사용함
* 메모리 주소는 페이지 번호와 오프셋으로 구성됨
* 페이지 번호(논리적)를 프레임 번호(물리적)으로 변환시켜주는 Page Table이 메모리 공간 내에 존재하고 OS가 관리함
* 페이징 기법은 외부 파편화를 막아주지만 내부 파편화는 존재함
* 내부 파편화를 막기 위해서는 페이지의 크기를 줄이면 되지만 그만큼 페이지 테이블이 커지는 단점이 존재

## Paging Process

* 프로세스가 생성되면 OS는 프로세스가 요구하는 페이지의 개수를 파악하고 이를 페이지 테이블을 통해 할당한다.
* 메모리에 접근할 때는 우선적으로 Page Table를 거친다. Page Table에서 프레임 번호를 얻어오고 해당 프레임에서 Offset만큼의 위치에 있는 데이터를 가져온다.


## TLB

* Page Table은 메모리에 있기 때문에 어떤 메모리에 접근하기 위해 Page Table에서 실제 Page까지 메모리에 2번 접근하게 되는데 이는 속도 저하가 생긴다.
* 이를 위해 TLB(Table Lookaside Buffer)가 캐시에 존재함
* TLB hit ratio는 시스템 성능에 아주 큰 영향을 끼친다.

## Protection

* Page Table에 Protection Bit를 설정해 프로세스간 메모리 보호가 가능함
* Valid/Invalid Bit를 통해 해당 프레임이 현재 사용할 수 있는 프레임인지 확인할 수 있음

## Shared Page

* 여러 프로세스가 같은 프레임을 공유함으로써 프로세스간 메모리 공유가 가능해짐

## Hierarchical Page

* 프로세스에 큰 메모리를 할당하기 위해 페이지 테이블을 계층화함
* 페이지 테이블 검색의 속도를 높이기 위해 Hash를 사용하기도 함
