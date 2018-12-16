# Convex Hull

볼록 껍질(평면의 점들을 이어 만든 도형에 평면의 모든 점이 들어있을 때) 만들기 알고리즘

## 사용 용도

1. 어떤 점에서 가장 멀리 있는 점은 볼록 껍질의 점 중의 하나이다.
2. 어떤 점에서 다른 점으로 이동하는 가장 짧은 거리는 직선이나거나 볼록 껍질의 점들을 이은 직선이다.

## 그레이엄 스캔

1. 점들 중 가장 아래에 있는 점들을 선택함.
2. 1번의 점에서 나머지 점들을 하나씩 이었을 때의 각도를 계산함.
3. 가장 낮은 각도의 점부터 선택해 나감.
4. 3에서 점을 선택할 때 아래와 같은 조건을 부여.
    * 선택된 점이 이전 점에서 왼쪽(반시계 방향)이면 추가.
    * 오른쪽이면 이전 점만 남기고 기존의 점은 제거(교체).
5. 모든 점에 대한 탐색이 끝나면 완료.

### Implementaion

```python
def convex_hull(points):
    points = sorted(set(points))

    if len(points) <= 1:
        return points

    def cross(o, a, b):
        return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]
```
