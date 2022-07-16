# **몽고 DB Document Count Method**

## ***db.collection.count(query, options)***
- ***query***: 선택 기준
- ***options***: 추가 옵션
  - `limit`: integer 타입 & Optional, 계산할 최대 문서 수
  - `skip`: integer 타입 & Optional, 계산 전 건너뛸 문서 수
  - `hint`: string or document 타입 & Optional, 쿼리에 사용할 인덱스 이름 또는 사양
  - `maxTimeMs`: 카운트를 실행할 수 있는 최대 시간
  - `readConcern`: string 타입, *read concern*을 지정
    - `local`: default 옵션
    - `majority`: 단일 스레드가 자체 쓰기를 읽게 함, *query*가 공백이면 안됨
  - `collation`: 작업에 사용할 데이터 정렬을 지정

- `find()` 쿼리와 매치되는 문서의 수 반환
  - `find()` 작업을 수행하지 않고 쿼리와 일치하는 결과 수를 계산하고 반환
- 쿼리 조건자가 없으면 컬렉션의 **메타데이터**를 기반으로 결과를 반환하여 **대략적인 개수**가 반환될 수 있음
  - ***쿼리 조건자 없이 사용 지양***

---

<br>

## ***db.collection.countDocuments( < query >, < options > )***
- ***query***: 선택 기준, 빈 문서 지정 시 모든 문서 계산
  - `$where` 사용 제한 --> 대신 `$expr`를 사용
  - `$near` 사용 제한 --> 대신 `$center`와 함께 `$geoWithin`을 사용
    ```js
      { $geoWithin: { $center: [ [ <x>, <y> ], <radius> ] } }
    ```
  - `$nearSphere` 사용 제한 --> 대신 `$centerSphere`와 함께 `$geoWithin`을 사용
    ```js
      { $geoWithin: { $centerSphere: [ [ <x>, <y> ], <radius> ] } }
    ```

<br>

- ***options***: 추가 옵션
  - `limit`: integer 타입 & Optional, 계산할 최대 문서 수
  - `skip`: integer 타입 & Optional, 계산 전 건너뛸 문서 수
  - `hint`: string or document 타입 & Optional, 쿼리에 사용할 인덱스 이름 또는 사양
  - `maxTimeMs`: 카운트를 실행할 수 있는 최대 시간

<br>

- **매커니즘**
  - `db.collection.count()`와 달리 메타데이터를 사용하지 않음.
  - 비정상적으로 종료된 후나 샤딩된 클러스터에 분리된 문서가 있는 경우에도 문서 집계를 수행하여 정확한 수를 반환
---  

<br>

## ***db.collection.estimatedDocumentCount(< options >)***
- ***options*의 field**
  - *maxTimeMS*: integer 타입, count하는 최대 시간

<br>

- 컬렉션 또는 보기에 있는 모든 문서의 수를 반환
- 쿼리 필터가 아닌 **메타데이터**를 사용하여 컬렉션 개수를 반환

<br>

- **메커니즘**
  - 메타 데이터가 없음
  - 문서 수는 *View Definition*에서 집계 파이프라인을 실행하여 계산
  - 빠른 예상 문서 수 없음
  - 샤딩된 클러스터에서 결과 개수는 분리된 문서를 올바르게 필터링 하지 않음

<br>

- 드리프트의 양은 **체크포인트와 비정상 종료 사이에 수행된 상비, 업데이트 또는 삭제 작업의 수에 따라 다름**
  - 체크 포인트는 일반적으로 60초마다 발생

> https://www.mongodb.com/docs/manual/reference/method/db.collection.count/ <br>
> https://www.mongodb.com/docs/manual/reference/method/db.collection.countDocuments/#std-label-countDocuments-restrictions <br>
> https://www.mongodb.com/docs/manual/reference/method/db.collection.estimatedDocumentCount/
