# Sheffling

* 가장 간단한 방법은 각 요소마다 난수를 지정하고 정렬하는 것.
* 시간복잡도가 O(nlogn)보다 아래로 내려갈 수 없음.

## Knuth Shuffle

* 각 항목을 돌면서 난수를 생성하고 난수에 해당하는 인덱스와 swap
* 이렇게 구현할 경우 O(n)의 시간복잡도

```python
from random import randint

def shuffle(cards):
    length = len(cards)
    for i in range(length):
        j = randint(0, i)
        cards[i], cards[j] = cards[j], cards[i]
```
