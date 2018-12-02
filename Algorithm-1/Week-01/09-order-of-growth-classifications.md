# 시간복잡도 순서

1, logN, N, NlogN, N^2, N^3, 2^N

* NlogN 이하의 복잡도면 좋은 알고리즘

## binary search

```python
def binary_search(list, value, low=0, high=0):
    if low > high:
        return False
    mid = (low + high) // 2
    if value > list[mid]:
        return binary_search(list, value, mid + 1, high)
    elif value < list[mid]:
        return binary_search(list, value, low, mid - 1)
    else:
        return mid
```

### 분석

```
T(N) <= T(N/2) + 1 <= T(N/4) + 1 + 1 ... <= T(N/N) + 1 + 1 ... + 1 = 1 + logN
```

### 3-sum 문제에 적용하기

1. 입력값을 정렬한다.
2. 2개의 조합에 대해서 `(조합의 합) + -1`의 값이 입력값 리스트에 존재하는지 확인(이진 탐색한다.

이 경우 시간복잡도는 N^2(정렬의 시간복잡도) * logN(이진 탐색)으로 N^3보다 개선되었음을 알 수 있다.
