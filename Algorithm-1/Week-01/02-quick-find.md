# Quick Find

## Eager Approach

### 자료구조

* 각 객체들은 배열을 이루고 있어서 각자 고유한 인덱스 값을 가지고 있다.
* Find: 각 객체들이 같은 값인지 확인
* Union: 입력받은 두 객체의 값을 같은 값(두 번째 객채의 값)으로 변경 - 1번째 객체의 값에 2번째 객체의 값을 입력

#### 예시

id|0|1|2|3
--|-|-|-|-
-|0|1|2|3

`Union(3, 2)` 실행 후

id|0|1|2|3
--|-|-|-|-
-|0|1|2|2

실행 후에는 `Find(2, 3) == True`

### 구현

```python
class QuickFind:
    
    def __init__(self, n):
        self.id = [i for i in range(n)]
        self.id_length = n

    def connected(self, p, q):
        return self.id[p] == self.id[q]

    def union(self, p, q):
        p_id = self.id[p]
        q_id = self.id[q]
        
        for i in range(self.id_length):
            if self.id[i] == p_id:
                self.id[i] = q_id
```

### 분석

분석의 기준은 **얼마나 많이 리스트에 접근하는가?**

알고리즘|init|union|find
--|-|-|-
복잡도|N|N|1

* n개의 객체에 대해 n번의 union을 실행한다면 n^2의 복잡도: 매우 비효율적(확장성이 없음)

* 비효율적: 10배 더 빠른 컴퓨터가 있어도 입력량이 10배보다 더 많아진다면 오히려 단위시간당 처리속도는 낮아짐
