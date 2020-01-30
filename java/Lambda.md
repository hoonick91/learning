# 함수형 프로그래밍 (lambda, stream)

왜?? 함수형 프로그래밍?

1~30까지 더하기

```java
int sum=0, i=0;
for(i=1;i<=30;i) {
	sum+=i
}
sout(sum);
```

함수형

```
sumAToB(1,30);
```

- 뛰어난 가독성
- 단위테스트 할때, 쉽게 할 수 있음
- 순수함수...



### 기타 - Thread

## @FunctionalInterface

```java
@FunctionalInterface
interface Func {
	public int calc(int a, int b);
}
```

```java
Func add = (int a, int b) -> a + b; //함수 정의
```

```java
add.calc(1,2); //3
```

위와 같은 방법으로 람다를 사용할 수 있다. 하지만 매번 이런 형식으로 람다를 정의해서 쓰면 귀찮다. 그래서 제공되는 interface가 있다.

- java.util.function패키지
  - Supplier<T>
  - Consumer<T>
  - Function<T, R>
  - Preficate<T>
  - BiConsumer<T,U>
  - BiPredicate<T,U>
  - BiFunction<T,U,R>

연습해봐야 한다

## 실전문제!!!

근데 function 패키지를 이용해서 람다를 자주 사용하지는 않는 것 같다. stream을 많이 쓴다.

## Stream

- 데이터의 흐름이다.
- 배열이나 collection에 함수를 조합해서 결과를 필터링하거나 조합할 수 있다.

## Lambda + steam()

- Map

  list의 엘리먼트 값을 모두 대문자로 변경하여 출력.

```java
final List<String> names = Arrays.asList("Sehoon", "Songwoo", "Chan", "Youngsuk", "Dajung");
            //java 7
            System.out.println("java 7");
            for(String name : names) {
                System.out.println(name.toUpperCase());
            }
 
            System.out.println("");
 
            //java 8 Lambda
            System.out.println("java 8");
            names.stream()
                .map(name -> name.toUpperCase())
                .forEach(name -> System.out.println(name));
```

- Filter

  'S' 로 시작하는 이름을 출력.

```java
final Listt<String> names = Arrays.asList("Sehoon", "Songwoo", "Chan", "Youngsuk", "Dajung");
 
        //java 7
        System.out.println("java 7");
        final List<string> startsWithN1 = new ArrayList<string>();
        for (String name : names) {
            if (name.startsWith("S")) {
                startsWithN1.add(name);
            }
        }
 
        System.out.println(startsWithN1);
 
        System.out.println("");
 
        //java 8 Lambda
        System.out.println("java 8");
        final List<string> startsWithN2 = 
                names.stream().filter(name -> name.startsWith("S"))
                                .collect(Collectors.toList());
 
        System.out.println(startsWithN2);
```

- reduce

  특정 스트링값의 길이보다 크고, 리스트의 가장 긴이름을 가진 엘리먼트를 출력.

```java
final List<String> names = Arrays.asList("Sehoon", "Songwoo", "Chan", "Youngsuk", "Dajung");
 
//java 7
String LongerEliment1  = "";
for (String name : names) {
    if(("hoone".length() <= name.length()) && (LongerEliment1.length() <= name.length())) {
        LongerEliment1 = name;
    }
}
 
System.out.println("java 7 "+LongerEliment1);
 
//java 8 Lambda
String LongerEliment2 = names.stream()
        .reduce("hoone", (name1, name2) ->
            name1.length() >= name2.length() ? name1 : name2);
System.out.println("java 8 "+LongerEliment2);
```

- collect()

  리스트의 엘리먼트를 콤마로 구분하여 출력. 단 마지막 엘리먼트에 콤마가 없어야한다.

```java
final List<String> names = Arrays.asList("Sehoon", "Songwoo", "Chan", "Youngsuk", "Dajung");
 
System.out.println("java 7");
//java 7
for(int i = 0; i < names.size() - 1; i++) {
    System.out.print(names.get(i).toUpperCase() + ", ");
}
 
if(names.size() > 0) {
    System.out.println(names.get(names.size() - 1).toUpperCase());
}
 
System.out.println("java 8");
//java 8 Lambda
System.out.println(names.stream()
            .map(String::toUpperCase)
            .collect(Collectors.joining(", ")));
```



함수형 프로그래밍은 함수를 일급객체로 취급하여 함수를 매개변수로 넣을 수 도 있고, return값에 함수를 줄 수 도 있다. 즉, 기존의 java처럼 정적인 객체에 대한 param, return값이 아닌 action이라는 함수를 줄 수 있다는 것이다. 

그래서 java에서는 lambda를 익명객체를 이용하여 함수형 프로그래밍을 구현할 수 있다. 객체를 익명으로 선언하여 함수를 그안에서 실행하는 것이다. 

```java
ArrayList<list> list = new ArrayAlist<>();
list.forEach(i->sout(i+""))
  
Map<String, String> map = new HashMap<>();
map.put("1","2");
map.forEach((k,v) -> sout(k,v));

```







## Optional + Lambda 조합

```java
Member member = memberRepository.findById(1L);
Coord coord = null;
if (member != null) {
  if (member.getAddress() != null) {
    String zipCode = member.getAddress().getZipCode();
    if (zipCode != null) {
      coord = coordRepository.findByZipCode(zipCode)
    }
  }
}

// TO-BE
Optional<Member> member = memberRepository.findById(1L);
Coord coord = member.map(Member::getAddress)
    .map(address -> address.getZipCode())
    .map(zipCode -> coordRepository.findByZipCode(zipCode))
    .orElse(null)
```



- 비어 있는 객체 생성

```java
Optional<Member> member = Optional.empty();
```

- Null 허용하지 않는 객체 생성

```java
Optional<Member> member = Optional.of(memberRepository.findById(1L));
member.get() // NullPointerException !!!
```

- 값이 존재할 때, 특정 메소드 호출

```java
// AS-IS
Member member = memberRepository.findById(1L);
if (member != null) {
  System.out.println(member);
}

// TO-BE
Optional<Member> member = Optional.ofNullable(memberRepository.findById(1L));
member.ifPresent(System.out::println);
```

- 값 존재할 경우와 아닌 경우를 삼항 연산자로 표현하지 않아도 됨

```java
// AS-IS
Member member = memberRepository.findById(1L);
System.out.println(member != null ? member : new Member("Unknown"));

// TO-BE
Optional<Member> member = Optional.ofNullable(memberRepository.findById(1L));
member.orElse(new Member("Unknown")).ifPresent(System.out::println);
```

- 특정 조건을 만족하는 경우에만 특정 행위를 하고 싶을 경우

```java
// AS-IS
Member member = memberRepository.findById(1L);
if (member != null && member.getRating() != null && member.getRating() >= 4.0) {
  System.out.println(member);
}

// TO-BE
Optional<Member> member = Optional.ofNullable(memberRepository.findById(1L));
member.filter(m -> m.getRating() >= 4.0)
    .ifPresent(m -> System.out::println)
```



## 메서드 참조

- 하는중...

- 스프링 부트에서는 test코드를 작성할때, lambda를 사용하더라

https://github.com/spring-projects/spring-boot/blob/543fcdbbfdc807dd4cc70934b076756d80e2d21c/spring-boot-project/spring-boot-autoconfigure/src/test/java/org/springframework/boot/autoconfigure/web/reactive/error/DefaultErrorWebExceptionHandlerIntegrationTests.java



