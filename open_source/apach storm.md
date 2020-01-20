Apache storm

실시간 데이터를 처리하기 위한 분산형 스트림 프로세싱 연산 프레임워크

실시간으로 데이터를 처리하려면, 데이터가 생성되자마자 처리하는게 빠른데, 

스톰에서는 spout가 데이터(tuple)를 생성하고, Bolt에게 던지면 Bolt가 이를 처리하여 실시간 통계나 이벤트를 만들어 낼 수 있다.



### Spout

로그파일, 데이터 스트림, 큐 등으로부터 데이터를 읽는 Storm 클러스터 Elastic stack에서 filebeat와 같은 역할??



### Bolt

읽은 데이터를 처리하는 함수. 데이터 스트림을 입력받고, 로직에 따라 데이터를 가공한 다음 데이터 스트림으로 다른 Bolt에게 넘겨주거나 종료



### Topology

여러 개의 Spout와 Bolt간의 연관 관계를 정의해서 데이터 흐름을 정의하는 것을 말한다.





## vs Spark

분산된 여러 노드에서 연산을 처리할 수 있는 범용 분산 클러스터링 플랫폼





### 참조

https://c-o-e.tistory.com/37

https://sungjk.github.io/2016/01/05/Bigdata.html