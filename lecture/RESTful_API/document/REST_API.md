# REST API

> REST API의 목적은 Client, Server 독립적으로 발전하기 위함이다.

Server와 Client가 서로 결합도가 높으면, Server가 개선될때, Client가 개선될때마다 서로 맞춰 주어야한다. 

그런 불편한점을 개선하고자 서로 약속을 해놓은게 REST API라고 할 수 있다.



## REST의 특징

### REST 아키텍처 스타일 ([발표 영상 ](https://www.youtube.com/watch?v=RP_f5dMoHFc)11분)

- Client-ServerStatelessCache

- **Uniform Interface**

  :pushpin: 각 Client와 관계 없이 통일된 URI를 갖는다.

- Layered SystemCode-On-Demand (optional)



### Uniform Interface

- Identification of resources

- manipulation of resources through represenations

- **self-descrive messages**

  :round_pushpin: 응답은 스스로 자신을 설명할 수 있어야 한다.

  ​	IANA에 등록된 Content-Type이거나, Field에 대해 설명된 링크를 응답에 포함시켜야 한다.

- **hypermisa as the engine of appliaction state (HATEOAS)**

  :round_pushpin: 응답에 대한 다음 상태에 대해서도 설명할 수 있어야 한다.

  ​	만약 게시글 조회 상태에서 진행될 수 있는 다음 상태는 게시글 상세조회, 게시글 생성/삭제인데 이에 대한 링크를 응답에 포함시켜야 한다.







## 참고

https://meetup.toast.com/posts/92

https://midnightcow.tistory.com/102