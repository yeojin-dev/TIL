# Stack and Queue

* Stack: LIFO
* Queue: FIFO

# Modular Programming

## 인터페이스와 구현을 완전히 분리해라.

1. 인터페이스: 사용자는 구현과는 관련이 없다.
2. 구현: 개발자는 사용자가 어떻게 이용할지는 고려하지 않는다.

# Stack

## ADT

### push

스택에 데이터 삽입

### pop

스택의 데이터를 출력하고 제거

### is_empty

스택이 비어있는지 확인

## Linked List and Stack

```python
class Node:
    def __init__(self, item, next=None):
        self.item = item
        self.next = next

class LinkedStack:
    def __init__(self):
        self.first = None
    def is_empty(self):
        return self.first == None
    def push(self, item):
        old_first = self.first
        self.first = Node(item, old_first)
    def pop(self):
        result = self.first.item
        self.first = self.first.next
        return result
```

## Array and Stack

```python
class ArrayStack:
    def __init__(self, capacity):
        self.array = capacity * [None]
        self.n = 0
    def is_empty(self):
        return self.n == 0
    def push(self, item):
        self.array[self.n] = item
        self.n += 1
    def pop(self):
        self.n -= 1
        return self.array[self.n]
```

## 고려할 점들

1. 언더플로우: 빈 스택에서 pop() 실행할 경우에 예외처리
2. 오버플로우: capacity가 가득 찼을 경우 resizing
3. None item: None 객체를 삽입할 수 있도록 할 것인가?
4. Loitering: pop()의 경후 해당 포인터를 None으로 연결시켜야 누수를 방지할 수 있음
