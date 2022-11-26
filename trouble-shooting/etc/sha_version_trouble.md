# Ubuntu Version에 따른 Pem key 인증 문제

## **문제**

- _Jenkins_ 자동 배포 설정 시 *EC2*에 접근 불가한 문제 발생
- *PEM key*를 `.ssh`에 위치시켰지만 `Auth Fail` 발생
- **Error Message**
  - `jenkins.plugins.publish_over.BapPublisherException: Failed to connect and initialize SSH connection. Message: [Failed to connect session for config [server__name]. Message [Auth fail]]`

---

<br>

## **해결방법**

- `etc/ssh/sshd_config`에 하단 설정을 추가
  - `PubkeyAuthentication yes`
  - `PubkeyAcceptedKeyTypes +ssh-rsa`
- sshd 서비스 재실행
- 또는 _Ubuntu_ 버전을 18이하로 다운그레이드

---

<br>

## **발생 원인**

- _OpenSSH_ 플러그인이 **_SHA2_** 방식이 아닌 **_SHA1_** 방식으로 접속시켜서 발생
- 높은 버전의 Ubuntu(혹은 OpenSSH)는 `SHA1` 방식이 기본값이 아니라 발생
