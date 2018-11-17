# 우선순위 큐

가장 높은 우선순위를 가진 항목부터 enque하는 자료구조, 리스트와 같은 자료구조에서 우선순위를 적용하려면 전체 데이터를 옮겨야만 하는 등의 문제가 일어남

## 우선순위 큐를 사용하는 영역

1. Event-driven simulation
2. Numerical computation
3. Data compression
4. Graph searching
5. Number theory
6. AI
7. Statistics
8. OS
9. Discrete optimization
10. Spam filtering

## Implementation

```python
# A simple implementation of Priority Queue 
# using Queue. 
class PriorityQueue(object): 
    def __init__(self): 
        self.queue = [] 
  
    def __str__(self): 
        return ' '.join([str(i) for i in self.queue]) 
  
    # for checking if the queue is empty 
    def isEmpty(self): 
        return len(self.queue) == [] 
  
    # for inserting an element in the queue 
    def insert(self, data): 
        self.queue.append(data) 
  
    # for popping an element based on Priority 
    def delete(self): 
        try: 
            max = 0
            for i in range(len(self.queue)): 
                if self.queue[i] > self.queue[max]: 
                    max = i 
            item = self.queue[max] 
            del self.queue[max] 
            return item 
        except IndexError: 
            print() 
            exit() 
```

## 시간복잡도

max를 찾는 과정에서 O(N)의 시간복잡도
