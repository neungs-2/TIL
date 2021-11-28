# HTTP Status Code

- 10X: 정보 확인
- 20X: 통신 성공
- 30X: 리다이렉트
- 40X: 클라이언트 오류
- 50X: 서버 오류

<br>

## **자주 쓰이는 코드**

<br>

**_200번대_**
|상태코드|이름|의미|
|:---:|:---:|:---:|
|200|OK|요청성공(GET)|
|201|Create|생성 성공(POST)|
|202|Accepted|요청 접수O, 리소스 처리X|
|204|No Contents|요청 성공O, 내용 없음|

<br>

**_300번대_**
|상태코드|이름|의미|
|:---:|:---:|:---:|
|300|Multiple Choice|요청 URL에 여러 리소스가 존재|
|301|Move Permanently|요청 URI가 새 위치로 옮겨감|
|304|Not Modified|요청 URI의 내용이 수정되지 않음|

<br>

**_400번대_**
|상태코드|이름|의미|
|:---:|:---:|:---:|
|400|Bad Request|API에서 정의되지 않은 요청 들어옴|
|401|Unauthorized|인증 오류|
|403|Forbidden|권한 밖의 접근 시도|
|404|Not Found |요청 URI에 대한 리소스 존재X|
|405|Method Not Allowed|API에서 정의되지 않은 메소드 호출|
|406|Not Acceptable|처리 불가|
|408|Request Timeout|요청 대기 시간 초과|
|409|Conflict|모순|
|429|Too Many Request|요청 횟수 상한 초과|

<br>

**_500번대_**
|상태코드|이름|의미|
|:---:|:---:|:---:|
|500|Internal Server Error|서버 내부 오류|
|502|Bad Gateway|게이트웨이 오류|
|503|Service Unavailable|서비스 이용 불가|
|504|Gateway Timeout|게이트웨이 시간 초과|

<br>

> **[참고]**<br> > https://developer.mozilla.org/ko/docs/Web/HTTP/Status<br> > https://gyoogle.dev/blog/web-knowledge/HTTP%20status%20code.html
