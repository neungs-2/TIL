# **REST API**

## **REST (REpresentational State Transfer)**

- World Wide Web 같은 **분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처 형식**
- Web에 존재하는 모든 **자원(이미지, 동영상, DB 자원)에 고유한 URI**를 부여해 활용하는 것
- **HTTP URI를 통해 자원을 명시**하고 **HTTP Method를 통해 해당 자원에 대한 CRUD Operation**을 적용
- REST의 함식을 따른 시스템: **RESTful**
- 설계 가이드일 뿐 명확한 표준은 없음

<br>

**_REST 구성요소_**

- **자원(Resource), URI**
  - HTTP에서 자원을 구별하는 ID는 URI임
- **행위(Verb), Method**
  - 클라이언트는 URI를 이용해 자원을 지정하고 조작하기 위해 HTTP Method를 사용
- **표현(Representation)**
  - 서버가 응답으로 보내주는 자원의 상태를 Representation이라고 함
  - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 표현 가능

<br>

**_REST의 특징_**

- 클라이언트 서버 구조
- 무상태 (Stateless)
- 캐시 처리 가능
- 계층화
- 인터페이스 일관성
- 자체 표현 구조

<br>

**_HTTP Method_**

- GET
  - 특정 리소스의 표시를 요청
  - 오직 데이터를 받기만 함
- POST
  - 특정 리소스에 엔티티를 제출할 때 쓰임
- PUT
  - 목적 리소스의 모든 현재 Representation을 request payload로 대체
- PATCH
  - 리소스의 부분적 수정 시 사용
- DELETE
  - 특정 리소스 삭제
- HEAD
  - GET과 동일하게 응답을 요청하지만 응답 본문을 포함하지 않음
- CONNECT
  - 요청한 리소스에 대해 양방향 연결을 시작하는 메소드
  - 터널을 열기 위해 사용
- OPTIONS
  - 목적 리소스의 통신을 설정하는데 사용
- TRACE
  - 목적 리소스의 경로를 따라 메시지 loop-back 테스트를 함

<br>

> https://www.youtube.com/watch?v=Nxi8Ur89Akw<br> > https://hckcksrl.medium.com/rest%EB%9E%80-c602c3324196<br> > https://developer.mozilla.org/ko/docs/Web/HTTP/Methods
