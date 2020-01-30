jdk8에서 9로 업그레이드 하는데, 뭐가 좋아졌는지 알아보려고 한다.



1. Immutable

   #### Java 8

   기존의 Immutable List를 만들려면 ```unmodifiableList()```  를 사용했다. 

   ```java
   List<String> fruits = new ArrayList<>();
   
   fruits.add("Apple");
   fruits.add("Banana");
   fruits.add("Cherry");
   fruits = Collections.unmodifiableList(fruits);
   
   fruits.add("Lemon"); // UnsupportedOperationException
   ```

   또는, Arrays를 사용하기도 했다. 하지만 List를 만드는데 Arrays를 사용하는것이 직관적이지 않다.

   ```java
   List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");
   ```

   Guava를 사용하기도 했다.

   ```java
   import com.google.common.collect.ImmutableList;
   List<String> fruits = ImmutableList.of("Apple", "Banana", "Cherry");
   fruits.add("Lemon"); // UnsupportedOperationException
   ```

   

   #### Java 9

   ```java
   List<String> fruits = List.of("Apple", "Banana", "Cherry"); // [Apple, Banana, Cherry]
   fruits.add("Lemon"); // UnsupportedOperationException
   ```

   깔끔하다.

   

> final vs unmodifiableList()
>
> - 변하지 않는 속성을 적용하기 위해 final도 사용한다. ```unmodifiableList()``` 와는 어떤 차이점이 있을까?
>
>   ```java
>   public static void main(String args[]){
>        String str1 = "hello";
>        String str2 = "world";
>        String str3 = "jaeyoung";
>        final String test[] = { str1, str2, str3 };
>        test[2] = "rangken";
>        // 변경이 가능하다. test array 가 final 이다
>        // test = anotherarr; 는 불가능하다
>        // final List<String> 도 마찬가지
>   }
>   ```
>
>   위 사례에서 
>   test[]의 참조값은 바꿀수 없지만, 내부 데이터```test[2]``` 는 변경이 가능하다.







#### 참조

https://www.daleseo.com/java9-immutable-collections/
http://rangken.github.io/blog/2014/unmodify_array/