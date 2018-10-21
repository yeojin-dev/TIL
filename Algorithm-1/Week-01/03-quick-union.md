# Quick Union

* lazy approach
* 트리 구조로 접근
* union(a, b): a는 b의 root의 자식 노드가 된다.

```python
class QuickUnion:
    
    def __init__(self, n):
        self.id = [i for i in range(n)]
        self.id_length = n

    def root(self, i):
        while i != self.id[i]:
            i = self.id[i]
        return i

    def connected(self, p, q):
        return self.root(p) == self.root(q)

    def union(self, p, q):
        i = self.root(p)
        j = self.root(q)
        self.id[i] = j
```

### 분석

분석의 기준은 **얼마나 많이 리스트에 접근하는가?**

알고리즘|init|union|find
--|-|-|-
복잡도|N|N|N

* 트리가 매우 길어질 수 있음
* 최악의 경우 find 작업에서 모든 노드를 탐색해야만 할 수도 있음
