# Quick Union Improvements

## Weighting

* 각 트리마다 노드의 개수를 기록하고 비교해서 큰 트리가 작은 트리의 노드가 되지 않도록 처리
* 트리의 높이가 커지는 것을 예방할 수 있음

```python
class QuickUnion:
    
    def __init__(self, n):
        self.id = [i for i in range(n)]
        self.id_length = n
        self.sz = [1 for __ in range(n)]

    def root(self, i):
        while i != self.id[i]:
            i = self.id[i]
        return i

    def connected(self, p, q):
        return self.root(p) == self.root(q)

    def union(self, p, q):
        i = self.root(p)
        j = self.root(q)
        if i == j:
            return
        if self.sz[i] < self.sz[j]:
            self.id[i] = j
            self.sz[j] += self.sz[i]
        else:
            self.id[j] = i
            self.sz[i] += self.sz[j]
```

* 노드의 개수가 N일 때, 트리의 최대 높이는 `lg(N)`이 됨

알고리즘|init|union|find
--|-|-|-
복잡도|N|lgN|lgN

## Path Compression

* 트리 아이디어는 결국 루트 노드까지 올라가면서 탐색하는 시간이 걸림
* 탐색한 노드 역시 루트 노드가 같은 상태임
* 탐색한 노드들의 부모 노드를 바로 루트 노드로 바꾸는(트리를 평평하게 만들기) 아이디어

```python
    def root(self, i):
        while i != self.id[i]:
            self.id[i] = self.id[self.id[i]]  # only one extra code
            i = self.id[i]
        return i
```

* 루트 노드가 아니라 부모의 부모 노드를 가리키는 알고리즘이지만 현실에서는 충분히 쓸 수 있음

## 정리

알고리즘|최악의 경우 수행시간
------|--------------
quick-find|M * N
quick-union|M * N
weighted quick-union|N + M lgN
quick-union + path compression|N + M lgN
weighted quick-union + path compression|N + M lg*N

> lg* 는 1의 값을 얻기 위해 수행해야만 하는 lg 연산의 횟수
