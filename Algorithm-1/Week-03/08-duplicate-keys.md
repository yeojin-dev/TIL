# Duplicate Keys

퀵 정렬을 우수하지만 중복된 값들이 존재할 경우에는 좀 더 최적화가 필요함.

## 3-way partitioning

* 2개의 인덱스를 설정하고 같은 값들은 두 인덱스 사이에 두도록 구현

## 구현

```python
def partition3(A, l, r):
    lt = l
    i = l
    gt = r
    pivot = A[l]
    while i <= gt:
        if A[i] < pivot:
            A[lt], A[i] = A[i], A[lt]
            lt += 1
            i += 1
        elif A[i] > pivot:
            A[i], A[gt] = A[gt], A[i]
            gt -= 1
        else:
            i += 1
            
    return lt, gt


def quick_sort(A, l, r):
    if l >= r: 
        return
    k = random.randint(l, r)
    A[k], A[l] = A[l], A[k]
    
    lt, gt = partition3(A, l, r)
    quick_sort(A, l, lt - 1)
    quick_sort(A, gt + 1, r)
```
