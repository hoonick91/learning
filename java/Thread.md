# Thread

1. critical section

   > synchronized로 나눌 수 있다.

   critical section은 하나의 thread만 들어가서 작업을 할 수 있다.

```java
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;
    
    public void add(int val1, int val2){
        synchronized(this){
            this.sum1 += val1;   
            this.sum2 += val2;
        }
    }
}
```

- 하나의 method에 2개의 thread가 들어갈 수 있다.


```javascript₩
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;
    
    public void add(int val1, int val2){
        synchronized(this){
            this.sum1 += val1;   
        }
        synchronized(this){
            this.sum2 += val2;
        }
    }
}
```



2. Thread와 immutability
   - Thread-safety를 위해 immutability를 유지해야한다.