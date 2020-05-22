## 오늘의 강의

### DB Index

- Hadoop Ecosystem - 대용량 파일 처리 시스템
- 관계형 DB는 분산이 되지 않는다. 무조건 마스터는 한 대 밖에 안된다. 백업을 위한 복제본은 만들 수 있지만, 메인 DB를 확장할 수 없다. (Oracle은 되지만 엄청 비쌈) 그래서 Redis 를 썼다. 인메모리 DB이기 때문에 아주 빠르다.
- 영구 데이터는 Hbase(하둡기반의 DB - 수평확장이 가능)를 사용하고, 빠른 데이터는 인메모리 DB인 Redis를 쓴다.
- DB Ranking 확인 - 주말에 튜토리얼 한번씩 해보는 것 권고. 관계형 DB, Redis, MongoDB 
- 토이 프로젝트에서 MongoDB를 쓰면 좋다. Document하나에 어플리케이션 하나 이다. (야구 플젝에서 MongoDB를 썼으면 좋았을 것이다.) 하나의 테이블에 모두 넣어두고 쓰면 된다.
- MongoDB는 인메모리 DB라서 속도가 매우 빠르다.  인메모리지만 백그라운드에서 디스크에 저장하여 영속성을 가진다.
- Redis 단점 - 실제 데이터의 2배의 메모리가 필요하다. (fork 떠야 하므로)
- 관계형 DB에서는 트래픽이 많아지면 처리 속도가 느려진다. 그래서 하둡 기반의 DB를(Redis와 같은..) 사용한다.
- Mongo DB : 메모리 DB이기 때문에 성능이 엄청 좋다. 백그라운드에서 디스크에도 저장한다.



### 실습

- MySQL Buik insert (대용량 데이터를 DB에 입력할 때 사용)

  ```
  LOAD DATA LOCAL INFILE '~/MOCK_DATA_1.csv' INTO TABLE user FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n';
  ```

- 검색 성능을 높이기 위해 인덱스를 사용 할 수 있다. (정렬의 경우 속도가 엄청 증가한다.)

- 인덱스를 만든다고 무조건 빨라지는 것은 아니다. 이유는?

- 데이터의 분포에 따라 오히려 느려질 수 있다. (성별과 같이 50% 확률 경우)

### 검색하기

* 250만건

* 전체검색 ⇒ `3.08 sec`

- 각각의 필드에 대해 지정검색 (name) ⇒ `0.67`, 범위검색 ( 300 <= money <= 600 ) ⇒ `0.70 sec`, count(*) ⇒ `0.39` 등을 실행해 보고 성능을 비교해 보자. 

- 특정 필드에 대해 정렬을 수행하고 성능을 비교해 보자.

  -  order by name desc ⇒ `6.22`
  -  order by name asc ⇒ `8.06`

  

### 인덱스 생성 및 성능 비교

- 각각의 필드에 대해 인덱스를 생성하고 검색 성능을 비교해 보자.

  - name필드에 인덱스 생성 (7.56 sec)

  ```mysql
  CREATE INDEX <인덱스명> ON <테이블명> ( 칼럼명1, 칼럼명2, ... );
  CREATE INDEX idx_name ON user ( name );
  ```

  * 인덱스 보기 

  ```mysql
  SHOW INDEX FROM <테이블명>;
  ```

   

- 인덱스 생성 후 정렬 성능은 어떻게 변하는가?

  -  order by name desc ⇒ `0.96`
  -  order by name asc ⇒ `0.76`

- 모든 필드들의 검색 성능은 동일한가?

  - (name) ⇒ `0.77`, 범위검색 ( 300 <= money <= 600 ) ⇒ `0.79 sec`, count(*) ⇒ `0.31` 

- 인덱스를 사용하면 무조건 성능이 개선되는가?

- 오름차순 / 내림차순 정렬의 성능은 동일한가? ⇒ 비슷해 보이는데 데이터가 많아지면 차이가 있을 것 같다. 2초면 큰 차이인 것 같기도

- Selectivity란 무엇일까?
