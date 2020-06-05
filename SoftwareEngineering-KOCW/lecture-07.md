# 클래스 다이어그램

## UML

* 분석, 설계를 비주얼화, 문서화하기 위한 그래픽 언어
    * Unified : 이전의 각종 방법들의 통합
    * Modeling : 객체지향 분석 설계를 위한 비주얼 모델링
    * Language : 모형화된 지식을 표현

### UML은 ( )이다.

* 시스템에 대한 지식을 찾고 표현하기 위한 언어
* 시스템을 개발하기 위한 탐구 도구
* 비주얼 모델링 도구
* 근거가 잘 정리된 가이드라인
* 분석, 설계 작업의 마일스톤
* 실용적 표준

### UML은 ( )이 아니다.

* 비주얼 프로그래밍 언어 환경
* 데이터베이스 표현 도구
* 개발 프로세스
* 모든 문제의 해결책
* 품질 보증 방안

## UML 2.5 다이어그램 체계

![UML 2.5](https://www.uml-diagrams.org/uml-25-diagrams.png)

## UML 클래스 다이어그램

* 객체지향 시스템에 존재하는 클래스, 클래스 안의 필드, 메소드, 서로 협력하거나 상속하는 클래스 사이의 연결 관계를 나타내는 그림

* 나타내지 않는 것
    * 클래스가 서로 어떻게 상호작용 하는지
    * 자세한 알고리즘
    * 특정한 동작이 어떻게 구현되는지

### 클래스 나타내기

![클래스](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSNWs2iCDUjrcUMOvrJIvviIpIWZX7gqOPtnwNs-l-xOxhwCe6-&usqp=CAU)

* 박스 위에 클래스 이름
* 추상 클래스는 이탤릭체
* 인터페이스 클래스는 \<\<interface\>\> 추가
* 속성은 객체가 가지는 모든 필드를 포함
* 아주 흔한 메소드는 생략, 상속된 메소드도 포함할 필요 없음

#### 클래스 속성

**(visibility) (name): (type)[count] = default value**

* visibility
    * \+ : public
    * \# : protected
    * \- : private
    * ~ : package
    * / : derived
    * underline : static

#### 클래스 오퍼레이션/메소드

**(visibility) (name)(parameters): (return_type)**

* visibility
    * \+ : public
    * \# : protected
    * \- : private
    * ~ : package
    * / : derived
    * underline : static

* 생성자나 void 메소드는 return_type 생략

#### 클래스 사이의 관계

* 일반화(generalization) : 상속 관계
    * 클래스 사이의 상속
    * 인터페이스 구현

* 연관(association) : 사용 관계
    * 의존
    * 집합 : 어떤 클래스가 다른 클래스의 모임으로 구성
    * 합성 : 포함된 클래스가 컨테이너 클래스가 없이는 존재할 수 없는 집합관계의 변형

##### 일반화 관계

![generalization](https://sourcemaking.com/files/sm/images/uml/img_120.jpg)

* 부모를 향한 화살표로 표시되는 하향 계층 관계
* 클래스 : 실선, 검은 화살표
* 추상 클래스 : 실선, 흰 헤드 화살표
* 인터페이스 : 점선, 흰 헤드 화살표

##### 연관 관계

![association](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQ0-12TB0T85Juh4D9o43xasfBH4wxTyPZFwrkDJyrQVAqase_O&usqp=CAU)

* 연관 : 어떤 클래스의 인스턴스가 작업을 수행하기 위하여 다른 클래스를 알아야 하는 경우

* 다중도(muliplicity)
    * \* : 0, 1 or more
    * 1 : 정확히 1개
    * 2..4 : 2개 내지 4개
    * 3..* : 3개 이상

* 방항샹(navigability) : 질의의 방향, 객체 사이의 선으로 표시하며 양쪽 방향인 경우는 화살표시 없음

* 연관의 타입
    * 집합(aggregation) : **contains** 포함하고 있는 클래스 쪽에 하얀 다이아몬드 표시
        * ShoeStore - NikiShoes
    * 합성(composition) : **이 목적을 위하여만 포함됨**
        * 집합보다 끈끈한 관계
        * 부분은 전체가 살고 죽느냐에 좌우됨
        * 포함하고 있는 클래스 쪽에 검은 다이아몬드
        * Hand - Finger
    * 의존(dependency) : **일시적 사용** 점선에 검은 헤드 화살표로 표시
        * LotteryTicket - Random

![class diagram sample](https://www.uml-diagrams.org/thumbnails/library-domain-uml-class-diagram-example.png)

#### 전파 현상(propagation)

* 전체 개념의 오퍼레이션이 부분 개념의 오퍼레이션에 의해 구현되는 현상
* 동시에 부품의 속성이 전체 개념에 전파되는 현상
* 전파와 전체, 부분 개념의 관계는 상속과 일반화 관계와 유사

## 패키지 다이어그램

![패키지 다이어그램](https://i.stack.imgur.com/5AMol.png)

## 클래스 다이어그램 작성 방법

* 반복, 점증적 방법 : 초벌로 작성 후 계속 추가, 삭제
    1. 클래스 후보 선정
    2. 관계 파악
        * 연관 관계와 속성 추가
        * 일반화 관계 파악
        * 클래스의 responsibility 파악, 오퍼레이션 파악
    3. 설계 패턴 적용
    4. 2~3번 반복
