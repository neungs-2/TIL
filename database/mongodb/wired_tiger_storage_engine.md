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

## **WiredTiger 엔진 내부 동작 방식**

- _B-Tree_ 구조의 데이터 파일과 서버 충돌 시 데이터 복구를 위한 *저널로그*를 가짐
- 새로운 로그 파일 생성과 사용하지 않은 파일 삭제하는 방식
  <br>(*Oracle*의 `Redo`는 파일 내에서 로테이션 --> *MongoDB*와 다른 방식)

<br>

- **쿼리 실행 시**
  - 블록 매니저를 통해 필요한 데이터 블록을 디스크에서 읽어 공유 캐시에 적재

<br>

- **도큐먼트 변경 시**
  - *WiredTiger 스토리지 엔진*이 트랜잭션을 시작하고 커서를 이용하여 도큐먼트 내용 변경
  - 변경 내용은 공유 캐시에 먼저 적용
  - 변경 내용을 디스크에 저장하는 것을 기다리지 않고 저널 로그에 기록한 후 처리 결과를 리턴 (`WriteConcern` 옵션에 따라 다를 수 있음)
  - 공유 캐시가 쌓이면 **체크포인트**를 발생시켜서 캐시의 **더티 페이지**를 모아 디스크에 기록함

<br>

- **공유 캐시에 더티 페이지를 읽을 공간이 없을 시**

  - `Eviction` 모듈이 오래된 페이지를 제거
  - 제거해야하는 데이터 페이지가 더티 페이지이면 디스크에 기록 후 공유캐시에서 제거

- **WiredTiger의 스토리지 엔진의 데이터 블록**
  - 상한선은 있지만 크기가 **가변적**
  - 크기가 가변적이므로 **압축 기능**이 자주 사용됨
    - 고정 크기라면 8KB의 페이지를 압축 시 데이터 내용에 따라 가변적인데 그 압축데이터를 다시 고정된 크기 블록에 넣는 것이 압축 효율을 떨어트림
    - 크기가 가변적이면 페이지를 압축한 사이즈대로 저장하여 압축효율 높음
  - 블록 매니저가 변경된 데이터 블록 기록 시 *fragmentation*을 최소화하며 블록 크기에 최적의 위치를 찾아서 저장하는 역할 수행

<br>

---

## **공유 캐시**

- *WiredTiger*는 내부에서 캐시를 사용
- 공유 캐시의 최적화는 MongoDB 처리 성능에 중요
- MongoDB가 사용할 **캐시 사이즈**
  - 50% of (RAM - 1 GB)
  - 256 MB (하한선)
- 공유 캐시는 MongoDB 서버를 재시작하지 않고도 크기 조정 가능
- *WiredTiger*는 디스크에서 데이터 페이지를 캐시로 공유 시 메모리에 적합한 트리 형태로 재구성
  - 별도의 매핑 없이 메모리 주소를 이용해 바로 검색이 가능
  - 맵핑 테이블의 경합이나 오버헤드가 없음
- *WiredTirger*는 레코드 인덱스를 별도로 관리하지 않고 페이지를 공유 캐시 메모리에 적재 시 레코드 인덱스 새롭게 생성
  - 따라서 디스크 -> 메모리 과정은 RDBMS보다 느리게 동작하지만 캐시에 적재 후 레코드 검색 및 변경 작업은 RDBMS보다 빠르게 동작

<br>

---

## **운영체제 캐시** (페이지 캐시)

- *WiredTiger*는 **Cached IO**를 기본 옵션으로 사용
  - 참조하려는 데이터 페이지는 리눅스 페이지 캐시에 있는 데이터
  - 리눅스 페이지 캐시의 데이터를 자신의 공유 캐시에 복사
- RDBMS에서는 더블 버퍼링을 해결하기 위해 **Direct IO** 주로 사용
  - 더블 버퍼링: OS의 페이지 캐시와 스토리지 엔진에 같은 데이터가 중복되는 것
- **Cached IO**는 가끔 페이지 캐시 처리에서 문제가 발생할 수 있지만 **Direct IO 보다 속도가 빠름**

<br>

> **[참고]** <br> https://www.mongodb.com/docs/manual/core/storage-engines/ <br> https://dev4u.tistory.com/811 <br> https://rastalion.me/mongodb%EC%9D%98-wiredtiger-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%97%94%EC%A7%84/
