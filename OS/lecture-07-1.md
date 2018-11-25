# Python Multiprocessing Package(1)

파이썬에서 멀티프로세싱을 위한 패키지

1. Process
2. Queue
3. Pipe: 프로세스간 통신

## Process

```python
from multiprocessing import Process

def f(name):
    print('hello ' + name)

if __name__ == '__main__':
    p = Process(target=f, args=('bob',))    # 프로세스 생성
    print(p.authkey)    # 프로세스 인증키
    p.daemon = True     # 프로세스 완료 후 자동으로 termination
    p.start()           # 프로세스 시작
    p.join()            # 프로세스가 완료될 때까지 대기
```

## Queue

```python
from multiprocessing import Process, Queue

def q(name):
    q.clear()
    q.put([42, None, 'hello'])

if __name__ == '__main__':
    q = Queue()
    p = Process(target=f, args=(q,))
    p.start()
    print(q.get())
    p.join()
    q.close()
```

## Pipe

```python
from multiprocessing import Process, Pipe

def f(p):
    p.send([42, None, 'hello'])

if __name__ == '__main__':
    parent, child = Pipe()  # parent는 receive point, child는 send point
    p = Process(target=f, args=(child,))
    p.start()
    print(parent.recv())
    p.join()
    parent.close()
```
