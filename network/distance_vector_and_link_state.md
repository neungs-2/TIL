# Distance Vector & Link State

<br>

## **Routing 종류**

- **Static Routing**
  - 엔지니어가 수동으로 경로를 지정
  - 메모리 등 자원 소모가 적고 빠른 속도
  - 초기 설정 및 수정에 시간과 노력이 많이 소요

<br>

- **Dynamic Routing**
  - 라우터가 다른 라우터들과 경로 정보를 교환하여 자동으로 경로 설정
  - Static Routing에 비해 계산 등의 절차를 거쳐서 비교적 속도가 느림
  - 초기 설정 및 수정이 자동으로 갱신

<br>

**_IP Routing Protocol 종류_**
![image](https://user-images.githubusercontent.com/60606025/151323225-08001fe1-a537-47f8-9e50-0f973fa05dcf.png)

---

## **Distance Vector**

- 거리와 방향만을 위주로 만들어진 라우팅 알고리즘
- 라우팅 테이블에 **목적지 까지의 거리, 방향만 저장**
- 경로는 저장 X
- 인접 라우터와 주기적으로 라우팅 테이블을 교환
  - 개별 라우터에 모든 정보를 가질 필요 없어 메모리 절약
  - 주기적 테이블 교환으로 트래픽 낭비
  - 라우팅 테이블 변화 시 모든 라우터가 그 변화를 받아들이기까지 시간 소요
- Distance Vector인 프로토콜 종류
  - IGRP
  - RIP(v1, v2)

---

<br>

## **Link State**

- 하나의 라우터가 목적지까지의 모든 경로 정보 저장
  - 인접한 라우터에 의존 X
- SPF(최단경로우선) 알고리즘을 이용한 SPF 트리 사용
- 짧은 시간 내에 네트워크의 모든 라우터에게 전달 (빠른 변화 대처)
- 트래픽 발생 적음
- 많은 메모리, CPU 성능 소요
- initial flooding으로 인한 대역 부족
- Link State인 프로토콜 종류
  - OSPF
  - IS-IS

---

<br>

## **Tip**

**_AS(Autonomous System)_**

- 라우팅을 자율적으로 관리할 수 있는 네트워크 그룹과 게이트웨이
  - 자유로운 내부 라우팅 구성
  - 모든 AS의 네트워크 정보를 수집
  - 다른 AS의 네트워크 정보를 통과시킬 하나 이상의 게이트웨이 지정 필수
