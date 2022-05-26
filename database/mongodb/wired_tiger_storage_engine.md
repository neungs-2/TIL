# WiredTiger Storage-Engine

- MongoDB 3.2 버전 이상부터 디폴트로 사용되는 엔진
  - **File Base** 기반의 엔진으로 **Big Data**의 빠른 쓰기와 읽기 중심 데이터에 적합
  - 서버 장애 발생 시 **빠른 복구**가 보장
  - Point in time Recovery가 가능한 **Dignostic** 기능을 제공
  - **Single CPU** 환경에서 구현 가능하며 충분한 시스템 메모리와 빠른 성능이 기대되는 **SSD**가 요구

<br>

---

## **Document Level Concurrency**

- *WiredTiger*는 **문서 수준 동시성 제어**를 사용하여 여러 클라이언트가 컬렉션의 서로 다른 문서를 동시에 수정 가능
- 대부분의 읽기/쓰기 작업에 대해 **Optimistic concurrency control**(낙관성 동시 제어)를 사용
- 스토리지 엔진이 두 작업 간의 쓰기 충돌을 감지하면 MongoDB가 해당 작업을 재시도
- 수명이 짧은 작업인 일부 전역 작업 시 **instance-wide lock** 필요
- `collMod` 같은 일부 다른 작업에서는 **exclusive database lock** 필요

<br>

---

## **저장 방식**

- WiredTiger 스토리지 엔진은 3가지 타입의 저장소를 가짐
- **레코드(도큐먼트) 스토어**
  - **_현재 몽고디비에서 3가지 중 유일하게 쓰이는 방식_**
  - 일반적인 RDBMS가 사용하는 저장 방식
  - 테이블의 레코드를 한번에 저장하는 방식
  - _B-Tree_ 알고리즘 사용
- **컬럼 스토어**
  - 대용량 분석(OLAP or DW) 용도로 주로 사용
  - 테이블의 레코드와 상관없이 각 컬럼 패밀리로 데이터 파일을 생성
    - 데이터 크기가 작고 속도가 빠르므로 대용량 분석에 적합
- **LSM(Log Structured Merge Tree) 스토어**
  - 데이터 읽기보다 쓰기에 집중한 저장 방식
  - *B-tree*가 아닌 순차 파일형태로 데이터를 저장
  - 메모리의 저장 가능 한계 초과 시 디스크에 저장
  - HBase, 카산드라 같은 NoSQL에서 자주 사용하는 방식

<br>

---

<br>

> **[참고]** <br> https://www.mongodb.com/docs/manual/core/storage-engines/ <br> https://dev4u.tistory.com/811 <br> https://rastalion.me/mongodb%EC%9D%98-wiredtiger-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%97%94%EC%A7%84/
