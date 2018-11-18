# 힙 정렬

1. 이진 힙을 유지하면서 최댓값을 계속 pop
2. pop 이후에는 다시 남은 트리를 힙으로 만듦

## Implementation

```python
def heapify(arr, n, i): 
    largest = i # Initialize largest as root 
    l = 2 * i + 1     # left = 2*i + 1 
    r = 2 * i + 2     # right = 2*i + 2 
  
    # See if left child of root exists and is 
    # greater than root 
    if l < n and arr[i] < arr[l]: 
        largest = l 
  
    # See if right child of root exists and is 
    # greater than root 
    if r < n and arr[largest] < arr[r]: 
        largest = r 
  
    # Change root, if needed 
    if largest != i: 
        arr[i],arr[largest] = arr[largest],arr[i] # swap 
  
        # Heapify the root. 
        heapify(arr, n, largest) 
  
# The main function to sort an array of given size 
def heapSort(arr): 
    n = len(arr) 
  
    # Build a maxheap. 
    for i in range(n, -1, -1): 
        heapify(arr, n, i) 
  
    # One by one extract elements 
    for i in range(n-1, 0, -1): 
        arr[i], arr[0] = arr[0], arr[i] # swap 
        heapify(arr, i, 0) 
```

## 시간복잡도

N번의 swap이 이루어지고 힙의 크기는 최대 logN이기 때문에 시간복잡도는 O(NlogN)

## 힙 정렬의 특징

* 추가 메모리를 사용하지 않으면서도 O(NlogN)의 시간복잡도를 보장
* 메모리 캐싱이 어렵기 때문에 실제로 잘 사용되지는 않음
