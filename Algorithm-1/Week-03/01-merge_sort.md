# 합병 정렬

* 합병 정렬은 분할 정복 알고리즘이다.

```python
def merge_sort(list):
    length = len(list)
    if length > 1:
        mid = length // 2
        left_list = list[:mid]
        right_list = list[mid:]

        merge_sort(left_list)
        merge_sort(right_list)

        i = 0
        j = 0
        k = 0

        length_right = len(right_list)
        length_left = len(left_list)

        while i < length_left and j < length_right:
            if left_list[i] < right_list[j]:
                list[k] = left_list[i]
                i += 1
            else:
                list[k] = right_list[j]
                j += 1
            k += 1

        while i < length_left:
            list[k] = left_list[i]
            i += 1
            k += 1

        while j < length_right:
            list[k] = right_list[j]
            j += 1
            k += 1

    return list
```

## 시간복잡도

O(NlnN)

## 공간복잡도

O(N)

## 합병 정렬에 향상시키기

1. 작은 크기의 하위 배열에 대하여 삽입 정렬을 이용한다.
2. 미리 정렬되었는지 확인하기: 두 하위 배열 중 첫 번째 배열의 마지막 값이 두 번째 배열의 첫 번째 값보다 작다면 이미 정렬이 된 상태라고 할 수 있다.
3. 메모리 접근성 향상시키기
