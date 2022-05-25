# WiredTiger Storage-Engine

- MongoDB 3.2 버전 이상부터 디폴트로 사용되는 엔진
  - **File Base** 기반의 엔진으로 **Big Data**의 빠른 쓰기와 읽기 중심 데이터에 적합
  - 서버 장애 발생 시 **빠른 복구**가 보장
  - Point in time Recovery가 가능한 **Dignostic** 기능을 제공
  - **Single CPU** 환경에서 구현 가능하며 충분한 시스템 메모리와 빠른 성능이 기대되는 **SSD**가 요구

<br>

## **Document Level Concurrency**

> **[참고]** <br> https://www.mongodb.com/docs/manual/core/storage-engines/ <br> https://dev4u.tistory.com/811 <br> https://rastalion.me/mongodb%EC%9D%98-wiredtiger-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%97%94%EC%A7%84/
