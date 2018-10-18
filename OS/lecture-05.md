# 다중 프로세서 스케줄링

## SMP

* 다중 프로세서 스케줄링이 어려운 기본적인 이유는 각 프로세서마다 성능이 다를 수 있다는 점
* asymmetric multiprocessing: 프로세서마다 OS가 따로 있고 그 중 하나의 OS가 master로서 다른 slave들을 관리
* symmetric multiprocessing(SMP): 하나의 OS가 모든 프로세서 관리, 현재는 대부분 SMP 구조
* SMP에서는 하나의 OS가 여러 개의 프로세스 스케줄러, 디스패처를 가지고 있음

## Processor Affinity

* 다중 프로세서라고 해도 프로세스를 매번 다른 프로세서가 처리하면 캐시 유지 문제가 발생함
* 프로세스가 처리하는 프로세서를 유지하는 것을 Processor Affinity라고 부름
* soft affinity와 hard affinity가 존재

```
캐시를 비우는 과정

1. writeback buffer에 기록
2. buffer가 가득 차면 캐시에서 메모리로 데이터 전송(burst)
3. 메모리로 데이터가 이동했기 때문에 해당 캐시 블록은 invalid flag 처리
```

## Load Balancing

* OS에서 어떤 프로세서가 가장 바쁜지 또는 노는지 체크(sleep 시간과 execution 시간 비교)

### Migration

* OS가 보기에 어떤 프로세서의 큐에 프로세스가 너무 많이 쌓였을 경우 다른 프로세서의 큐로 프로세스를 옮김(migration)
* 바쁜 프로세서가 한가한 프로세서로 보내는 것을 push migration, 한가한 프로세서가 바쁜 프로세서로부터 가져오는 것을 pull migration이라고 함
* push migration이 더 안정적(I/O 처리가 더 쉬움)
* 두 종류를 같이 사용할 수 있음
* migration 자체가 오버헤드이기 때문에 프로세서 간 불균형이 클 때에만 사용함

```
어떤 프로세스를 migration할 것인가?

가장 오랫동안 실행하지 않은 프로세스를 보내는 것이 가장 좋다.
```

## Multi-Threading

* 하나의 프로세서에 여러 개의 스레드를 할당할 수 있음
* 하이퍼스레딩(인텔의 브랜드): 프로세서에 복수의 레지스터 세트를 설계해서 어떤 프로세스가 wait 상태가 되면 다른 레지스터 세트로 바로 스위칭해서 연산을 계속하는 기술
* 논리적으로 복수의 프로세서가 동작하는 것과 같게 되고 논리적 프로세서들의 총합은 CPU utilization 100%를 넘지 못 함
* Coarse-grained: 스레드가 블록될 경우에만 레지스터 스위칭
* Fine-grained: 일정한 간격마다 레지스터 스위칭

## Real-Time OS

* 주기적으로 인터럽트, 이벤트가 발생하고 주기마다 그 인터럽트, 이벤트를 처리해주어야 하는 OS
* event-driven OS
* Hard real-time: 반드시 주기 안에 처리해야만 하며 처리하지 못 할 것 같으면 아예 처리하지 않음
* Soft real-time: 정기적으로 가장 시급한 이벤트를 처리하지만 처리하지 못 할 수도 있음
* 각 인터럽트마다 interrupt service routine이 존재하고 이를 위해 context switching 필요함

```
리얼타임 OS에서 중요한 정보

1. period: 이벤트가 발생하는 주기
2. deadline: 이벤트를 처리해야하는 제한시간
3. time: 실제 이벤트를 처리하는데 걸리는 시간(CPU burst)
```

### Rate-Monotonic Scheduling

* preemptive scheduling
* deadline이 작은 이벤트부터 처리함
* CPU 사용량이 일정 이상이 되면 처리 불가능한 상황이 존재하며 프로세서가 N개일 때 한계 CPU 사용률은 아래와 같음

CPU utilization < N * (2<sup>1/N</sup> - 1)

### Earliest-Deadline-First Scheduling

* 남은 deadline이 짧은 이벤트부터 처리
* Rate-Monotonic Scheduling으로 처리할 수 없는 구조도 실행 가능해짐

## 실제 OS 사례

### Linux(Kernel 2.6.23)

* 0~139 단계의 queue가 존재하며 0~99번 queue는 real-time(earliest-deadline-first), 100~139번은 normal task(Completely Fair Scheduler) 처리함
* 높은 우선순위일수록 퀀텀 타임이 길게 설계되어 있음
* Completely Fair Scheduler: 실제 CPU 실행시간을 공평하게 해주는 스케줄러, 실제 실행시간(runtime)을 기록해나가면서 관리함
* 리눅스 시스템의 경우, 복수의 사용자가 동시에 시스템을 사용할 수 있기 때문에 fairness가 중요함
* priority 개념과 fairness 개념을 모두 만족시키기 위해 virtual runtime 도입함(priority에 따라 계산할 실제 runtime을 보정)
