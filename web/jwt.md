# **Json Web Token (JWT)**

JSON 객체를 사용하여 가볍고 자가수용적인 방식으로 정보를 안전성 있게 전달하기 위한 토큰

- 웹표준(RFC 7519)으로서 대부분의 프로그래밍 언어에서 지원
- 자가수용적 : 필요한 모든 정보를 자체적으로 지님

---

<br>

## **구성요소**

- '.'을 구분자로 헤더, 내용, 서명 3가지로 구성

<br>

### **_헤더(Header)_**

- typ과 alg 두가지 정보로 구성
  - **typ**: 토큰의 타입 (="JWT")
  - **alg**: 해싱 알고리즘 지정 (Signature를 해싱하기 위한 알고리즘)

<br>

### **_정보(Payload)_**

- 토큰에서 사용할 정보의 조각들인 Claim을 담음 (JSON - key:value 쌍)
  - **Registered claim**: 토큰 정보 표현을 위해 이름이 이미 정해진 Claim으로 선택적으로 사용 가능
    - _iss_: 토큰 발급자 (issuer)
    - _sub_: 토큰 제목 (subject)
    - _aud_: 토큰 대상자 (audience)
    - _exp_: 토큰 만료시간 (expiration)
    - _nbf_: 토큰 활성 날짜 (not before)
    - _iat_: 토큰 발급 시간 (issued at)
    - _jti_: 토큰 식별자 (JWT ID)
  - **Public claim**: 공개용 정보를 위한 Claim으로 충돌 방지를 위해 URI 포맷을 이용
  - **Private claim**: 서버와 클라이언트 사이 임의로 지정한 정보를 저장

<br>

### **_서명(Signature)_**

- 토큰을 인코딩하거나 유효성 검증을 위해 사용하는 고유한 암호화 코드
- 헤더와 페이로드 값을 BASE64Url로 인코딩
- 인코딩 값을 비밀키를 이용하여 정해진 알고리즘으로 해싱
- 해싱한 값을 다시 BASE64Url로 인코딩하여 생성

---

<br>

## **JWT 고려사항**

- **Self-contained**

  - 토큰 자체에 정보를 담고 있음 (양날의 검)

  <br>

- **토큰 길이**
  - 토큰의 Payload에 3종류의 클레임을 저장하므로 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하

<br>

- **Payload 인코딩**
  - Payload 자체는 암호화 된 것이 아니라, BASE64로 인코딩 된 것
  - Payload를 탈취하여 디코딩하면 데이터를 볼 수 있음
  - JWE로 암호화하거나 Payload에 중요 데이터를 넣지 않아야 함

<br>

- **Stateless**
  - JWT는 상태를 저장하지 않기 때문에 한번 만들어지면 제어가 불가능
  - 토큰을 임의로 삭제하는 것이 불가능하므로 토큰 만료 시간이 필수적

<br>

- **Tore Token**
  - 토큰은 클라이언트 측에서 관리해야 하므로 토큰을 저장해야 함

---

<br>

[참고] <br>
https://mangkyu.tistory.com/56 <br>
https://github.com/gyoogle/tech-interview-for-developer/blob/master/Web/JWT(JSON%20Web%20Token).md
