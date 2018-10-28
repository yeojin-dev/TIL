# 배열 크기 조정하기

> 파이썬에서는 일반적으로 해당하지 않는 내용이라 JAVA 기준으로 작성하였음

## 꽉 차있을 경우에 배열 확대하기

```java
private void resize(int capacity)
{
    String[] copy = new String[capacity];
    for (int i = 0; i < N; i++)
        copy[i] = s[i];
    s = copy;
}
```
## 줄어들었을 경우에 배열 줄이기

확대와는 다르게 줄이는 경우는 어느 정도 '공간을 남겨두고' 줄여야 함. 그렇지 않으면 줄인 이후 pop() 실행 시 다시 배열을 확대해야만 하기 때문임.

```java
public String pop()
{
    String item = s[--N];
    s[N] = Null;
    if (N > 0 && N == s.length/4) resize(s.length/2);
    return item;
}
```

# Linked List vs. Array

* 링크드 리스트는 전체적으로 메모리 접근이 많아서 느리지만 안정적임
* 배열은 속도가 빠르고 메모리 효율성이 좋지만 배열 크기를 조정해야하는 상황이 올 경우 매우 느려지기 때문에 안정적이지 않음
