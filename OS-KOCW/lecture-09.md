# CPU Scheduling 1

## Scheduling Criteria

1. CPU utilization: keep the CPU as busy as possible

2. Throughput: number of processes that complete their execution per time out

3. Turnaround time: amount of time to execute a particular process

4. Waiting time: amount of time a process has been waiting in the ready quere

5. Response time: amount of time it takes from when a request was submitted until the first response is produced, not output

## FCFS

![FCFS](https://www.studytonight.com/operating-system/images/fcfs.png)

* Convoy effect: 짧은 프로세스가 큰 프로세스 뒤에서 오랜 시간동안 기다리는 현상

## SJF

* 각 프로세스의 다음 CPU burst time 기준으로 스케줄링
* CPU burst time 값이 가장 짧은 프로세스부터 제일 먼저 스케줄링
* 주어진 프로세스들에 대해 가장 짧은 평균 대기시간을 가짐

![SJF](https://www.studytonight.com/operating-system/images/sjf.png)

### SRTF(Shortest Remaining Time First)

* 선점 환경에서 CPU 사용 중 현재 프로세스의 남은 시간보다 새로 들어온 프로세스의 시간이 더 짧으면 현재 프로세스는 CPU 자원을 빼앗기고 더 짧은 프로세스에게 CPU 할당

### starvation

* 큰 프로세스가 아주 오랜 시간 자원을 받지 못 할 수 있음

### 다음 CPU burst time 예측

* 프로세스가 미래에 얼마나 CPU 자원을 쓰는지 예측

![Next CPU burst](https://slideplayer.com/slide/4429543/14/images/18/Determining+Length+of+Next+CPU+Burst.jpg)

* 위 식을 정리하면 t<sub>n+1</sub> = (1 - a)<sup>n+1</sup> * t<sub>0</sub>

## Priority Scheduling

* 우선순위가 높은 프로세스부터 수행
* SJF도 일종의 우선순위 스케줄링
* starvation 현상이 존재하기 때문에 aging(as time progresses increase the priority of the process) 기법이 필요

## Round Robin

* 각 프로세스는 동일한 크기의 할당 시간(10~100ms)을 가지고 있음
* 시간이 지나면 ready queue로 이동
* n개의 프로세스가 있고 할당 시간이 q라면, 어떤 프로세스도 (n-1)*q 이상 기다리지 않음
* q의 값이 커지면 FCFS와 같아지고, 너무 작으면 context switching 비용이 지나치게 커짐
* SJF보다 평균 대기시간은 길지만 response time은 더 짧음
