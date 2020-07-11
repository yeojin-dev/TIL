# 디자인 패턴

* 좋은 아키텍처의 모임

## 설계 작업에 대한 도전

### 소프트웨어 설계는 어려운 일

* 문제를 잘 분할하고
* 유연하고 잘 모듈화한 우아한 디자인이 되어야 함

### 설계는 시행 착오의 결과

### 성공적인 설계가 존재

* 두 설계가 똑같은 일은 없음
* 반복되는 특성

## 디자인 패턴

* 공통된 소프트웨어 문제에 오랫동안 사용되어 검증된 솔루션
* 디자인 작업에 사용되는 공통 언어, 의사소통을 항상 시키며 구현, 문서화에 도움
* 패턴은 전문 기술을 담고 있고 그것을 전수시킬 수 있음

## GoF 패턴

1. 생성 패턴(추상 객체 인스턴스화)

* 추상 팩토리, 팩토리, 빌더, 프로토타입, 싱글톤

2. 구조 패턴(객체 결합)

* 어댑터, 브리지, 컴포지트, 데코레이터, 퍼사드, 플라이웨이트, 프록시

3. 행위 패턴(객체 간 커뮤니케이션)

* 책임 체인, 커맨드, 인터프리터, 반복자, 중재자, 메멘토, 옵서버, 상태, 전략, 템플릿 메소드, 비지터

### 싱글톤 패턴

#### 문제

* 클래스의 객체를 하나만 만들어야 하는 경우가 있음
* 2개 이상 만들지 못 하게 하고 싶을 때(키보드 리더, 프린터 스풀러, 점수기록표)

#### 고려해야 하는 이유

* 객체를 많이 만드는 것은 시간이 많이 걸림
* 잉여 객체는 메모리 낭비
* 여러 가지 다른 객체가 메모리에 떠 있는 것은 유지보수에 어려움을 줌

#### 그 타입의 유일한 객체

* 많아야 하나의 인스턴스를 가짐을 보장
* 인스턴스에 대하여 어디서든 접근할 수 있게 하여야 함
* 프로그래머가 인스턴스를 없애버리는(또는 더 생성할) 관리 책무를 빼앗음
* 사용자에게 유일한 인스턴스를 접근할 수 있는 메소드를 제공

#### 구현

* 생성자를 밖에서 부르지 못 하도록 private로 만듦
* 클래스 안에 클래스의 인스턴스를 static private 타입으로 선언
* 단일 인스턴스를 접근할 수 있는 public getInstance() 메소드를 구현

```python
class OnlyOne:
    class __OnlyOne:
        def __init__(self, arg):
            self.val = arg
        def __str__(self):
            return repr(self) + self.val
    instance = None
    def __init__(self, arg):
        # GIL이 있기 때문에 Locking 필요 없음
        if not OnlyOne.instance:
            OnlyOne.instance = OnlyOne.__OnlyOne(arg)
        else:
            OnlyOne.instance.val = arg
    def __getattr__(self, name):
        return getattr(self.instance, name)

x = OnlyOne('sausage')
print(x)
y = OnlyOne('eggs')
print(y)
z = OnlyOne('spam')
print(z)
print(x)
print(y)
print(`x`)
print(`y`)
print(`z`)
output = '''
<__main__.__OnlyOne instance at 0076B7AC>sausage
<__main__.__OnlyOne instance at 0076B7AC>eggs
<__main__.__OnlyOne instance at 0076B7AC>spam
<__main__.__OnlyOne instance at 0076B7AC>spam
<__main__.__OnlyOne instance at 0076B7AC>spam
<__main__.OnlyOne instance at 0076C54C>
<__main__.OnlyOne instance at 0076DAAC>
<__main__.OnlyOne instance at 0076AA3C>
'''
```

```java
public class RandomGenerator {
    private static RandomGenerator gen = null;
    public static synchronized RandomGenerator getInstance() {
        if (gen == null) {
            gen = new RandomGenerator();
        }
        return gen;
    }
}
```

### 팩토리 패턴

* 팩토리 : 다른 클래스의 인스턴스를 쉽게 생성하고 리턴하는 임무를 가진 클래스
    * 생성자를 부르는 대신 팩토리 클래스의 정적 메소드를 사용하여 객체를 셋업(사용과 생성을 분리)
    * 구축 정보를 사용 정보에서 분리(응집을 높이고 결합을 약하게 하기 위하여)하여 객체의 생성과 관리를 쉽게
    * 서브 클래스의 인스턴스화를 지연하는 효과(다형성)

#### 구현

```python
# A simple static factory method.
from __future__ import generators
import random

class Shape(object):
    # Create based on class name:
    @staticmathod
    def factory(type):
        #return eval(type + "()")
        if type == "Circle":
            return Circle()
        if type == "Square":
            return Square()
        assert 0, "Bad shape creation: " + type

class Circle(Shape):
    def draw(self): print("Circle.draw")
    def erase(self): print("Circle.erase")

class Square(Shape):
    def draw(self): print("Square.draw")
    def erase(self): print("Square.erase")

# Generate shape name strings:
def shapeNameGen(n):
    types = Shape.__subclasses__()
    for i in range(n):
        yield random.choice(types).__name__

shapes = [Shape.factory(i) for i in shapeNameGen(7)]

for shape in shapes:
    shape.draw()
    shape.erase()
```

```java
public class ImageReaderFactory
{
    public static ImageReader createImageReader (InputStream is) {
        int imageType = figureOutImageType(is);
        switch (imageType) {
            case ImageReaderFactory.GIF:
                return new GIFReader(is);
            case ImageReaderFactory.JPEG:
                return new JPEGReader(is);
        }
    }
}
```

### 데코레이터 패턴

* 데코레이터 : 다른 객체의 행위를 바꾸거나 기능을 추가하는 객체
    * 객체에 동적으로 책무를 추가
    * 데코레이션 당하는 객체가 데코레이터를 알지 못 함
    * 데코레이터는 감싸려는 객체에 통일된 인터페이스를 제공해야만 함

```python
def decorator_maker_with_arguments(decorator_arg1, decorator_arg2, decorator_arg3):
    def decorator(func):
        def wrapper(function_arg1, function_arg2, function_arg3) :
            "This is the wrapper function"
            print("The wrapper can access all the variables\n"
                  "\t- from the decorator maker: {0} {1} {2}\n"
                  "\t- from the function call: {3} {4} {5}\n"
                  "and pass them to the decorated function"
                  .format(decorator_arg1, decorator_arg2,decorator_arg3,
                          function_arg1, function_arg2,function_arg3))
            return func(function_arg1, function_arg2,function_arg3)

        return wrapper

    return decorator
```

### 퍼사드 패턴

* 퍼사드 : 서로 다른 인터페이스 위에 통일된 인터페이스 또는 복잡한 인터페이스 위에 간단한 인터페이스를 제공하는 객체

```java
public class RemoteControl {
    
    public void turnOn()
    {
        System.out.println("TV를 켜다");
    }
    public void turnOff()
    {
        System.out.println("TV를 끄다");
    }
}

public class Movie {
    
    private String name="";
    
    public Movie(String name)
    {
        this.name = name;
    }
    
    public void searchMovie()
    {
        System.out.println(name + " 영화를 찾다");
    }
    
    public void chargeMovie()
    {
        System.out.println("영화를 결제하다");
    }
    public void playMovie()
    {
        System.out.println("영화 재생");
    } 
}

public class Beverage {
    
    private String name="";
    
    public Beverage(String name)
    {
        this.name = name;
    }
    
    public void prepare()
    {
        System.out.println(name + " 음료 준비 완료 ");
    } 
}

public class Facade {
    
    private String beverageName  = "";
    private String movieName = "";
    
    public Facade(String beverage, String movieName)
    {
        this.beverageName = beverage_Name;
        this.movieName = Movie_Name;
    }
    
    public void viewMovie()
    {
        Beverage beverage = new Beverage(beverageName);
        Remote_Control remote= new RemoteControl();
        Movie movie = new Movie(movieName);
        
        beverage.Prepare();
        remote.Turn_On();
        movie.Search_Movie();
        movie.Charge_Movie();
        movie.play_Movie();
    }
}
```

### 플라이웨이트 패턴

* 중복되는 객체는 비효율적임
    * 여러 객체들이 같은 상태를 가지고 있음

* 플라이웨이트 : 어떤 클래스의 인스턴스도 동일한 상태를 갖지 않는다는 보장이 있다면
    * 객체 구축의 노력을 줄이기 위하여 객체들의 동일한 부분을 캐시로 만듦
    * 싱글톤과 유사하나 많은 인스턴스를 가지며 각 객체마다 고유한 상태를 가짐
    * 타입의 인스턴스는 많으나 각 인스턴스에 유사한 부분이 많은 경우에 유용함

### 반복자 패턴

* 반복자 : 집합에 포함된 객체를 검사하여 반복하는 일을 할 수 있도록 표준 방법을 제공하는 객체
    * 클라이언트는 자세한 표현 방법을 몰라도 됨
    * 접근 인터페이스의 단순화

## 디자인 패턴의 선택

* 디자인 패턴이 주어진 문제를 어떻게 해결하고 있는지 스터디
* 주어진 상황에 제일 잘 맞는 패턴이 무엇인지 숙고
* 시스템의 변경, 발전, 재사용 어느 측면이 유력한지 고려하여 적용
