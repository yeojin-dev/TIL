# 삽입 정렬

앞 인덱스부터 차례대로 정렬해나가는 정렬 알고리즘. 정렬을 진행해 나가면서 부분적으로는 정렬된 상태가 된다.

```python
import random

def insertion_sort(list):
    length = len(list)
    for i in range(1, length):
        curr_value = list[i]
        index = i
        while index > 0 and list[index - 1] > curr_value:
            list[index] = list[index - 1]
            index -= 1
        list[index] = curr_value
    return list

num_list = list()

for i in range(20):
    num = random.randrange(500)
    num_list.append(num)

print(insertion_sort(num_list.copy()))
```

## 시간복잡도

O(N^2)

## Best Case

이미 정렬된 상태일 경우. 부분적으로 정렬된 상태인 경우에도 효과적이다.

## Worst Case

역순으로 정렬된 상태일 경우.