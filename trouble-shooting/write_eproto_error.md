# Error: Write EPROTO

## **문제**

- API Test tool을 이용해 Localhost에 요청을 보낼 시 에러 발생
- **Error Message**<br>write EPROTO 140427579402112:error:1408F10B:SSL routines:ssl3_get_record:wrong version number:../deps/openssl/openssl/ssl/record/ssl3_record.c:332:

<br>

## **해결 방법**

- localhost에서는 https 요청을 보낼 수 없으므로 http 프로토콜을 사용
