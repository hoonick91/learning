# Jdk1.8 -> Jdk11

> 기존엔 이렇고, 현재에는 이렇다.

## jdk9

- immutable List, Map, Set, Map.Entry 생성

```java
List immutableList = List.of();
List immutableList = List.of(“one”, “two”, “thress”);
Map immutableMap = Map.of(1, "one", 2, "two");
```

- Map.Entry
  code

  ```java
  Map<String, Object> test = Map.of("key1", "value", "key2", "value2");
  System.out.println(test.entrySet().toString());
  System.out.println(test.entrySet().toString());
  for(Map.Entry<String, Object> t : test.entrySet()){
    System.out.println(t.toString());
  }
  ```

  output

  ```
  [key1=value, key2=value2]
  [key1=value, key2=value2]
  key1=value
  key2=value2
  ```

- Project Jigsaw: Module System Quick-Start Guide : java 개발환경을 모듈별로 관리할 수 있게 해준다.

- CompletableFuthre API 개선

  - CompletableFuture

  ```java
  ExecutorService executor = Executors.newSingleThreadExecutor();
  CompletableFuture
          .runAsync(()->{ 
            try{Thread.sleep(1000);} catch(Exception e){};
            System.out.println("Hello!");
            try{Thread.sleep(1000);} catch(Exception e){};
          },executor)
    .thenRun(()->System.out.println("World"));
  System.out.println("async request is ready.");
  ```

  ```java
  Executor executor = CompletableFuture.delayedExecutor(50L, TimeUnit.SECONDS);
  ```

- Reactive Stream API 추가

