# String, StringBuilder의 차이점

```java
int total = 50000;
String s = ""; 
for (int i = 0; i < total; i++) { s += String.valueOf(i); } 
// 4828ms

StringBuilder sb = new StringBuilder(); 
for (int i = 0; i < total; i++) { sb.append(String.valueOf(i)); } 
// 4ms
```

```Java
String s = new StringBuilder(a).append(b).append(c).toString();
```

 