# RESTful API와 TDD

> Client, Server가 독립적으로 발전하기 위해 서로 정의한 약속?? 즉, Client, Server간의 결합도를 낮춘다.

:question: 결합도가 가장 높은 것은 먼저 URI이다. Client에 Server API URI가 고정되어 있으면 Server에서 URI를 바꾸지 못한다. 그래서 Server에서 URI를 알려줄 필요가 있다.(HATEOAS)



## RESTful API

### Uniform interface

- identification of resources
- manipulation of resources through representation
- self-descriptive messages
- hypermedia as the engine of application state(HATEOAS)

RESTful API에서는 요청/응답에 대한 정보를 header에 표시한다.(Content-Type 등)





## TDD 시퀀스

1. 시스템에 필요한 기능을 Test class의 메소드로 정의한다.

   ```java
   @Test
   void 시간을_입력받아_통계_그래프에_사용되는_카테고리_목록_조회() throws Exception {
   }
   
   @Test
   void 조건을_입력받아_통계_그래프_데이터_조회(){
   }
   ```

2. Test code를 작성하면서 필요한 클래스, 메소드를 선언한다.

3. RESTful API를 만족하기 위해 ```Uniform interface```를 고려한다.





## 예시: 이벤트 Respository

1. Event를 엔티티로 만든다.
2. EventRepository interface를 생성
   - Extends 
3. EventContrller에서 EventRepository의존성 추가 후 save(event) 메서드 호출
4. @MockBean, Mockito를 사용하여 EventRepository를 Bean으로 등록
5. header()에 Location과 Content-type이 존재하는지 확인



??@MockBean

```
@WebMvcTest는 Test시 Web관련 Bean들만 등록해준다. 그래서 @MockBean을 통해 Bean으로 등록할 수 있는데, 이러면 Controller에서의 EventRepository는 null로 반환된다. 그래서 Mockito를 이용하여...?
```





