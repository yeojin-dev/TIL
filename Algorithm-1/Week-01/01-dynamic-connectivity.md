# Dynamic Connectivity

어떤 객체 집합이 정의되어 있고 두 객체 사이의 연결 정보들이 주어졌을 때, 어떤 두 객체가 연결되어 있는지 알아내는 알고리즘(경로 정보가 아닌 연결 여부만 알아내기)

## Modeling the objects

1. 각 객체들의 이름은 숫자로 추상화: 배열(리스트)의 인덱스로 사용할 수 있도록
2. `연결되어 있다(is connected to)`는 표현을 이산수학의 동치 관계(equivaience relation)로 가정
    1. reflextive: 모든 객체는 자기 자신과 연결되어 있음
    2. symmetric: p->q => q->p
    3. transivitve: p->q, q->r => p->r
3. 연결 요소(connected components)는 집합으로 표현할 수 있는데 이 집합은 전체 객체 집합의 부분집합임

## Implementing the operations

1. Find query: 2개의 객체가 같은 객체인지 확인
2. Union command: 2개의 객체가 같은 집합의 원소인지 확인

## Union find data type(API)

1. 오브젝트의 개수 N은 매우 크다.
2. 연산의 개수 M 역시 매우 크다.
3. Find, Union이 서로 섞인 순서로 연산을 처리해야 한다.

## Dynamic-connectivity client

1. 객채의 개수, 연결 정보는 표준 입력 처리
2. 연결 정보는 여러 쌍의 객체 인덱스를 받는데 연결이 이미 되어 있는 객체들이면 출력하지 않고 연결되지 않은 상태라면 출력

```
다음과 같은 연결 정보가 주어졌을 때, 연결 요소들은 모두 몇 개인가?

{ 1-2, 3-4, 5-6, 7-8, 7-9, 2-8, 0-5, 1-9 }

정답: 3개 - {0, 5, 6}, {3, 4}, {1, 2, 7, 8, 9}
```
