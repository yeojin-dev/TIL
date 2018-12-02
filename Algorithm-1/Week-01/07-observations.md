# 3-sum problem

주어진 숫자 리스트에서 서로 다른 3개를 더했을 때 합이 0이 되는 조합은 몇 개인가?

```python
def three_sum(a):
    count = 0
    length = len(a)
    for i in range(length):
        for j in range(i + 1, length):
            for k in range(j + 1, length):
                if a[i] + a[j] + a[k] == 0:
                    count += 1
    return count
```

# 분석 방법들

## Empirical

프로그램을 직접 실행시켜가면서 시간을 측정한다. 만약 매우 느린 프로그램을 분석해야만 한다면 그만큼의 시간이 걸린다.

## Data

데이터 입력의 크기와 실행시간에 대한 관계를 분석함으로써 직접 프로그램을 실행하지 않고도 실행시간을 알 수 있다.

### log-log plot

* X, Y축 모두 log 변환한 값으로 알고리즘 분석
* 직선 형태의 그래프가 나타난다면 그 알고리즘의 시간복잡도는 a*N^b이다. (b는 직선의 기울기)
* 회귀분석을 통해 실행시간을 예측할 수 있다.

### doubling hypothesis

* 입력값의 크기가 N, 2N일 때의 실행시간의 비(2N은 N의 몇 배만큼 시간이 걸렸는지)를 측정하고 로그값을 구하면 b를 구할 수 있고 a는 b를 가정한 후 실헙적으로 계산할 수 있다.
