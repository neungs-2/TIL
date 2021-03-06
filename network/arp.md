# Address Resolution Protocol; ARP

<br>

## **ARP**

- 주소 결정 프로토콜
- 네트워크에서 IP 주소에 대응되는 MAC주소를 결정하는 규약

---

<br>

## **ARP 동작 순서**

단말 A가 C에게 데이터를 전송하려는데 IP주소는 알지만 MAC 주소를 모름

<br>

**_동일 네트워크에 존재 시_**

- 단말 A가 같은 네트워크 대역에 **ARP 요청**
  - 특정 IP 주소에 대응되는 MAC 주소를 묻는 메시지
  - 출발지는 A의 주소
  - 도착지는 브로드캐스트 주소
- 스위치가 ARP 요청을 받고 A의 MAC주소를 **MAC 주소 테이블**에 저장
  - MAC 주소 테이블에 주소가 있다면 바로 A에게 C의 MAC 주소 알려줌
- 스위치가 수신 포트를 제외한 나머지 포트에(**플러딩**) **브로드캐스팅**으로 메시지 전달
- 해당 IP 주소가 아닌 B, D는 메시지 폐기
- 해당 IP 주소인 C는 자신의 **MAC 주소를 응답**
- 스위치는 C의 주소를 MAC 주소 테이블에 저장
- A에게 **ARP 응답 메시지** 전달
- A는 **ARP 캐시 테이블**에 C의 주소 저장 후 **유니캐스트** 통신 시작

<br>

**_다른 네트워크 존재 시_**

- 위의 과정과 비슷하지만 중간에 라우터를 거치는 과정 발생 (라우팅)

---

<br>

## **ARP 프로토콜 패킷**

- ARP 패킷은 같은 네트워크 내에서만 동작하는 프로토콜
  - 다른 네트워크 통신의 경우 게이트웨이를 이용하여 목적지를 찾기 때문
  - 다른 네트워크로 넘어가기 위한 **IP 헤더 없음**
- **2계층**임이 기술 표준 문서에 명시됨

<br>

![image](https://user-images.githubusercontent.com/60606025/151143777-4dc817cf-4561-4370-8088-d8b08327e8ed.png)

---

<br>

### **_Tip_**

<br>

**플러딩 (flooding)**

- 데이터를 수신한 인터페이스를 제외한 나머지 모든 인터페이스로 데이터를 단순 복사하여 전송하는 과정

<br>

**MAC Learning**

- 스위치가 자신을 지나는 데이터의 MAC 주소 정보를 가지고 MAC 주소 테이블을 갱신하는 과정

<br>

**RARP**

- MAC 주소를 이용해 IP 주소를 매핑하는 프로토콜

<br>

Mac 주소 테이블과 ARP 테이블의 정보는 일정 시간만 유지
<br>
단말의 이동 시 IP 주소가 변경되기 때문
