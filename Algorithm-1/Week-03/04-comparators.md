# Comparators

> 파이썬에서는 일반적으로 해당하지 않는 내용이라 JAVA 기준으로 작성하였음

특정 상황에 맞는 정렬 기준을 설정해주기 위해서는 인터페이스 구현이 필요하다.

```java
private static class ByName implements Comparator<Student>
{
    public int compare(Student v, Student w)
    {
        return v.name.compareTo(w.name);
    }
}
```

파이썬에서도 key 패러미터를 지정해줄 수 있다.

```python
list_sample = [list1, list2, list3]
list_sample.sort(key=lambda x: len(x))  # 리스트의 요소 개수를 기준으로 정렬
```
