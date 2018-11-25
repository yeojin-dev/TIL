# 정지 문제

어떤 알고리즘과 입력값이 주어졌을 때, 알고리즘이 다항시간 내에 완료할지 아니면 무한루프에 빠질지 판별하는 알고리즘이 존재할까?

## 존재하지 않는다 - 증명(귀류법)

만약 정지 문제에 대한 알고리즘이 있다고 가정하고 이를 `halt(a, i)`이라고 정의(true - 다항시간 내 완료, false - 무한루프)하자. 이에 대해 다음과 같은 알고리즘을 가정할 수 있다.

```
function trouble(string s)
    if halt(s, s) == false
        return true
    else
        loop forever
```

여기서 `trouble()` 자체를 나타내는 입력값을 t라고 가정했을 때, `trouble(t)`는 어떤 결괏값을 보여줄까?

### halt(t, t)가 true를 리턴한다면

`trouble(t)`는 무한루프에 빠질 것이며 이는 `halt(t, t)`가 예상한 값과 모순된다.

### halt(t, t)가 false를 리턴한다면

`trouble(t)`는 true 값을 리턴하고 종료될 것이며 이는 `halt(t, t)`가 예상한 값과 모순된다.

즉, 최초의 가정인 `halt(a, i)` 알고리즘이 존재한다는 가정이 잘못되었음을 알 수 있다.
