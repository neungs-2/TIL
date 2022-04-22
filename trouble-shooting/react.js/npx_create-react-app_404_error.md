# npx create-react-app 명령어 404 Error 발생

<br>

## **문제**

- `npx create-react-app 생성할_프로젝트_폴더명` 명령어 실행 시 404 Error 발생
- **Error message**<br> npm ERR! 404 Not Found - GET https://registry.npmjs.org/creat-react-app - Not found

---

<br>

## **해결 방법 1.**

- `npm config set registry http://registry.npmjs.org` 를 실행

<br>

## **해결 방법 2.**

- npm 으로 create-react-app 실행
- `npm install -g create-react-app` 으로 cra 설치
- `create-react-app 생성할_프로젝트_폴더명` 으로 생성할 프로젝트 폴더에서 cra 실행
- `npx create-react-app`으로 react 설치 시 package-lock.json을 생성하지 않으므로 팀원과의 개발환경 공유 시 해당 방법을 사용하는 것을 권장
