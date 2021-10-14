# **Promise**

자바스크립트 비동기 처리에 사용되는 객체
<br><br>
비동기 작업이 미래에 완료 또는 실패하는 값을 나타냄

---

<br>

## **Promise의 상태**

<br>

### **_대기(pending)_** : 이행하거나 거부되지 않은 초기 상태

- `new Promise()` 매서드 호출 시 대기상태
- 이 때 콜백 함수를 선언할 수 있고 콜백 함수의 인자는 resolve, reject

<br>

### **_이행(fullfilled)_** : 연산이 성공적으로 완료

- 콜백 함수의 인자 resolve를 생행 시 이행 상태
- 이행 상태 시 `then()`을 사용하여 처리 결과 값 받음

<br>

### **_거부(rejected)_** : 연산 실패

- 콜백 함수의 인자 reject를 호출 시 실패 상태 됨
- 실패 상태 시 `catch()`로 실패한 이유(실패 처리의 결과 값) 받음

---

<br><br>

## **then( )과 catch( )**

- Promise.prototype.then( ) 과 Promise.prototype.catch( ) 메서드의 반환 값은 다른 프로미스이므로 서로 연결 가능

```js
function getData() {
  return new Promise({
    // ...
  });
}

getData()
  .then(function (data) {
    // ...
  })
  .then(function () {
    // ...
  })
  .then(function () {
    // ...
  });
```

<br>

> **참고** : https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
