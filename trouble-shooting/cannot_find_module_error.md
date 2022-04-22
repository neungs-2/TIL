# Cannot find module \<path\> Error

<br>

## **문제**

- `Nest`에서 `jest`를 사용한 테스트를 진행하는 경우 발생하는 에러
- import 하는 모듈의 경로를 제대로 읽지 못하는 것으로 보임
- **Error Message**
  <br> `Cannot find module 'src/schemas/adminsBanners.schema' from 'src/admins-banners/admins-banners.service.ts'`

---

<br>

## **해결방법 1.**

- 절대경로를 **상대경로**로 변경
- 위의 경우 `src/~` 부분을 `../~`로 변경함

## **해결방법 2.**

- `jest`의 config 설정 변경
- **package.json** 내부의 `jest` 설정

```js

```
