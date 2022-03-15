# **Introduction to MongoDB**

## **MongoDB 특징**

- Document 지향 데이터베이스
- 비 관계형 데이터베이스
- 동적 스키마
- 확장 가능한 설계

<br>

**_Document-Oriented Storage_**

- 도큐먼트는 JSON과 같은 특정 형식의 태그 구조
- mongoDB가 데이터를 저장하는 최소 단위
- 도큐먼트는 필드와 값의 쌍으로 구성
- 관계를 갖는 데이터를 중첩 도큐먼트와 배열을 사용하여 1개의 도큐먼트로 표현 가능
- 데이터 입출력 시에는 JSON 형식의 도큐먼트 사용
- DB 저장 시에는 이진 포맷으로 인코딩한 BSON 형식의 도큐먼트로 변환하여 저장

<br>

**_Non-Relational Database_**

- mongoDB는 관계(Relationship) 개념이 없는 비 관계형 데이터베이스
- Join을 지원하지 않고 대신 임베디드 방식의 도큐먼트 구조를 사용하거나 레퍼런스 방식의 도큐먼트 구조를 사용한 후 애플리케이션에서 조인

<br>

**_Schemaless_**

- 스키마의 선언 없이 필드의 추가와 삭제가 자유로운 Schema-less 구조
- 컬렉션 내 모든 도큐먼트들의 필드 집합이 동일하지 않고 같은 필드라도 데이터 타입이 다를 수 있는 비정형 스키마
- 동적 스키마로 생성

<br>

**_Scalable Design_**

- Auto Sharding을 통해 수평적 확장이 용이
- 데이터를 여러 노드에 걸쳐 분산하는 것을 자동 관리

<br>

---

### **Tip**

<br>

**_JSON vs BSON_**
![image](https://user-images.githubusercontent.com/60606025/158432891-4d85d3a0-1918-4c6c-8c5d-44d6bde9afda.png)

<br>

> [참고]<br>https://meetup.toast.com/posts/275
