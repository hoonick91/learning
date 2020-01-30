# Elasticsearch 성능 최적화

1. Heap size
2. shard 개수 늘리기



## Elasticsearch - 분산환경의 검색엔진

- Cluster

- Node : 물리 서버

  - Master : 노드 관리, 검색, 색인 작업에 대한 분배
  - Data : 검색, 색인
  - Client : Search에 대한 node 분산

- Indices : 루씬 기반 

  - Shard
    - Primary 
    - Replica node 수와 replica -1을 하면 검색속도가 빨라진다.

- Retrieve, Query

  - Retrieve : _id검색 그냥 바로 검색해서 준다.
  - Query : 어떤 shard에 있는지 모르니까 모든 shard에 data 검색을 요청하고, 결과값을 모두 받아 sorting이라던지.. 작업을 해서 리턴한다. (요청을 받은 노드가 마지막에 합치거나 해서 리턴한다.)

- Modeling

  - Indice/type design

  - Field design 

  - Shard design

    - shards Data_node의 수보다 크게
    - Replica의 수는 Data_node -1 보다 작거나 같게

  - Shard sizing

    - index당 최대 200개... 200개 이하로 구성
    - 하나 당 최대 크기 : 20~50GB (검색할때 속도에 영향을 주게된다. indexing에는 상관 없음)
    - 하나 당 최소 크기 : 3GB

    

## 성능 최적화

장비 관점

- network
- Disk i/o - ssd
- RAM 
- CPU cores : thread pool

운영체제 관점

- Increase File descriptor - 32k, 64k(file기반 index라 file open을 많이 한다.)
- Avoid swap

검색엔진 관점

- Thread pool 
- segment merge
- index buffer size
- Storage device - SSD
  - hdd는 **Segment merge 1개 만 할 수 있다.**
  - **Segment merge** 병렬처리 가능



### 색인 최적화

- _all
- _source 
- Indexing 방식을 type명에 e2eTransaction ID,  e2eTransactionId로 넣고, 그 안에 logs들을 넣는다.
- Thread pools
- Optimize-> Flush, Refresh



### 질의 최적화

- Shard를 늘리면 indexing은 빨라지지만 Search가 느려진다.

  

## Reference

- https://deview.kr/2014/session?seq=43