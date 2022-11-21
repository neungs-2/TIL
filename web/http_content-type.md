# **HTTP: Content-Type**

## **Content-Type 이란?**

- **_MIME_** 타입으로 변환된 문서의 종류를 알리는 항목
- Body에 담긴 클라이언트
- 웹 리소스의 **확장자** 역할
- HTTP 헤더에 위치
  > **MIME (Multipurpose Internet Mail Extensions)**<br>HTTP 프로토콜을 이용한 웹에서 서버가 클라이언트에게 보내준 문서의 종류를 정의한 것

---

<br>
<br>

## **Content-Type 종류**

|      타입       |                                          설명                                           |                                                                                                       종류                                                                                                        |
| :-------------: | :-------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    **text**     |                               텍스트를 포함하는 모든 문서                               |                                                                              text/plain<br>text/html<br>text/css<br>text/javascript                                                                               |
|    **image**    |  모든 종류의 이미지<br>애니메이션되는 이미지는 포함(gif 등)<br>비디오는 포함하지 않음   |                                                                          image/gif<br>image/png<br>image/jpeg<br>image/bmp<br>image?webp                                                                          |
|    **audio**    |                                 모든 종류의 오디오 파일                                 |                                                                         audio/midi<br>audio/mpeg<br>audio/webm<br>audio/ogg<br>audio/wav                                                                          |
|    **video**    |                                 모든 종류의 비디오 파일                                 |                                                                                              vidio/webm<br>video/ogg                                                                                              |
| **application** |                          모든 종류의 **이진 데이터**를 나타냄                           | **_application/json_**<br>**_application/x-www-form-urlencode_**<br>application/octet-stream<br>application/pkcs12<br>application/vnd.mspowerpoint<br>application/xhtml+xml<br>application/xml<br>application/pdf |
|  **multipart**  | 다른 MIME 타입들을 지닌 개별적인 파트들로 나누어지는 문서의 카테고리 => **합성된 문서** |                                                           **_multipart/formed-data_**<br>multipart/mixed<br>multipart서alternative<br>multipart/related                                                           |

---

<br>
<br>

## _application/json_ vs _application/x-www-form-urlencoded_

- **application/json**
  - JSON 형태의 데이터
  - `{key: value}`
- **application/x-www-form-urlencoded**
  - 쿼리 스트링 형태의 데이터
  - `key=value&key=value`
  - 개인적으로 해당 타입에 json 데이터를 넣어서 값이 전달 안되는 버그 종종 있었음!
