# MySQL Time Zone Setting Error

<br>

## **문제**

- MySQL에서 time_zone 설정을 **Asia/Seoul**로 설정 시 문제 발생
  - `set time_zone = 'Asia/Seoul'`
- Asia/Seoul이라는 time_zone 옵션이 미등록된 것으로 보임
- **Error Message**
  <br>`ERROR 1298 (HY000): Unknown or incorrect time zone: 'Asia/Seoul'`

---

<br>

## **해결방법**

- DB가 설치된 서버에서 아래 코드를 한줄씩 실행
  - `$> mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root -p mysql`
  - `$> mysql_tzinfo_to_sql /usr/share/zoneinfo/Asia/Seoul KST`
- 이후 다시 time_zone 설정 시 오류 해결
  - `set time_zone = 'Asia/Seoul';`

<br>

- 단, 위의 방법만 적용하면 DB 재접속 시 설정이 풀림
- 영구 반영하기 위해서 mysqld.cnf에 설정 등록
  - `[root@(host)]$> vi /etc/mysql/mysql.conf.d/mysqld.cnf`
  - `default-time-zone=Asia/Seoul`을 맨 아래에 적어넣음

<br>

![image](https://user-images.githubusercontent.com/60606025/149471684-4e047883-9fe9-46c0-a987-ec41a9ed7d8f.png)
