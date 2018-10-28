# Generics

> 파이썬에서는 일반적으로 해당하지 않는 내용이라 JAVA 기준으로 작성하였음

데이터 타입마다 별도의 스택, 큐를 구현하는 불편함을 해소하기 위한 개념

## 제네릭 문법 사용하지 않을 경우

```java
StackOfObjects s = new StackOfObjects();
Apple a = new Apple();
Orange b = new Orange();
s.push(a)
s.push(b)
a = (Apple) (s.pop());  // Runtime Error
```

## 제네릭 문법 사용할 경우

```java
Stack<Apple> s = new Stack<Apple>();
Apple a = new Apple();
Orange b = new Orange();
s.push(a)
s.push(b)  // Compile Error
a = s.pop();
```

```java
public class Stack<Item>
{
    private Node first = null;

    private class Node
    {
        Item item;
        Node next;
    }

    public boolean isEmpty()
    {
        return first == null;
    }

    public void push(Item item)
    {
        Node oldFirst = first;
        first = new Node();
        first.item = item;
        first.next = oldFirst;
    }

    public Item pop()
    {
        Item item = first.item;
        first = first.next;
        return item;
    }
}
```

> JAVA에서는 제네릭 타입의 배열을 허용하지 않음
