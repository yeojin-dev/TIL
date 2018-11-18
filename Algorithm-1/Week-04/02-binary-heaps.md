# 이진 힙

이진 힙은 전 이진 트리 중의 하나로 부모 노드가 자식 노드보다 작은(또는 큰) 전 이진 트리를 이진 힙이라고 부름

## 삽입

1. 전 이진 트리의 가장 마지막 노드에 삽입
2. 부모 노드와 값을 비교해 만약 부모 노드보다 작은 값을 가지고 있다면 swap
3. 부모 노드보다 클 때까지 2를 계속 수행

## 삭제

1. 최상위 노드와 마지막 노드를 swap
2. 원래 최상위 노드는 pop
3. 현재 최상위 노드는 전체 트리가 힙 조건을 만족할 때까지(두 자식 노드보다 큰 경우) 자식 노드와 swap

## Implementation

```python
# A Python program to demonstrate common binary heap operations 
  
# Import the heap functions from python library 
from heapq import heappush, heappop, heapify  
  
# heappop - pop and return the smallest element from heap 
# heappush - push the value item onto the heap, maintaining 
#             heap invarient 
# heapify - transform list into heap, in place, in linear time 
  
# A class for Min Heap 
class MinHeap: 
      
    # Constructor to initialize a heap 
    def __init__(self): 
        self.heap = []  
  
    def parent(self, i): 
        return (i-1)/2
      
    # Inserts a new key 'k' 
    def insertKey(self, k): 
        heappush(self.heap, k)            
  
    # Decrease value of key at index 'i' to new_val 
    # It is assumed that new_val is smaller than heap[i] 
    def decreaseKey(self, i, new_val): 
        self.heap[i]  = new_val  
        while(i != 0 and self.heap[self.parent(i)] > self.heap[i]): 
            # Swap heap[i] with heap[parent(i)] 
            self.heap[i] , self.heap[self.parent(i)] = ( 
            self.heap[self.parent(i)], self.heap[i]) 
              
    # Method to remove minium element from min heap 
    def extractMin(self): 
        return heappop(self.heap) 
  
    # This functon deletes key at index i. It first reduces 
    # value to minus infinite and then calls extractMin() 
    def deleteKey(self, i): 
        self.decreaseKey(i, float("-inf")) 
        self.extractMin() 
  
    # Get the minimum element from the heap 
    def getMin(self): 
        return self.heap[0] 
```

## 시간복잡도

O(logN)
