# Data Structure and Application Design

## Data Access Mechanism

* 프로그램 개발 시에 메모리 사용에 대한 설계가 매우 중요함.

### TLB

* 메모리에 있는 페이지 테이블을 CPU 내 캐시로 옮긴 것.
* TLB에 접근하려는 페이지 정보가 없다면 프로그램이 매우 느려짐.

### 절댓값 구하기

1. Logical

```python

def abs(n, m):
    k = n - m
    return k if k > 0 else -k
```

2. Data-Centric

```python
numbers = [n for n in range(100, 0, -1)]
ref = numbers + [0]
numbers.reverse()
ref += numbers

def abs(n):
    k = n - m
    return ref[100 + k]
```

* 실험을 해보면 2번이 더 빠르다. 파이프라인을 유지할 수 있기 때문임.
