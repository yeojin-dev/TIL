# Queue

## ADT

### enqueue

큐에 데이터 삽입

### dequeue

큐의 데이터를 출력하고 제거

### is_empty

큐가 비어있는지 확인

```python
class Node:
    def __init__(self, item, next=None):
        self.item = item
        self.next = next

class LinkedQueue:
    def __init__(self, first=None, last=None):
        self.first = first
        self.last = last
    
    def is_empty(self):
        return self.first == None
    
    def enqueue(self, item):
        last = Node(item)
        if self.is_empty():
            self.first = last
        else:
            self.last.next = last
    
    def dequeue(self):
        item = self.first
        self.first = self.first.next
        return item
```
