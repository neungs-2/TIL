# npm install Error: 'pickAlogorithm' of null error

## **문제**
- `npm install` 시 에러가 발생하여 **node_modules** 패키지를 완성할 수 없음
- 설치 과정에서 아래와 같은 에러 메시지 발생
- **Error Message**
  - `npm ERR! Cannot read properties of null (reading 'pickAlgorithm)`

<br>

## **해결방법**
- `npm cache clear --force` 명령어를 실행
- 이후 다시 `npm install` 실행

<br>

## **발생 원인**
- `npm install`을 실행하여 패키지들을 설치하다보면 **npm-cache/_cache** 폴더에 캐시가 저장됨
- 모든 HTTP 요청 데이터와 패키지 관련 데이터를 저장하는 캐시
- 패키지 버전 이슈로 충돌이 일어나 꼬이는 경우 발생
- 따라서 캐시 데이터를 지워주어 오류 해결 가능