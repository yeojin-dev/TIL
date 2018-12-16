# Shell Sort

* 삽입 정렬이 어느 정도 정렬된 배열에 대해서는 대단히 빠른 것에 착안
* 삽입 정렬의 최대 문제점: 요소들이 삽입될 때, 이웃한 위치로만 이동
* 즉, 만약 삽입되어야 할 위치가 현재 위치에서 상당히 멀리 떨어진 곳이라면 많은 이동을 해야만 제자리로 갈 수 있다.
* 삽입 정렬과 다르게 셸 정렬은 전체의 리스트를 한 번에 정렬하지 않는다.

## 과정
1. 먼저 정렬해야 할 리스트를 일정한 기준에 따라 분류
2. 연속적이지 않은 여러 개의 부분 리스트를 생성
3. 각 부분 리스트를 삽입 정렬을 이용하여 정렬
4. 모든 부분 리스트가 정렬되면 다시 전체 리스트를 더 적은 개수의 부분 리스트로 만든 후에 알고리즘을 반복
5. 위의 과정을 부분 리스트의 개수가 1이 될 때까지 반복

## Implemention

```python
def shellSort(alist):
    sublistcount = len(alist)//2
    while sublistcount > 0:

      for startposition in range(sublistcount):
        gapInsertionSort(alist,startposition,sublistcount)

      print("After increments of size",sublistcount,
                                   "The list is",alist)

      sublistcount = sublistcount // 2

def gapInsertionSort(alist,start,gap):
    for i in range(start+gap,len(alist),gap):

        currentvalue = alist[i]
        position = i

        while position>=gap and alist[position-gap]>currentvalue:
            alist[position]=alist[position-gap]
            position = position-gap

        alist[position]=currentvalue
```

## 시간복잡도

* 삽입 정렬과 같지만 평균적인 경우에 O(n^1.5)로 더 우수하다.