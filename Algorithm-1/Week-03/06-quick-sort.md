# Quick Sort

현실적으로 가장 좋은 성능을 보여주는 정렬.

## 개념

1. 임의의 인덱스를 정하고 해당 인덱스보다 작은 값을 인덱스 왼쪽에, 인덱스보다 큰 값을 인덱스 오른쪽에 배치한다.
2. 인덱스 왼쪽, 오른쪽에 대해 퀵 정렬을 재귀적으로 수행한다.
3. 재귀는 정렬 대상이 되는 리스트의 요소가 1개일 경우에 종료한다.

## 구현

```python
def partition(list, start, end):
    pivot = list[start]
    left = start + 1
    right = end
    done = False
    while not done:
        while left <= right and list[left] <= pivot:
            left += 1
        while left <= right and list[right] > pivot:
            right -= 1
        if right < left:
            done = True
        else:
            list[left], list[right] = list[right], list[left]
    list[start], list[right] = list[right], list[start]  # 피봇 교환
    return right


def quick_sort_cpp_style(list, start, end):
    if start < end:
        pivot = partition(list, start, end)
        quick_sort_cpp_style(list, start, pivot - 1)
        quick_sort_cpp_style(list, pivot + 1, end)
    return list
```

## 분석

* 시간복잡도는 죄선의 경우, 평균적인 경우에는 O(nlogn), 최악의 경우에는 O(N^2)이지만 최악의 경우일 확률은 극히 낮다.
* 공간복잡도의 경우에 퀵 정렬은 추가 공간을 사용하지 않는다.
