# Oracle Cloud Platform server external connection Error

<br>

## **문제**

- MySQL Workbench에서 등록해놓은 DB 서버 접속할 수 없다는 에러 발생
- Node Express와 DB 연결 역시 에러 발생
- 터미널을 통해 Docker 서버 --> OCP 서버 --> DB 접근 및 사용 가능

---

<br>

## **해결 방법**

- 서버 방화벽에서 3306 포트를 열어주니 해결
- 명령어는 iptables와 그 대안인 firewall(방화벽 데몬) 존재
  - `sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent`
  - `sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 3306 -m state --state NEW,ESTABLISHED -j ACCEPT`

---

<br>

## **Note**

- **_문제 발생 원인_**
  - 외부에서 3306 포트로 접근할 수 없게 막혀 있었기 때문
  - 열려있던 포트가 닫힌 이유는 찾지 못함
- **_터미널에서는 접근 가능했던 이유_**
  - DB는 OCP 서버에 설치되어 있기 때문
  - 로컬에서 OCP 서버로 넘어간 이후 DB 접속 시 포트를 거치는 것이 아니라 로컬 호스트로서 접근하기 때문
