# MongoDB Query

- 쿼리는 컬렉션에서 도큐먼트읭 서브셋을 반환

<br>

## **Read Operations**

- `find()` / `findOne()`함수 사용
  - `db.collection.find(query criteria, projection)`

<br>

- _Query Criteria_
  - `find()`의 첫 매개변수(query criteria)로 가져올 데이터 필터링
  - 빈 쿼리 도큐먼트 `{}`는 컬렉션 내 모든 것과 일치
  - `{}`는 생략 가능
  - 쿼리 도큐먼트에 복수의 키:값 쌍 지정 시 **AND** 조건으로 인식

<br>

- _Projection_
  - `find()`의 두번째 매개변수(projection)로 반환을 원하는 키 지정
  - 필요한 키/값 쌍만 반환 받을 수 있음
  - 네트워크 데이터 전송량, 도큐먼트 디코딩에 드는 시간/메모리 감소
    <br>
  ```sh
  db.users.find(              # collection
    { age: { $gt: 18 } },     # query criteria --> filter
    { name: 1, address: 1 }   # projection
  ).limit(5)                  # cursor modifier
  ```

<br>

- _제약 사항_

  - 쿼리 도큐먼트 값은 반드시 **상수**
  - 즉, 도큐먼트 내 **다른 키의 값을 참조 할 수 없음**
    <br>

  ```sh
  # 재고 도큐먼트
  # @Param: in_stock --> 재고 수량 키
  # @Param: num_sold --> 판매 수량 키

  db.stock.find({'in_stock': 'this.num_sold'}) #작동하지 않음

  # 상품 구매 시 in_stock 값을 감소시켜서 품절 상품을 확인
  db.stock.find({'in_stock': 0})
  ```

---

<br>

## **쿼리 조건**

- _쿼리 조건절_
  - `$lt`: less than (<)
  - `$lte`: less than equal (<=)
  - `$gt`: greater than (>)
  - `$gte`: greater than equal (>=)
  - `$ne`: not equal (!=)
  - ex) `db.users.findj({'age' : {'$gte': 18, '$lte': 30}})`

<br>

- _OR 쿼리_
  - `$in`: 하나의 키를 다양한 값과 비교
  - `$or`: 여러 키를 주어진 값과 비교
  - ex) `db.raffle.find({'ticket_no' : {'$in' : [725, 542, 390]}})`
  - ex) `db.raffle.find({'$or' : [{'ticket_no' : 725}, {'winner' : true}]})`

<br>

- _$not_
  - 메타 조건절로 어떤 조건에서도 사용가능
  - 정규 표현식 등 주어진 패턴과 일치하지 않는 도큐먼트 조회 시 유용
    <br>
    ```sh
      # id를 5로 나눌 시 나머지가 1이 아닌 사용자를 반환
      db.users.find({'id_num' : {'$not' : {'$mod' : [5, 1]}}})
    ```

---

<br>

## **형 특정 쿼리**

<br>

## **$where 쿼리**

<br>

## **커서**

<br>

> [참고] > <br> https://docs.mongodb.com/manual/reference/method/db.collection.find/#mongodb-method-db.collection.find > <br> MongoDB 완벽 가이드 3판
