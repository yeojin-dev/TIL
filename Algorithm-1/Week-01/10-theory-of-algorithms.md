# 알고리즘 분석 타입

## 입력값의 타입

1. Base Case: 가장 쉬운 경우, Best Case에 대한 분석으로 어떤 알고리즘이 Base Case보다는 오래 걸릴 것을 보장할 수 있다.
2. Worst Case: 가장 어려운 경우, Worst Case에 대한 분석으로 어떤 알고리즘이 Worst Case보다는 빠를 것을 보장할 수 있다.
3. Average Case: 랜덤한 경우의 분석

## 어떤 케이스에 대해 가장 최적의 알고리즘인가?

1. Big Theta: 알고리즘의 복잡도를 상세하게 규정
2. Big Oh: 알고리즘의 실행시간의 상한
3. Big Omega: 알고리즘의 실행시간의 하한

# 탐색 문제

1. Base Case: 상수 시간
2. Worst Case: O(N)의 복잡도

# three-sum 문제

## Brute Force

* Worst Case에 대해 O(N^3)

## Optimize

* Worst Case에 대해 O(N^2logN)

# 알고리즘 분석 단계

1. 알고리즘 개발, Big Oh 계산
2. Big Oh를 낮추고, Big Omega를 높여 최적화한다.
