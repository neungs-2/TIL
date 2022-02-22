# **컴퓨터의 구성**

## **하드웨어와 소프트웨어**

- 하드웨어: 컴퓨터를 구성하는 기계적 장치

  - 중앙처리장치: CPU
  - 기억장치: RAM, ROM
  - 입출력 장치: 마우스, 프린터

- 소프트웨어: 하드웨어의 동작을 지시하고 제어하는 명령어 집합
  - 시스템 소프트웨어: 운영체제, 컴파일러
  - 응용 소프트웨어: Word, Excel

---

<br>

## **하드웨어**

![image](https://user-images.githubusercontent.com/60606025/155113130-e7af02ca-38ce-4e9c-8efd-65cb1e951e9b.png)

<br>

**_중앙처리장치(CPU)_**

- 주기억장치에서 프로그램 명령어와 데이터를 읽어와 처리
- 명령어의 수행 순서를 제어
- 구성요소
  - **산술논리연산장치(ALU)**: 비교와 연산을 담당
  - **제어장치**: 명령어 해석과 실행을 담당
  - **레지스터**: 속도가 빠른 데이터 기억장소

<br>

**_기억장치_**

- 프로그램, 데이터, 연산의 중간 결과를 저장하는 장치
- **캐시메모리**: CPU와 주기억장치의 속도 차이를 완화시키기 위한 고속 버퍼 메모리
- **주기억장치**
  - 현재 CPU의 처리 내용을 저장하는 장치
  - 용량이 크고 처리속도가 빠름
  - RAM(휘발성), ROM(비휘발성)
- **보조기억장치**
  - 물리적인 디스크가 연결되어 있는 기억장치
  - 컴퓨터의 전원을 꺼도 사라지지 않는 장치
  - HDD (디스크 기반), SSD (반도체 기반)

<br>

**_입출력장치_**

- 입력장치: 컴퓨터 내부로 자료를 입력하는 장치 (키보드, 마우스)
- 출력장치: 컴퓨터 외부로 표현하는 장치 (모니터, 스피커, 프린트)

---

<br>

## **시스템 버스**

- 하드웨어 구성 요소를 물리적으로 연결하는 통로
- 선, 광파이버 등 **하드웨어** 부품 및 통신 프로토콜을 포함한 **소프트웨어**를 통칭
- 용도에 따라 `데이터 버스`, `주소 버스`, `제어 버스`로 나뉨

![image](https://user-images.githubusercontent.com/60606025/155127847-ddc1240d-914f-494e-910b-255d7d7665fc.png)

<br>

**_데이터 버스_**

- CPU와 기타 장치 사이에서 데이터를 전달하는 통로
- 각 구성요소 (CPU, Memory, I/O Unit) 간 양방향으로 데이터 전달이 가능한 양방향 버스 사용

<br>

**_주소 버스_**

- 메모리 주소나 I/O Unit의 포트번호 전달을 위한 통로
- **CPU --> Memory**는 단방향 버스
- **CPU, Memory <---> I/O unit**은 양방향 버스

<br>

**_제어 버스_**

- 제어 신호를 전달하기 위한 통로
- **제어 신호 종류**
  - 기억장치 읽기/쓰기
  - 버스 요청 및 승인
  - 인터럽트 요청 및 승인
  - 클락, 리셋 등
- 각 구성요소 (CPU, Memory, I/O Unit) 간 양방향으로 데이터 전달이 가능한 양방향 버스 사용

---

> [참고]<br>https://dheldh77.tistory.com/entry/%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B5%AC%EC%A1%B0-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EB%B2%84%EC%8A%A4System-bus<br>https://gyoogle.dev/blog/computer-science/computer-architecture/%EC%BB%B4%ED%93%A8%ED%84%B0%EC%9D%98%20%EA%B5%AC%EC%84%B1.html