# **몽고 DB Document Count Method**

***db.collection.countDocuments( < query >, < options > )***
- *query*: 선택 기준, 빈 문서 지정 시 모든 문서 계산
- *options*: 추가 옵션
  - `limit`: integer 타입 & Optional, 계산할 최대 문서 수
  - `skip`: integer 타입 & Optional, 계산 전 건너뛸 문서 수
  - `hint`: string or document 타입 & Optional, 쿼리에 사용할 인덱스 이름 또는 사양
  - `maxTimeMs`: 카운트를 실행할 수 있는 최대 시간
- 매커니즘 TODO: 여기부터 ㄲ

<br>

***db.collection.estimatedDocumentCount(< options >)***
- *options*의 field 
  - **maxTimeMS**: integer 타입, count하는 최대 시간
- 컬렉션 또는 보기에 있는 모든 문서의 수를 반환
- 쿼리 필터가 아닌 **메타데이터**를 사용하여 컬렉션 개수를 반환
- 메커니즘
  - 메타 데이터가 없음
  - 문서 수는 *View Definition*에서 집계 파이프라인을 실행하여 계산
  - 빠른 예상 문서 수 없음
  - 샤딩된 클러스터에서 결과 개수는 분리된 문서를 올바르게 필터링 하지 않음
- 드리프트의 양은 **체크포인트와 비정상 종료 사이에 수행된 상비, 업데이트 또는 삭제 작업의 수에 따라 다름**
  - 체크 포인트는 일반적으로 60초마다 발생

> https://www.mongodb.com/docs/manual/reference/method/db.collection.countDocuments/#std-label-countDocuments-restrictions <br>
> https://www.mongodb.com/docs/manual/reference/method/db.collection.estimatedDocumentCount/
