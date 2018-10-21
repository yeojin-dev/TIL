# Quick Find 응용

## Percolation(침투)

![percolation](https://www.math.psu.edu/treluga/textbook/figures/siteperc_n=16.png)

* N * N grid에서 p의 확률로 흰색, 1-p의 확률로 검은색이 결정되었다고 했을 때, 흰색 칸만을 통해서 위에서 아래로 침투할 수 있는지 판별하는 문제
* 다양한 물리계에서 사용할 수 있음(전기, 유체, 상전이 등)
* SNS에서 어떤 두 사용자가 건너건너 친구인지 알아내는 문제

### Monte-Carlo Simulation

* 퍼콜레이션 문제를 시뮬레이션
* p의 확률을 높여가다가 침투가 가능해졌을 때 열린 공간의 비율을 계산
* 위 계산을 수백만 번 계산

## 정리

1. 문제와 모델을 정의
2. 알고리즘으로 해결
3. 성능 향상(복잡도 계산)
