# 선택 정렬

1. 주어진 리스트 중에 최솟값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다.
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

## 구현

```python
def selection_sort(list):
    length = len(list)
    for i in range(length - 1):
        min_index = i
        for j in range(i, length):
            if list[min_index] > list[j]:
                min_index = j
        if min_index != i:
            list[i], list[min_index] = list[min_index], list[i]
    return list
```

## 분석

시간복잡도는 O(N^2)
