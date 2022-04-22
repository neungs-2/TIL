# Redis Connection refused

## **문제**

- Redis를 새롭게 설치한 후 `redis-server --daemonize yes`로 서버를 데몬 실행
- `redis-cli` 명령어를 입력해도 연결되지 않음
- `ps -ef | grep redis`로 살펴보니 프로세스 자체가 start 되지 않음
- Systemctl restart로 재부팅해도 연결되지 않음
- **Error Message**
  <br>
  `Could not connect to Redis at 127.0.0.1:6379: Connection refused`

<br>

## **문제 원인**

- Redis를 처음 사용해서 6379번 Port가 닫혀있었음

---

<br>

## **해결 방법**

- local에서 6379번 Port 열어주기
  - `sudo ufw allow 6379`
  - `sudo service redis-server restart`
