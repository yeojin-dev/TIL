# Timsort

* 파이썬의 시스템 정렬
* 기본적으로 합병 정렬이지만 충분히 작은 리스트의 경우에는 삽입 정렬을 사용함
* 삽입 정렬은 최선의 경우에 시간복잡도가 O(N)이기 때문임
* 현실의 정렬 사례에서는 부분적으로는 정렬이 이미 된 케이스가 많기 때문임

## 구현

```python
def insertion_sort(the_array):
    l = len(the_array)
    for index in range(1, l):
        value = the_array[index]
        pos = binary_search(the_array, value, 0, index - 1)
        the_array = the_array[:pos] + [value] + the_array[pos:index] + the_array[index+1:]
    return the_array

def merge(left, right):
    if not left:
        return right
    if not right:
        return left
    if left[0] < right[0]:
        return [left[0]] + merge(left[1:], right)
    return [right[0]] + merge(left, right[1:])


def timsort(the_array):
    runs, sorted_runs = [], []
    l = len(the_array)
    new_run = [the_array[0]]
    for i in range(1, l):
        if i == l-1:
            new_run.append(the_array[i])
            runs.append(new_run)
            break
        if the_array[i] < the_array[i-1]:
            if not new_run:
                runs.append([the_array[i-1]])
                new_run.append(the_array[i])
            else:
                runs.append(new_run)
                new_run = []
        else:
            new_run.append(the_array[i])
    for each in runs:
        sorted_runs.append(insertion_sort(each))
    sorted_array = []
    for run in sorted_runs:
        sorted_array = merge(sorted_array, run)
```