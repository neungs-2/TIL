# MongoDB Query

- 쿼리는 컬렉션에서 도큐먼트읭 서브셋을 반환

<br>

## **Read Operations**

- `find()` / `findOne()`함수 사용
  - `db.collection.find(query criteria, projection)`

<br>

**_Query Criteria_**

- `find()`의 첫 매개변수(query criteria)로 가져올 데이터 필터링
- 빈 쿼리 도큐먼트 `{}`는 컬렉션 내 모든 것과 일치
- `{}`는 생략 가능
- 쿼리 도큐먼트에 복수의 키:값 쌍 지정 시 **AND** 조건으로 인식

<br>

**_Projection_**

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

**_제약 사항_**

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

**_쿼리 조건절_**

- `$lt`: less than (<)
- `$lte`: less than equal (<=)
- `$gt`: greater than (>)
- `$gte`: greater than equal (>=)
- `$ne`: not equal (!=)
- ex) `db.users.findj({'age' : {'$gte': 18, '$lte': 30}})`

<br>

**_OR 쿼리_**

- `$in`: 하나의 키를 다양한 값과 비교
- `$or`: 여러 키를 주어진 값과 비교
- ex) `db.raffle.find({'ticket_no' : {'$in' : [725, 542, 390]}})`
- ex) `db.raffle.find({'$or' : [{'ticket_no' : 725}, {'winner' : true}]})`

<br>

**_$not_**

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

- 일부 데이터 형은 쿼리 시 형에 특정하게 작동 (type-specific)

<br>

**_null_**

- null은 스스로와 일치하는 것을 찾음
- **값이 null인 키**만 찾는 법
  - 키가 null인 값을 쿼리
  - `$exist` 조건절을 사용하여 null 존재 여부를 확인

```sh
# 아래의 쿼리는 z키가 null인 document만 찾을 것 같지만 z키가 없는(null) document도 반환
> db.c.find({'z' : null})
{'_id' : ObjectId('4ba0f0df....621'), 'y' : null }
{'_id' : ObjectId('4ba0f0df....622'), 'y' : null }

# 따라서 $exist 조건절을 사용해야 함
> db.c.find({'z' : {'$eq' : null, '$exists' : true}})
# z키가 null인 document 출력
```

<br>

**_정규 표현식_**

- `$regex`는 정규식 기능을 제공
- MongoDB는 정규 표현식 일치에 **펄 호환 정규표현식(PCRE)** 라이브러리 사용
  - 쿼리 전에 JS shell로 정규식 작동 확인 후 실행
- 대소문자 관계 없이 찾기
  - `db.users.find({ 'name' : { '#regex' : /joe/i }})`

<br>

**_배열에 쿼리하기_**

- 배열 요소 쿼리는 스칼라 쿼리와 같은 방식으로 동작
- 과일 목록이 `{'fruit' : ['apple', 'banana', 'peach']}` 일 때
  - 바나나 찾기 `db.food.find({'fruit' : 'banana'})`
  - 키:배열 쌍을 다음처럼 생각하면 편함 `{'fruit' : 'apple', 'fruit' : 'banana', 'fruit' : 'peach'}
- 배열 내 특정 요소를 쿼리하려면 key.index 구문 사용
  - `db.food.find({'fruit.2' : 'peach'})`

<br>

- `$all` 연산자
  - 2개 이상의 배열 요소가 일치하는 배열을 찾기 위해 `$all` 연산자 사용
  - 배열 내 여러 요소와 일치하는지 확인
  - 요소의 순서와 관계 없이 존재하는지만 판단
    <br>
    ```sh
      > db.food.find({'fruit' : {'$all' : ['apple', 'banana']}})
      {'_id' : ObjectId('4ba0f0df....521'), 'fruit' : ['apple', 'banana', 'peach']}
      {'_id' : ObjectId('4ba0f0df....522'), 'fruit' : ['grape', 'banana', 'apple'] }
    ```

<br>

**_$size 연산자_**

- 특정 크기의 배열을 쿼리하는 조건절
  - `db.food.find({'fruit' : {'$size' : 3}})`
- `$gt` 등 다른 조건절과 결합할 수 없지만 도큐먼트에 **size** 키를 추가하여 범위로 쿼리 가능
  - `db.food.update(criteria, '$push' : {'fruit' : 'strawberry', '$inc' : {'size' : 1}})`
  - `db.food.find('size' : {'$gt' : 3})`

<br>

**_$slice_**

- `find`의 두번째 매개변수에는 반환받을 특정 키를 지정
- `$slice` 연산자를 사용하여 배열 요소의 부분집합을 반환 받을 수 있음
- 반환 받을 키를 명시하지 않으면 모든 키를 반환

```sh
# 블로그에서 먼저 달린 댓글 10개 반환 받기
> db.blog.posts.findOne(criteria, {'comments' : {'$slice' : 10}})

# 나중에 달린 댓글 10개 반환 받기
> db.blog.posts.findOne(criteria, {'comments' : {'$slice' : -10}})

# 원하는 범위 안에 있는 결과 반환 받기
# 처음 23개 요소 건너 뛰고 24번째부터 (idx 시작은 0 이므로) 10개 (33번째 까지) 반환
> db.blog.posts.findOne(criteria, {'comments' : {'$slice' : [23, 10]}})

# 일치하는 배열요소를 쉽게 지정하는 방법 --> $
# find지만 첫번째로 일치하는 댓글만 반환 함
> db.blog.posts.find({'comments.name' : 'bob'}, {'comments.$' : 1})
```

<br>

**_배열 및 범위 쿼리의 상호작용_**

- 도큐먼트 내 스칼라(비 배열 요소)는 쿼리 기준의 각 절과 일치해야 함
  - {'x' : {'$gt' : 10, '$lt' : 20}} 이면 AND 조건처럼 작용하여 x는 10~20 사이 값
- 도큐먼트 내 배열 요소는 각 절의 조건을 충족하는 도큐먼트가 일치됨
  - OR 조건처럼 작용
- 배열 요소를 모든 조건을 충족시키게 하려면 `$elemMatch` 연산자를 사용
  - 단, `$elemMatch`는 비배열 요소를 일치시키지 않음

```sh
{'x' : 5}
{'x' : 15}
{'x' : 25}
{'x' : [5, 25]}

# [5, 25]는 10~20 범위는 아니지만 25는 10보다 크고 5는 20보다 작으므로 반환됨
> db.test.find({'x' : {'$gt': 10, '$lt': 20}})
{'x' : 15}
{'x' : [5, 25]}

> db.test.find({'x' : {'$elemMatch' : {'$gt': 10, '$lt': 20}}})
# 결과 없음 --> $elemMatch는 비배열 요소를 일치시키지 않으므로

# 조건을 스칼라와 배열 요소 모두 적용하기 위한 방법 --> min/max 함수를 적용
# 주의!! min, max 함수 사용시 hint에 인덱스로 사용할 요소 꼭 제시
> db.arrTest.find({'x': {'$gt':10, '$lt': 20}}).min({'x': 10}).max({'x' : 20}).hint({'x' : 1})
{'x': 15}

```

<br>

### **내장 도큐먼트에 쿼리하기**

- 내장 도큐먼트 쿼리는 도큐먼트 전체를 대상으로 하는 방식과 키/값 쌍 각각을 대상으로 하는 방식으로 나뉨

<br>

**_전체 도큐먼트를 대상으로 하는 쿼리_**

- 서브 도큐먼트 전체에 쿼리하려면 query filter(criteria)가 **서브 도큐먼트와 정확히 일치** 해야함
- 서브 도큐먼트에 없는 필드를 추가하거나 키 순서가 바뀌면 찾지 못함
- 따라서 서브 도큐먼트에 쿼리할 때는 **특정 키로 쿼리하는 방법 추천**

```sh
{
  'name' : {
    'first' : 'Joe',
    'last' : 'Schmoe'
  },
  'age' : 45
}

# 올바른 값 출력
> db.people.find({'name' : {'first': 'Joe', 'last': 'Schmoe'}})
# 위의 name 키에 해당하는 서브 도큐먼트 반환

# 올바르지 않은 값
> db.people.find({'name' : {'first': 'Joe'}})
> db.people.find({'name' : {'last': 'Schmoe', 'first': 'Joe' }})
# 서브 도큐먼트를 반환하지 못함

# 특정 키로 쿼리
> db.people.find({'name.first' : 'Joe', 'name.last' : 'Schmoe'})
> db.people.find({'name.first' : 'Joe'})
> db.people.find({'name.last' : 'Schmoe', 'name.first' : 'Joe'})
# 모두 올바른 서브 도큐먼트를 반환
```

---

<br>

## **$where 쿼리**

- `$where` 절을 사용하면 자바스크림트를 쿼리의 일부분으로 실행 가능
- 일반 쿼리보다 실행 느림, 인덱스도 사용할 수 없음
  - 사용 시 쿼리 필터로 다른 조건을 한번 거른 후 세부 조정에만 사용
- SQL Injection 등의 보안상 이유로 `$where` 사용을 제한해야 함!
- 정말 어쩔 수 없을 때만 최후에 사용
- 집계 표현식은 `$expr`를 사용하는 것을 권장

<br>

> [참고] > <br> https://docs.mongodb.com/manual/reference/method/db.collection.find/#mongodb-method-db.collection.find > <br> MongoDB 완벽 가이드 3판
