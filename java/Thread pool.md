# Thread pool

병렬적으로 작업을 하기위해 thread를 늘려서 작업한다.
하지만, 무분별한 Thread 증가는 하드웨어의 제한사항때문에 지양해야한다. 그래서 Thread pool이라는 개념을 이용한다.
java에서는 **Executors, ExcutorService**를 사용한다.



### # Single Thread Pool

```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
```

- 단일 Thread, 실패 시 새로운 Thread를 생성하지 않음



### # Fixed Thread Pool

```java
ExecutorService executorService = Executors.newFixedThreadPool(int nThreads);
```

- 모든 Thread를 사용 중 이라면 Queue에 쌓여 대기한다.
- 실패할때, 새로운 Thread를 생성

#### Cached Thread Pool

```java
ExecutorService executorService = Executors.newCachedThreadPool();
```

#### Scheduler Thread Pool

```java
ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(int corePoolSize);
```

#### Work Stealing Thread Pool

```java
ExecutorService executorService = Executors.newWorkStealingPool(int parallelism);
```