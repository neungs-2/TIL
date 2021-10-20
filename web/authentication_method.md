# **인증 방식; Web Authentication Method**

<br>

### **인증** vs **인가** vs **Grant**

<br>

**_인증 (Authentication)_** : 유저의 Identification을 확인하는 절차
<br>

**_인가 (Authorization)_** : 유저의 요청 권한을 확인하는 절차
<br>

**_Grant_** : 사용자가 자신의 인증정보를 Application에 넘기는 것

---

<br>

## **서버(세션) vs 토큰 기반 인증**

기존의 인증방식은 세션 기반 인증 방식이었지만 시스템 규모가 커짐에 따라 한계점을 보이고 토큰 기반 인증 방식이 등장.

<br>

### **_서버(세션) 기반 인증_**

<img width="561" alt="Session" src="https://user-images.githubusercontent.com/60606025/138047297-0e15c1bb-b200-43ca-9ed9-00bdf86d1287.png"><br>
출처: https://tansfil.tistory.com/58
<br>

- 기존의 사용하던 인증 방식
- Stateful 서버(클라이언트의 상태를 계속 유지)를 통해 세션을 유지하여 사용자 정보를 저장하고 서비스 제공 시 사용
- 세션은 메모리나 디스크, 데이터베이스 등을 통해 관리
- 사용자 요청 시 세션 저장소를 거쳐서 응답
- 시스템 규모가 커짐에 따라 한계점을 보임

<br>

**[한계점]**

- **세션**
  - 사용자 인증 시 저장하는 정보
  - 대부분의 경우 메모리에 저장
  - 사용자가 늘어날 경우 서버의 RAM에 부하
  - DB에 저장 시 DB에 부하
- **확장성**
  - 사용자 늘어날 경우 더 많은 트래픽을 처리하기 위해 서버를 확장해야 함
  - 세션을 분산시키는 시스템을 설계해야 하지만 어렵고 복장한 과정임
- **CORS(Cross-Origin Resource Sharing)**
  - 세션 관리에 사용되는 쿠키는 여러 도메인에서 관리하기 번거로움

<br><br>

### **_토큰 기반 인증_**

<img width="608" alt="token" src="https://user-images.githubusercontent.com/60606025/138049322-4260df33-9bc3-43e4-b031-a2b47ebc7993.png"><br>
출처: https://tansfil.tistory.com/58
<br>

- 인증 받은 사용자에게 토큰을 발급하고 서버에게 요청 시 헤더에 토큰을 첨부하여 유효성 검사
  - 로그인 성공 시 Signed 토큰을 발급
- 사용자의 인증 정보를 서버나 세션에 유지하지 않고(Stateless) 클라이언트 측에서 들어오는 요청만으로 처리
- 메모리나 디스크, 데이터베이스에 부하를 주지 않고 시스템을 확장하기 용이함

<br>

**[장점]**

- **Stateless(무상태성) & Scalability(확장성)**
  - 토큰은 클라이언트 측에 저장되어 서버는 완전히 Stateless
  - 클라이언트와 서버의 연결고리가 없으므로 확장하기에 적합
    - 서버를 분산처리 할 시 세션 방식에서는 처음 로그인한 서버에만 요청을 받도록 설정
    - 토큰 사용 시 어떤 서버로 요청이 와도 처리 가능
- **확장성(Extensibility)**
  - 시스템의 확장성을 의미하는 Scalability와 달리 Extensibility는 로그인 정보가 사용되는 분야의 확장을 의미
  - 토큰에 선택적 권한만 부여하여 발급 가능
  - OAuth 경우 Facebook, Google과 같은 소셜 계정을 이용하여 다른 웹서비스에서 로그인 가능
- **CORS 해결**
  - 서버 기반 인증 시스템 문제점인 CORS 해결 가능
  - 토큰 사용 시 서로 다른 디바이스, 도메인에서도 토큰 유효성 검사 가능
  - 따라서 assets 파일은 CDN에서 제공하고 서버 측에서는 API만 다루도록 설계 가능

<br>

**[단점]**

- **제한적인 Payload**
  - Payload는 암호화되지 않기 때문에 디코딩 시 누구나 정보 확인 가능
  - 따라서 유저의 중요 정보를 담을 수 없음
- **Token 탈취**
  - 토큰이 탈취 당하면 유효기간 만료까지 계속 사용 가능
  - 쿠키가 악의적으로 이용시 세션을 지우면 되지만 토큰은 유효기간 만료까지 계속 사용 가능

**_Token 탈취 해결책_**

- 기존 Access Token의 유효기간을 짧게 하고 Refresh Token을 발급
- Refresh Token 만료 시 재로그인

---

<br>

## **API Key 방식**

- 특정 사용자만 알 수 있는 문자열인 API key를 사용
  - 개발자는 API key를 발급 받음
  - API 사용 시 API key를 메시지에 넣어서 요청
  - 서버는 메시지 안의 API를 이용하여 유저 인증 및 권한 확인
- Key 유출 시 대비하기 힘들기 때문에 주기적으로 Key 업데이트 필요

---

<br>

## **OAuth 방식**

- API key의 단점을 보완할 수 있는 방식
- API key가 요청하는 서버와 받는 서버의 단순한 구성이라면 OAuth2는 기존 2개의 서버에 인증하는 서버로 세분화
  - 사용자가 Application 사용을 위한 요청 시 인증되어 있지 않다면 인증서버로 Redirection
  - 사용자에게 Grant Accept을 받음
  - 인증과 인가가 완료되면 인증서버는 해당 인가코드를 저장소에 저장 (코드는 짧은 유효기간 존재)
  - Application이 해당 코드를 Request Token으로 사용하여 인증서버에 요청 보냄
  - 인증서버는 인가코드와 일치하는지 확인 후 Access Token을 전달
  - Application은 Access Token을 통해 리소스 서버에 요청
  - 리소스 서버는 다시 인증서버에 토큰 유효성을 검증한 후 요청한 정보를 응답

---

<br>

## **JWT 방식**

- JSON Web Token 의 줄임말로 인증 흐름의 규약이 아닌 Token 작성에 대한 규약
- 기존 Token 방식의 번거로움을 줄여줌
  - 기존의 Token은 의미가 없는 문자열이므로 Token 유효성을 매번 확인해야 함
  - JWT는 인증여부 확인을 위한 값, 유효성 검증을 위한 값, 인증정보 자체를 담고 있어서 인증서버에 묻지 않고 사용 가능

---

[참고]<br>
https://www.sauru.so/blog/basic-of-oauth2-and-jwt/<br>
https://bcho.tistory.com/955
