# Python Multiprocessing Package(2)

파이썬에서 멀티프로세싱을 위한 패키지

4. Manager
5. Lock
6. Pool: 같은 함수에 서로 다른 데이터(list)를 입력해서 처리할 때 사용

```python
from multiprocessing import Process, Manager

def f(d, l):
    d[1] = '1'
    d['2'] = 2
    d[0.25] = None
    l.reverse()

if __name__ == '__main__':
    manager = Manager()
    d = manager.dict()
    l = manager.list(range(10))

    p = Process(taget=f, args=(d, l,))
    p.start()
    p.join()

    print(d)
    print(l)
```

```python
from multiprocessing import Process, Lock

def f(l, i):
    l.acquire()
    print('hello world ' + i)
    l.release()

if __name__ == '__main__':
    lock = Lock()

    for num in range(10):
        Process(target=f, args=(lock, str(num))).start()
        print('gogo!')
```