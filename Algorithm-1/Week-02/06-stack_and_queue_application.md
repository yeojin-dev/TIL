# Stack and Queue Apps

JAVA에는 콜렉션 프레임워크가 존재하고 Python에는 리스트 자료형이 존재함.

# 사례

`수집한 웹사이트를 무작위 순서로 보여주기`

1. 배열을 이용하고 인덱스를 무작위로 사용한다. 만약 중복되었으면 다시 뽑는다.
    * 복잡도는 N^2

2. 연결 리스트를 이용해서 무작위 추출 후 삭제한다.
    * 복잡도는 N^4

# Stack의 이용 사례

1. 컴파일러의 파싱 모듈
2. JVM
3. Undo 기능
4. 웹 브라우저의 뒤로가기 기능
5. 컴파일러의 함수 호출

```java
public class Evaluate
{
    public static void main(String[] args)
    {
        Stack<String> ops = new Stack<String>();
        Stack<Double> vals = new Stack<Double>();
        while (!StdIn.isEmpty()) {
            String s = StdIn.readString();
            if (s.equals("("))
                ;
            else if (s.equals("+"))
                ops.push(s);
            else if (s.equals("*"))
                ops.push(s);
            else if (s.equals(")"))
            {
                String op = op.pop()
                if (op.equals("+"))
                    vals.push(vals.pop() + vals.pop());
                else if (op.equals("*"))
                    vals.push(vals.pop() * vals.pop());
            }
            else
                vals.push(Double.parseDouble(s));
        }
        StdOut.println(vals.pop());
    }
}
```
