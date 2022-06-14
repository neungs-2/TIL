# MongoDB Schema Design Pattern

## **다형성 패턴**(polymorphic pattern)
- 컬렉션 내 모든 도큐먼트가 유사하지만 동일하지 않은 구조
- 단일 컬렉션에서 정보에 액세스(쿼리)하려는 경우에 유용
- 실행하려는 쿼리를 기반으로 문서를 그룹화하면(테이블이나 컬렉션 간에 개체를 분리하는 대신) 성능 개선

```js
{
  "sport": "ten_pin_bowling",
  "athlete_name": "Earl Anthony",
  "career_earnings": {value: NumberDecimal("1441061"), currency: "USD"},
  "300_games": 25,
  "career_titles": 43,
  "other_sports": "baseball"
}
{
  "sport": "tennis",
  "athlete_name": "Martina Navratilova",
  "career_earnings": {value: NumberDecimal("216226089"), currency: "USD"},
  "event": [
    {
      "type": "singles",
      "career_tournaments":390,
      "career_titles": 167
    },
    {
      "type": "doubles",
      "career_tournaments": 233,
      "career_titles": 177,
      "partner": ["Tomanova", "Fernandez", "Morozova", "Evert", ...]
    }
  ]
}
```

> https://www.mongodb.com/developer/how-to/polymorphic-pattern/
---

<br>

## **속성 패턴**(attribute pattern)
- 다음의 경우에 적합한 패턴
  - (공통 특성을 갖는) 정렬하거나 쿼리하려는 도큐먼트에 필드의 서브셋이 있는 경우
  - 정렬하려는 필드가 도큐먼트의 서브셋에만 존재하는 경우
- 데이터 하위 집합을 키-값 하위 문서로 이동
  - 비결정적 필드 이름 사용
  - 정보에 추가 한정자를 추가
  - 원래 필드와 값의 관계를 명확하게 설명 가능
- **필요한 익덱스의 수가 적어지므로 쿼리가 더 간단하고 빨라짐**

```js
// 속성 패턴 적용 전
// 검색을 위해 필요한 색인: {release_US: 1}, {release_France: 1}, {release_Italy: 1}, ...
{
  title: "Star Wars",
  director: "George Lucas",
  ...
  release_US: ISODate("1977-05-20"),
  release_France: ISODate("1977-05-20"),
  release_Italy: ISODate("1977-05-20"),
  release_UK: ISODate("1977-05-20"),
  ...
}

// 속성 패턴 적용 후
// 검색을 위해 필요한 색인: {"releases.location": 1, "releases.date": 1}
{
  title: "Star Wars",
  director: "George Lucas",
  ...
  releases: [
    {
      location: "USA", 
      date: ISODate("1977-05-20")
    },
    {
      location: "France", 
      date: ISODate("1977-05-20")
    },
    {
      location: "Italy", 
      date: ISODate("1977-05-20")
    },
    {
      location: "UK", 
      date: ISODate("1977-05-20")
    },
    ...
  ],
  ...
}
```

> https://www.mongodb.com/developer/how-to/attribute-pattern/
---

<br>

## **버킷 패턴**(bucket pattern)
- 일정 기간 동안 스트림으로 들어오는 데이터(시계열 데이터)에 적합
- 특정 시간 범위의 데이터를 각각 보유하는 도큐먼트 셋으로 **버킷화**하는 것이 시간/데이터 포인트의 포인트 당 도큐먼트 생성보다 효율적
  - 1시간 버킷 사용시 해당 시간 동안의 모든 판독 값을 단일 도큐먼트 내 배열에 배치
- 컬렉션 전체 문서 수를 줄이고 인덱스 성능 개선
- 사전 집계를 활용한 데이터 엑세스 단순화

```js
// 버킷 패턴 적용 전
{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:00:00.000Z"),
  temperature: 40
}

{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:01:00.000Z"),
  temperature: 40
}

{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:02:00.000Z"),
  temperature: 41
}

// 버킷 패턴 적용 후
{
  sensor_id: 12345,
  start_date: ISODate("2019-01-31T10:00:00.000Z"),
  end_date: ISODate("2019-01-31T10:59:59.000Z"),
  measurements: [
    {
      timestamp: ISODate("2019-01-31T10:00:00.000Z"),
      temperature: 40
    },
    {
      timestamp: ISODate("2019-01-31T10:01:00.000Z"),
      temperature: 40
    },
    ...
    {
      timestamp: ISODate("2019-01-31T10:42:00.000Z"),
      temperature: 42
    }
  ],
  transaction_count: 42,
  sum_temperature: 2413
}
```

> https://www.mongodb.com/developer/how-to/bucket-pattern/
---

<br>

## **이상치 패턴**(outlier pattern)
- 도큐먼트의 쿼리가 애플리케이션의 정상적인 패턴을 벗어날 때 사용
- *플래그(flag)*를 사용해 도큐먼트가 이상점임을 나타냄
  - 플래그는 애플리케이션 코드에서 오버플로 도큐먼트를 검색하기 위한 추가 쿼리를 만드는데 사용
- **추가 오버플로**를 하나 이상의 도큐먼트에 저장
  - 추가 오버플로: "_id"를 통해 첫 번째 도큐먼트를 다시 참조
- 인기도가 중요한 SNS에서 많이 사용

```js
{
  "_id": ObjectID("507f191e810c19729de860ea"),
  "title": "Harry Potter, the Next Chapter",
  "author": "J.K. Rowling",
  ...,
  "customers_purchased": ["user00", "user01", "user02", ..., "user999"],
  "has_extras": "true" // 플래그
}
```

> https://www.mongodb.com/developer/how-to/outlier-pattern/
---

<br>

## **계산된 패턴**(computed pattern)
- 반복적으로 계산이 필요한 데이터가 있거나 데이터 접근 패턴이 읽기 집약적일 때 사용
- 주요 도큐먼트가 주기적으로 갱신되는 백그라운드에서 계산을 수행하도록 함
- 동일한 계산이 반복되는 것을 방지
- CPU 워크로드를 줄이고 애플리케이션 성능 향상

> https://www.mongodb.com/developer/how-to/computed-pattern/
---

<br>

## **서브셋 패턴**(subset pattern)
- 장비에 RAM 용량을 초과하는 작업 셋이 있을 때 사용
- 자주 사용하는 데이터와 아닌 데이터를 두 개의 **별도 컬렉션에 분리**
  - 전자상거래에서 상위 10개 리뷰를 저장, 이외의 리뷰는 다른 컬렉션에 분리 후 필요 시에만 쿼리

> https://www.mongodb.com/developer/how-to/subset-pattern/
---

<br>

## **확장된 참조 패턴**(extended reference pattern)
- **확장된 참조 패턴**은 자주 엑세스하는 필드만 복사 (최소한의 중복만 허용)
- 고유한 컬렉션이 있는 여러 논리 엔티티(사물)가 있고 특정 기능을 위해 이 엔티티들을 모을 때 사용
- 반복적인 Join 작업이 많을 때 적합
- 엔티티를 모두 나누면 데이터 중복은 발생하지 않지만 Join이 많아져 성능 하락
- 개별 컬렉션에서 모든 정보를 포함 시 Join은 줄어들지만 중복정보가 발생하여 성능 하락 가능성 존재
- **확장된 참조 패턴**은 보다 적은 Join과 빠른 엑세스를 통해 성능 향상

```js
// 확장된 참조 패턴이 적용되기 전
// customer collection
{
  _id: 123,
  name: "Katrina Pope",
  street: "123 Main St",
  city: "Somewhere",
  country: "Someplace",
  date_of_birth: ISODate("1992-11-03"),
  social_handles: [
    twitter: "twitterID",
    facebook: "facebookID"
  ]
}

// order collection
{
  _id: ObjectId("507flf77bcf9b"),
  date: ISODate("2019-02-18"),
  customer_id: 123,
  order: [
    {
      product: "widget",
      qty: 5,
      cost: {
        value: NumberDecimal('11.99'),
        currency: "USD"
      }
    }
  ]
}

// 확장된 참조 패턴 적용 후
// customer collection의 일부 데이터를 포함한 order collection
{
  _id: ObjectId("507flf77bcf9b"),
  date: ISODate("2019-02-18"),
  customer_id: 123,
  shipping_address: {
    name: "Katrina Pope",
    street: "123 Main St",
    city: "Somewhere",
    country: "Someplace"
  }
  order: [
    {
      product: "widget",
      qty: 5,
      cost: {
        value: NumberDecimal('11.99'),
        currency: "USD"
      }
    }
  ]
}
```

>https://www.mongodb.com/blog/post/building-with-patterns-the-extended-reference-pattern
---

<br>

## **근사패턴**(approximation pattern)
- **리소스**(시간, 메모리, CPU)가 많이 드는 계산이 필요하지만 **정확할 필요가 없는** 경우 유용
  - ex) 추천수, 페이지 조회수 등 (99만 9583회 -> 100만회로 표시해도 상관 없음)
- 1회 변화 시 마다 카운터 갱신이 아닌 100회 변화시마다 카운터를 갱신하면 쓰기 횟수를 줄일 수 있음
---

<br>

## **트리 패턴**(tree pattern)
- **쿼리가 많고 계층적**인 데이터가 있을 때 적용
- 다중 조인을 피하여 **성능이 향상**될 수 있지만 **그래프 업데이트를 관리**해야 함
- 다양한 형태의 트리 구조 존재

<br>

***Model Tree Structures with Parent References***
- 각 *document*에 트리 노드와 부모의 키를 저장
```js
db.categories.insertMany( [
   { _id: "MongoDB", parent: "Databases" },
   { _id: "dbm", parent: "Databases" },
   { _id: "Databases", parent: "Programming" },
   { _id: "Languages", parent: "Programming" },
   { _id: "Programming", parent: "Books" },
   { _id: "Books", parent: null }
] )

db.categories.createIndex( { parent: 1 } ) // 빠른 검색을 위한 인덱스 지정
db.categories.findOne( { _id: "MongoDB" } ).parent // 노드의 부모 검색
db.categories.find( { parent: "Databases" } ) // 부모의 직계 자식 노드 검색
```

<br>

***Model Tree Structures with Child References***
- 각 *document*에 트리 노드와 자식의 키를 저장
- **하위 참조 패턴**은 노드가 여러 부모를 가질 수 있는 그래프를 저장하는 데 적합
```js
db.categories.insertMany( [
   { _id: "MongoDB", children: [] },
   { _id: "dbm", children: [] },
   { _id: "Databases", children: [ "MongoDB", "dbm" ] },
   { _id: "Languages", children: [] },
   { _id: "Programming", children: [ "Databases", "Languages" ] },
   { _id: "Books", children: [ "Programming" ] }
] )

db.categories.createIndex( { children: 1 } ) // 빠른 검색을 위한 인덱스 지정
db.categories.findOne( { _id: "Databases" } ).children // 노드의 직계 자녀 검색
db.categories.find( { children: "MongoDB" } ) // 상위, 형제 노드 검색
```

<br>

***Model Tree Structures with an Array of Ancestors***
- 부모 노드에 대한 키와 모든 조상(상위 노드)를 **배열**로 저장하는 구조
- `Array of Ancestors` 패턴은 `Materialized Paths` 패턴보다 약간 느리지만 사용하기 쉬움
```js
db.categories.insertMany( [
  { _id: "MongoDB", ancestors: [ "Books", "Programming", "Databases" ], parent: "Databases" },
  { _id: "dbm", ancestors: [ "Books", "Programming", "Databases" ], parent: "Databases" },
  { _id: "Databases", ancestors: [ "Books", "Programming" ], parent: "Programming" },
  { _id: "Languages", ancestors: [ "Books", "Programming" ], parent: "Programming" },
  { _id: "Programming", ancestors: [ "Books" ], parent: "Books" },
  { _id: "Books", ancestors: [ ], parent: null }
] )

db.categories.createIndex( { ancestors: 1 } ) // 인덱스 설정
db.categories.findOne( { _id: "MongoDB" } ).ancestors // 노드의 조상 배열 검색
db.categories.find( { ancestors: "Programming" } ) // 조상의 모든 하위 항목 검색
```

<br>

***Model Tree Structures with Materialized Paths***
- 조상 노드의 키를 **문자열**로 저장
- 문자열과 정규식을 사용하는 단계가 추가되지만 경로 작업이 더 유연해짐(부분 경로로 노드를 찾기)
```js
db.categories.insertMany( [
   { _id: "Books", path: null },
   { _id: "Programming", path: ",Books," },
   { _id: "Databases", path: ",Books,Programming," },
   { _id: "Languages", path: ",Books,Programming," },
   { _id: "MongoDB", path: ",Books,Programming,Databases," },
   { _id: "dbm", path: ",Books,Programming,Databases," }
] )

db.categories.find().sort( { path: 1 } ) // path 필드를 정렬하여 전체 트리 검색
db.categories.find( { path: /,Programming,/ } ) // 정규식을 이용한 특정 노드의 하위 항목 검색
db.categories.find( { path: /^,Books,/ } ) // 최상위 수준(Book)의 하위 항목 검색
db.categories.createIndex( { path: 1 } ) // 인덱스 생성
```
- 위의 인덱스의 경우,
  - **root가 포함**된 쿼리 (e.g. /^,Books, ... ,/)의 경우 쿼리 **성능이 크게 증가**
  - **root가 포함되지 않은** 하위 트리 쿼리(e.g. /,Databases/)의 경우 **전체 인덱스**를 검사하므로 인덱스가 전체 컬렉션보다 훨씬 작은 경우 어느정도 성능이 향상

<br>

***Model Tree Structures with Nested Sets***
- 트리를 왕복 순회하여 멈춤으로 노드를 식별
- *application* 각 노드를 두번씩 방문 (initial stop, return stop)
- 트리 노드에 부모의 키, 초기 멈춤(initial stop), 반환 멈춤(return stop)을 저장
- `Nested Sets` 패턴은 **빠른 하위 트리 검색**이 가능하지만 **트리 구조 수정이 비효율적**
  - 변경되지 않는 **정적 트리에 적합**

![image](https://user-images.githubusercontent.com/60606025/173533548-76849cf7-9afe-496e-90a9-76d48156ae1e.png)
```js
db.categories.insertMany( [
   { _id: "Books", parent: 0, left: 1, right: 12 },
   { _id: "Programming", parent: "Books", left: 2, right: 11 },
   { _id: "Languages", parent: "Programming", left: 3, right: 4 },
   { _id: "Databases", parent: "Programming", left: 5, right: 10 },
   { _id: "MongoDB", parent: "Databases", left: 6, right: 7 },
   { _id: "dbm", parent: "Databases", left: 8, right: 9 }
] )

// 노드의 하위 항목 검색
var databaseCategory = db.categories.findOne( { _id: "Databases" } );
db.categories.find( { left: { $gt: databaseCategory.left }, right: { $lt: databaseCategory.right } } );
```

<br>

> https://www.mongodb.com/blog/post/building-with-patterns-the-tree-pattern<br>https://www.mongodb.com/docs/manual/applications/data-models-tree-structures/
---
