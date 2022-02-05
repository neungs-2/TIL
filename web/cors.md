# **Cross-Origin Resource Sharing; CORS**

## **CORS 란?**

![image](https://user-images.githubusercontent.com/60606025/152636103-f6ba7c1f-d4e6-4266-979d-cda769d36320.png)

- 교차 출처 리소스 공유
- 웹 애플리케이션이 **다른 출처의 자원에 접근**할 수 있는 권한을 부여할 수 있게 하는 체제
- **웹 애플리케이션이 리소스가 자신의 출처와 다를 때** 교차 출처 HTTP 요청 실행
  - 출처--> **도메인 + 프로토콜 + 포트**
- HTTP 헤더를 사용
- CORS Error는 Front/Back **Server가 아닌 브라우저에서 터지는 에러**

---

<br>

## **Same Origin Policy; SOP**

- 다른 출처의 리소스를 사용하는 것을 제한하는 보안 방식
- 보안을 위협하는 문서를 격리하여, 보안 위협으로부터 보호
  - 링크를 통해 해커의 웹 사이트로 들어가서 script를 실행하는 등

---

<br>

## **CORS 접근제어 시나리오**

### **프리플라이트 요청** (Preflight Request)

- `OPTIONS 메서드`를 통해 다른 도메인의 리소스에 요청이 가능한 지 확인 작업
  - 사전 요청 작업
- 요청이 가능하다면 실제 요청을 보냄
- 요청 허용 시 응답 헤더에 `HTTPHeader("Access-Control-Allow-Methods")` 표시

<br>

- Preflight는 CORS를 인식하지 못하는 서버들을 보호할 수 있는 매커니즘
- CORS spec 이전에 SOP만 허용하던 서버들은 Cross-Site Request에 대한 보안 매커니즘 부재
- Preflight request로 서버가 CORS를 인식하고 핸들링할 수 있는지 먼저 확인

<br>

**_Preflight 구성_**

- **Request 메시지**

  - Origin: 요청 출처
  - Access-Control-Request-Method: 실제 요청의 메서드
  - Access-Control-Request-Headers: 실제 요청의 추가 헤더

- **Response 메시지**
  - Access-Control-Allow-Origin: 서버 측 허가 출처
    - 응답에 이게 없을 경우 `CORS Error` 발생!!
  - Access-Control-Allow-Method: 서버 측 허가 메서드
  - Access-Control-Allow-Header: 서버 측 허가 헤더
  - Access-Control-Max-Age: 응답 캐시 기간
    - 트래픽 낭비를 막기 위해 응답 캐시 기간 동안은 Preflight 요청 없이 바로 본 요청 전송

<br>

**_예시_**

```sh
# 요청 예시
OPTIONS /resource/foo
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: origin, x-requested-with
Origin: https://foo.bar.org

# 응답 예시
HTTP/1.1 204 No Content
Connection: keep-alive
Access-Control-Allow-Origin: https://foo.bar.org
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Max-Age: 86400
```

<br>

### **단순요청** (Simple Request)

- Preflight 요청 없이 바로 본 요청을 보냄
- 다음 중 하나를 만족해야 함
  - `GET`, `POST`, `HEAD` 메서드만 허용
  - Content-Type 는 다음 타입만 허용
    - A. `application/x-www-form urlencoded`
    - B. `multipart/form-data`
    - C. `text/plain`
  - 헤더는 `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` 만 허용

<br>

### **인증정보 포함 요청** (Credentialed Request)

- 인증 관련 헤더를 포함할 때 사용하는 요청
- Client 측
  - `credentials : include`
- Server 측
  - `Access-Control-Allow-Credentials : true`
    - 단, Access-Control-Allow-Origin : \* 은 불가

---

<br>

## **CORS 해결 방법**

1. 프론트 프록시 서버 설정 (개발 환경)
2. 직접 헤더에 설정
3. 스프링 부트, express 미들웨어 등 사용

<br>

- **Front Proxy Server** 설정 <br>![image](https://user-images.githubusercontent.com/60606025/152639814-983c6942-6e3f-4f1c-af8d-7c7ea9759381.png)
  - 외부 도메인 서버에 접근 시 바로 외부 도메인 서버를 통하는 것이 아닌 자신의 서버를 매개로 외부서버에 요청하는 방식
  - CORS는 브라우저 규약이므로 서버에서 요청 시 영향 받지 않음

<br>

- **직접 헤더 설정** <br> ![image](https://user-images.githubusercontent.com/60606025/152639930-dcd4e834-08a8-43c6-93bd-bab978c7043b.png)
  - 서버 측에서 `Access-Control-Allow-Origin` 헤더에 접근권한 설정
  - 직접 `Access-Control-Allow-Origin: https://testA.com` 이런식으로 헤더 적용

```js
var app = require('express')();
var cors = require('cors');

// Access-Control-Allow-Origin 적용방법: 직접 헤더에 적용
app.all('/*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'X-Requested-With');
  next();
});

app.listen(80, function () {
  console.log('http server is listening on port 80');
});
```

<br>

- **프레임워크에서 미들웨어 적용**
  - `Express`에서 `cors()` 사용시 모든 origin 허용

```js
var app = require('express')();
var cors = require('cors');

// Access-Control-Allow-Origin 적용방법: cors 미들웨어 사용
// 단, cors() 사용 시 모든 origin 허용
app.use(cors());

app.listen(80, function () {
  console.log('http server is listening on port 80');
});
```

<br>

**참고**

> Mozilla 공식 문서: https://developer.mozilla.org/ko/docs/Web/HTTP/CORS <br>
> 우테코 Youtube: https://www.youtube.com/watch?v=-2TgkKYmJt4&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=55&t=748s

---

## Tip

**_Proxy Server_**
<br>

- **프록시**

  - 서버와 클라이언트 사이의 중계기로서 대리로 통신을 수행하는 것

- **프록시 서버**
  - 서버, 클라이언트 사이의 중계기 역할을 하는 것
  - 클라이언트가 자신을 통해 다른 네트워크에 간접적으로 접속할 수 있게 접속할 수 있게 하는 컴퓨터 시스템, 응용 프로그램
