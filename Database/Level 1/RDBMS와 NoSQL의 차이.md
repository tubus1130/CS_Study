## 📓 키워드

- RDBMS(관계형데이터베이스)
- NoSQL 데이터베이스

---

## ✏️ 관계형데이터베이스 & NoSQL데이터베이스

![img.png](../img/DB%20RANKING.png)

- RDBMS가 더 많이 쓰이는 것을 알 수 있다

### 💭 RDBMS와 NoSQL데이터베이스 비교

| 기능 / 특징 |         RDBMS          |           NoSQL 데이터베이스            |
|:-------:|:----------------------:|:---------------------------------:|
|   데이터   |        테이블(관계형)        |        다양한 모델(키-값, 그래프 등)         |
|   스키마   |        엄격하게 정의됨        |          유연하고 동적으로 변경가능           |
|  쿼리 언어  |          SQL           |  JSON, API, Cypher(Neo4j) 등 다양함   |
|  트랜잭션   |           지원           |                지원                 |
| 격리성(기본) | repeatable_read(mysql) | local := read_uncommited(mongodb) |
| 인덱스 구조  |         B-Tree         |               LSM트리               |