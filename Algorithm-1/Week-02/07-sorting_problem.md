# 정렬하기 - 어떤 기준에 따라 정렬할 것인가?

정렬의 기준은 클래스의 정의된 메소드에 따라 달라진다.

* JAVA의 경우에는 인터페이스를 활용함
* Python의 경우에는 매직 메소드를 활용함

```
__cmp__(self, other)
__cmp__ is the most basic of the comparison magic methods. It actually implements behavior for all of the comparison operators (<, ==, !=, etc.), but it might not do it the way you want (for example, if whether one instance was equal to another were determined by one criterion and and whether an instance is greater than another were determined by something else). __cmp__ should return a negative integer if self < other, zero if self == other, and positive if self > other. It's usually best to define each comparison you need rather than define them all at once, but __cmp__ can be a good way to save repetition and improve clarity when you need all comparisons implemented with similar criteria.

__eq__(self, other)
Defines behavior for the equality operator, ==.

__ne__(self, other)
Defines behavior for the inequality operator, !=.

__lt__(self, other)
Defines behavior for the less-than operator, <.

__gt__(self, other)
Defines behavior for the greater-than operator, >.

__le__(self, other)
Defines behavior for the less-than-or-equal-to operator, <=.

__ge__(self, other)
Defines behavior for the greater-than-or-equal-to operator, >=.
```