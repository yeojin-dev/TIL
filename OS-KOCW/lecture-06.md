# Process 3

## Benefits of Threads

### 1. Responsiveness

#### multi-threaded web: if one thread is blocked, another thread continues

### 2. Resource Sharing

#### n threads can share binary code, data and resource of the process

### 3. Economy

#### creating and CPU switching thread(rather then a process)
#### Solaris의 경우 각각 30배, 5배 차이가 남

### 4. Utilization of MP Architectures

#### each thread may be running in parallel on a difference processor

## Implementation of Threads

### some are supported by kernel(kernel thread)

* OS가 멀티스레드라는 것을 알고 있음

### others are supported by library(user thread)

* POSIX P-thread, Mach C-thread
* 프로세스 안에 스레드가 여럿 있다는 것을 OS가 모름
* 프로세스가 직접 스레드를 관리

### some are real-time threads
