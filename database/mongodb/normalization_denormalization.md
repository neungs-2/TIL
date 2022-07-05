# **정규화 vs 비정규화**

## **정규화와 비정규화**

***정규화(Normalization)***
- 컬렉션 간의 참조를 이용해 **데이터를 여러 컬렉션으로 분할**하는 작업
- **쓰기 작업 성능 향상**
  - 데이터 변경 시 *하나의 컬렉션만 갱신*하면 됨
  - 각 데이터 조각은 하나의 컬렉션에 포함되기 때문

```js
> db.studentClasses.findOne({"studentId": studentId})
{
  "_id": ObjectId("523523c1d86...."),
  "studentId": ObjectId("5125123c1d86...."),
  "classes": [
    ObjectId("4325523c1d87...."),
    ObjectId("5225623c1d88...."),
    ObjectId("5127183c1d89...."),
    ObjectId("5133173c1d90....")
  ]
}

> db.studentClasses.findOne({"_id": id})
{
  "_id": ObjectId("523523c1d86...."),
  "name": "John Doe" 
  "classes": [
    ObjectId("4325523c1d87...."),
    ObjectId("5225623c1d88...."),
    ObjectId("5127183c1d89...."),
    ObjectId("5133173c1d90....")
  ]
}
```

<br>

***비정규화(De-Normalization)***
- 모든 데이터를 **하나의 도큐먼트에 내장**
- 도큐먼트가 데이터의 참조를 갖는 대신 *데이터의 사본을 가짐*
- 데이터 변경 시 *여러 도큐먼트가 갱신*
- **조회 작업 성능 향상**
  - 데이터 *조회 시 하나의 쿼리*만 사용

```js
// 각 과목을 내장 도큐먼트로 (한번에 모든 정보 조회)
> db.studentClasses.findOne({"_id": id})
{
  "_id": ObjectId("523523c1d86...."),
  "name": "John Doe" 
  "classes": [
    > db.studentClasses.findOne({"_id": id})
{
  "_id": ObjectId("523523c1d86...."),
  "name": "John Doe" 
  "classes": 
    {
      "class": "Trigonometry",
      "credits": 3,
      "room": 204
    },
    {
      "class": "Physics"
      "credits": 5,
      "room": 306 
    },
    {
      "class": "AP European History"
      "credits": 2,
      "room": 508
    }
  ]
}
```

```js
// 혼합된 확장 참조 패턴 
// 자주 조회되는 정보만 내장 도큐먼트에 포함
// 다른 데이터 필요 시 class 컬렉션 조회
> db.studentClasses.findOne({"_id": id})
{
  "_id": ObjectId("523523c1d86...."),
  "name": "John Doe" 
  "classes": [
    > db.studentClasses.findOne({"_id": id})
{
  "_id": ObjectId("523523c1d86...."),
  "name": "John Doe" 
  "classes": 
    {
      "_id": ObjectId("4325523c1d87...."),
      "class": "Trigonometry"
    },
    {
      "_id": ObjectId("5225623c1d88...."),
      "class": "Physics"
    },
    {
      "_id": ObjectId("5133173c1d90...."),
      "class": "AP European History"
    }
  ]
}
```

<br>

***정규화 vs 비정규화***
- **정규화**: 데이터가 정기적으로 혹은 빈번하게 갱신될 때 
- **비정규화**: 데이터 변경이 잦지 않고 조회 성능이 중요할 때
  - 많은 정보를 생성할 수록 더 적은 정보를 내장해야 함
    - 내장된 필드의 개수, 내용이 제한 없이 늘어나야 한다면 참조가 바람직
  - 내장 도큐먼트 갱신 시 모든 작업의 성공을 보장하기 위해 cron job을 설정해야 함
  - 모든 도큐먼트 갱신 전에 서버가 고장난다면 감지하고 재시도할 방법이 필요
  
- **멱등 연산**: 다수의 시도에도 매번 같은 결과가 나오는 연산
  - `$set` 연산자는 멱등이지만 `$inc`는 멱등을 보장하지 않음
  - 멱등 연산은 네트워크 오류 시 재시도만으로 충분
  - 멱등 연산이 아니면 작업을 쪼개야 함
    - 개별적으로 멱등이고 재시도해도 안전한 작업으로 분리

|**내장 방식이 추천되는 경우**|**참조 방식이 추천되는 경우**|
|:---:|:---:|
작은 서브 도큐먼트|큰 서브 도큐먼트
주기적으로 변하지 않는 데이터|자주 변하는 데이터
결과적인 일관성이 허용|즉각적인 일관성이 필요할 때
증가량이 적은 도큐먼트|증가량이 많은 도큐먼트
두 번째 쿼리를 수행하는 데 자주 필요한 데이터|결과에서 자주 제외되는 데이터
빠른 읽기|빠른 쓰기