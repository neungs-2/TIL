# **Pagination and Infinite Scroll**

## **페이지네이션; Pagination**

- 콘텐츠를 웹사이트의 **다른 페이지들로 분리**하는 방법
- 사용자는 **페이지 하단의 숫자 링크**를 클릭하여 페이지 탐색
- 페이지네이션된 콘텐츠는 공통된 주제나 목적을 지님

---

<br>

## **무한 스크롤; Infinite Scroll**

- 사용자가 페이지 하단 도착 시 콘텐츠가 계속 로드되는 방식
- 끝이 없는 단일 페이지에서 정보의 흐름을 경험
- 버튼을 클릭하지 않고 스크롤 시 지연로딩

---

<br>

## **Pagination vs Infinite scroll**

**_사용 목적 및 콘텐츠 유형_**

- **Pagination**

  - 제품 검색 및 카탈로그 작성이 용이하여 전자상거래에서 많이 사용
  - 특정 유형의 컨텐츠를 보기 원하는 유저에게 적합

- **Infinite Scroll**
  - 특정한 컨텐츠를 원하는 것이 아닌 즐거움 혹은 무작위 정보 원할 때
  - 사용자를 관련 콘텐츠 스트림에 연결하기 좋은 방법
  - 뉴스나 SNS 등에서 많이 사용

<br>

**_Engagement_**

- **Pagination**

  - 콘텐츠를 빨리 얻을 수 있게 하는데 효과적
  - 버튼 클릭 후 새로운 페이지 로드 시간이 걸림
  - 개별 페이지를 늘려서 **로드 시간 최적화 필요**

- **Infinite Scroll**
  - 사용자 참여를 높이고 페이지에 오래 머물게하는데 효과적
  - 방문자에게 특정 목표가 없으면 효율적이고 이해하기 쉬우며 중단이 없는 방식
  - 스크롤을 많이 할수록 많은 컨텐츠를 로드해야해서 페이지 성능 저하
  - 따라서 이미지 크기를 필요한만큼까지 줄여야 함

---

<br>

## **Offset-based Pagination VS Cursor-based Pagination**

_위의 무한 스크롤, 페이지네이션과 동일한 개념은 아님_<br>
_Offset/Cursor Pagination 모두 페이지네이션, 무한 스크롤 구현 가능_

<br>

**_Offset-based Pagination_**

- **N개의 row를 skip하고 데이터를 로드**
- 데이터 요청하는 시점에 DB의 수정(추가, 제거) 발생할 경우 응답 데이터의 중복 혹은 생략 발생
  - **DB 데이터 추가의 경우**
    - ex) 기존 4페이지가 5페이지로 밀려나며 5페이지를 눌렀지만 유저는 4페이지와 동일한 페이지 조회
  - **DB 데이터 제거의 경우**
    - ex) 기존 5페이지가 4페이지로 당겨지며 5페이지를 눌렀지만 유저는 기존의 6페이지를 조회 --> 기존의 5페이지 데이터는 생략되게 됨

<br>

**_Cursor-based Pagination_**

- **기존 요청한 데이터(cursor) 이후의 N개 데이터를 로드**
- 데이터의 중복, 생략이 발생하지 않음
- 정렬 이후 cursor를 적용
  - 데이터 생략을 막기 위해 정렬 기준이 되는 필드 중 적어도 하나는 고유값이어야 함
  - ex) 고유값 필드가 가격이라면 `WHERE price > 14100` 조건에서 동일 가격 데이터가 생략될 수 있음

<br>

> https://blog.hubspot.com/website/pagination-vs-infinite-scroll<br>https://www.adityathebe.com/cursor-vs-offset-based-pagination<br>https://velog.io/@minsangk/%EC%BB%A4%EC%84%9C-%EA%B8%B0%EB%B0%98-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%84%A4%EC%9D%B4%EC%85%98-Cursor-based-Pagination-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0
