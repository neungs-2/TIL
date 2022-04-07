# **URL Convention**

## **REST API 설계 시 가장 중요한 2가지**
- URI는 정보의 자원을 표현해야 함
- 자원에 대한 행위는 HTTP Method로 표현

<br>

## **RESTful한 URL 설계**
- **소문자**를 사용
- 언더바(_) 대신 **하이픈(-)** 사용
- **슬래시**(/)는 계층 구분에만 사용하고 마지막에는 사용하지 않음
- 행위는 포함하지 않음
  - 행위는 URL 대신 Method를 사용하여 전달 (GET, POST, PUT, DELETE)
  <br>
  ```sh
  # Bad
  POST http://restapi.example.com/users/1/delete-post/1

  # Good
  DELETE http://restapi.example.com/users/1/posts/1
  ```
- 파일 확장자는 URI에 포함시키지 않음
  - 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자는 Accept header 사용
  <br>
  ```sh
  // Bad
  http://restapi.example.com/users/photo.jpg

  // Good
  GET http://restapi.example.com/users/photo
  HTTP/1.1 Host: restapi.example.com Accept: image/jpg
  ```
- 전달려는 자원의 명사를 사용하되 컨트롤 자원 의미 시 예외적으로 동사 허용
