# Iteration

> 파이썬에서는 일반적으로 해당하지 않는 내용이라 JAVA 기준으로 작성하였음

```java
Iterator<String> i = stack.iterator();
while (i.hasNext())
{
    String s = i.next();
    StdOut.println(s);
}
```

## java.util.Iterator

이터레이터를 만들기 위한 인터페이스. 인터페이스를 구현하는 방식을 통해 자료구조를 더 편리하게 만들 수 있다.
