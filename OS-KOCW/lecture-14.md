# Process Synchronization 4

## Concurrency Control

* 모니터 코드가 더 객체지향적이고 가독성이 높은 편임
* 모니터와 세마포어는 서로 바꿔서 작업할 수 있음

### Dining Philosophers Example

```c
monitor dining_philosopher
{
    enum { thinking, hungry, eating } state[5];
    condition self[5];
    void pickup(int i) {
        state[i] = hungry;
        test(i);
        if (state[i] != eating) {
            self[i].wait();
        }
    }
    void putdown(int i) {
        state[i] = thinking;
        test((i + 4) % 5);
        test((i + 1) % 5);
    }
    void test(int i) {
        if ((state[(i + 4) % 5] != eating) && (state[i] == hungry) && (state[(i + 1) % 5] != eating)) {
            state[i] = eating;
            self[i].signal();
        }
    }
    void init() {
        for (int i = 0; i < 5; i++)
            state[i] = thinking;
    }
}

Each Philosopher
{
    pickup();
    eat();
    putdown();
    think();
} while (1)
```
