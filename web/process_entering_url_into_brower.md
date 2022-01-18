# 브라우저에 URL 입력 후 과정

URL 입력부터 랜더링 이전까지의 과정

<br>

## 1) **브라우저의 URL 해석/파싱**

- 가장 먼저 URL을 파싱하여 해석하는 과정 거침
  - **Scheme(Protocol)**: 어떤 프로토콜을 사용할 것인지
  - **Host(Domain)**: 어떤 도메인 주소로 서버에 요청을 보낼 것인지
  - **Port**: 어떤 포트 번호로 요청할 것인지
    <br>
    _Tip_
- 포트 번호 생략 시
  - http 사용: 80 포트
  - https 사용: 443 포트

---

<br>

## 2) **HSTS 목록 조회**

- **HSTS**(HTTP Strict Transport Security)
  - HTTP를 허용하지 않고 `HTTPS 프로토콜을 통한 연결만 허용`하는 기능
  - HTTP 접속을 HTTPS로 redirect 시 HTTP 연결을 거치므로 보안상 위험 --> HSTS 사용
- URL이 HSTS List에 존재 시 HTTPS만 요청 가능
- 만약 HTTPS만 가능한데 HTTP 요청 보낸 경우
  - 응답 헤더에 "Strict Transport Security" 필드를 포함하여 응답
  - 브라우저는 이를 확인하고 HTTPS만을 이용하여 통신 + HSTS 캐시에 URL 저장

---

<br>

## 3) **DNS 서버에서 IP 주소 받기** (Domain --> IP Addreass)

- Domain은 IP주소를 사람이 사용하기 편리하게 변환 시킨 것
- DNS(Domain Name Server)에서 Domain에 해당하는 IP 주소로 변경
- 브라우저 캐시 or 로컬 host 파일에 해당 URL 존재하면 바로 접속 없다면 `DNS Lookup`

**_DNS Lookup 과정_**

1. _Browser_ --요청--> _Local DNS_ <br>`Local DNS에 존재 시 바로 응답`
2. _Local DNS_ --요청--> _Root DNS_ <br>`하위 DNS 서버 IP 주소 응답`
3. _Local DNS_ --요청--> _com DNS_ <br>`하위 DNS 서버 IP 주소 응답`
4. _Local DNS_ --요청--> _해당 Domain의 DNS_ <br> `해당 도메인의 IP 주소 응답`
5. *Local DNS*에 해당 IP주소를 캐싱하고 *Brower*에게 `IP주소 응답`

---

<br>

## 4) **라우터로 네트워크 최적 경로 찾기**

- 라우터를 통해 해당 서버의 게이트웨이를 찾아 원하는 서버의 네트워크로 이동

---

<br>

## 5) **IP 주소를 MAC 주소로 변환**

**_IP 주소와 MAC 주소 차이점_**

- **IP 주소**
  - 접속하려는 서버의 네트워크를 찾기 위한 주소
- **MAC 주소**
  - 네트워크 내부의 컴퓨터와 통신하기 위한 주소

<br>

- IP 서버가 있는 곳이 로컬 네트워크가 아닌 경우 그 지역 라우터까지 패킷 전달
- 라우터에서 IP 주소에 해당하는 컴퓨터를 찾기 위해 MAC 주소가 필요

<br>

- `ARP(address resolution protocol)`
  - 논리 주소인 IP주소를 물리주소인 MAC으로 변환하는 역할
  - 송신측은 ARP 요청 패킷을 브로드캐스트 방식으로 전달
    - 브로드캐스트 방식으로 전달하는 이유는 최종 목적지의 물리주소를 모르기 때문에 모두에게 요청
  - 모든 Host와 Router는 송신자가 보낸 ARP 요청을 수락
  - 해당되는 수신자만 자신의 IP주소와 MAC 주소를 넣어 응답

---

<br>

## 6) **TCP 소켓 통신**

- TCP 소켓 연결
  - `3-way-handshake` 과정 거침
  - HTTPS라면 `TLS handshake`도 추가

---

<br>

## 7) **HTTPS(HTTP) 프로토콜 요청/응답**

- TCP 소켓을 통한 연결 완료 이후 페이지를 달라고 서버에 요청

---

<br>

## 8) **브라우저에서 응답 해석**

- 서버에서 보낸 응답 (HTML, CSS, JS)을 브라우저에서 해석하여 화면을 보여줌

<br>
<br>
<br>

> **[참고]**<br>https://1-7171771.tistory.com/134
