# 이벤트 중심 시뮬레이션

충돌할 수도 있는 여러 입자들의 시뮬레이션

```java
public class Ball
{
    private double rx, ry;
    private double vx, vy;
    private final double radius;
    public Ball()
    {

    }
    public void move(double dt)
    {
        if ((rx + vx*dt < radius) || (rx + vx*dt > 1.0 - radius)) { vx = -vx; }
        if ((ry + vy*dt < radius) || (ry + vy*dt > 1.0 - radius)) { vy = -vy; }
        rx = rx + vx*dt;
        ry = ry + vy*dt;
    }
    public void draw()
    {
        StdDraw.filledCircle(rx, ry, radius);
    }
}
```

## 문제점

* 매 순간마다 입자의 충돌을 계산하면 입자의 수가 많고 dt의 값이 작을수록 연산량이 커짐

## 해결책

* 충돌을 물리 법칙에 따라 예상하고 시간순서(시간 우선순위)에 따라 우선순위 큐에 예측한 충돌 데이터를 저장
* 시간 우선순위에 따라 충돌 이벤트를 발생시킴(최상위 노드 데이터를 반영)
* 입자들은 우선순위 큐에 따른 운동 변경 이외에는 항상 직선 운동만 함
