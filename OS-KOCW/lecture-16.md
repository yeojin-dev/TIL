# Deadlock 2

## Deadlock Detection and Recovery

* Resource-Allocation Graph를 축약시킨 Wait-for Graph에서 cycle이 존재하는지 확인
* cycle을 찾는 오버헤드는 O(n^2)

### Recovery

1. process termination

2. Resource Preemption
    * 비용을 최소화할 victim 선정 필요
    * safe state로 롤백하여 프로세스를 재시작
    * starvation 문제 존재함

## Deadlock Ignorance

* 데드락이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음
    * 데드락은 매우 드물게 발생하므로 데드락에 대한 조치 자체가 더 큰 오버헤드일 수 있음
    * 만약 시스템에 데드락이 발생할 경우 비정상적으로 동작하는 것을 사람이 느낀 후 직접 프로세스를 종료시키는 방법 등으로 대처
    * 대부분의 범용 OS가 채택
