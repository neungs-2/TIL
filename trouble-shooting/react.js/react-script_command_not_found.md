# 'npm start' doesn't work

## **문제**

- 리액트 프로젝트에서 `npm start` 명령어가 동작하지 않을 때
- **Error message**
  - `sh: 1: react-scripts: not found`

<br>

## **해결 방법 1.**

- `npm install`을 실행
- **package.json**의 script 부분이 읽히지 않아서 발생하는 문제로 `npm install`을 하지 않았을 가능성이 큼
- **package.json**, **package-lock.json** 모두 있는데 실행이 되지 않는다면 둘 다 삭제 후 다시 설치
- **npm install** 이 실패하는 경우 해당 케이스에 맞는 해결 방법을 찾을 것