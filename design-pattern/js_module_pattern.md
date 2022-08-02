# **Javascript Module Pattern**

## **모듈**

- 어플리케이션 일부를 **독립된 코드로 분리**하여 만들어 놓은 것
- **장점**
  - 재사용이 용이함
  - 유지보수가 용이함
    - 해당 모듈 수정 시 사용되는 모든 어플리케이션에 적용 가능
    - 코드 수정 시 필요한 로직 찾기 용이함
  - 필요한 로직만을 로드하여 메모리 낭비 방지
  - 브라우저에서 사용 시 모듈은 저장되므로 동일한 로직을 로드 시 시간과 네트워크 트래픽 절약 가능

---

<br>

## **모듈 패턴**

**_모듈 패턴 - 객체 리터럴 사용_**

```js
const module = {
  key: 'data',
  publicMethod: function () {
    return this.key;
  },
};

console.log(module.key); //data
console.log(module.publicMethod()); // data
```

- 객체 리터럴은 모듈 패턴인 동시에 하나의 객체라는 점에서 싱글톤 패턴으로 볼 수 있음
- 객체 리터럴은 간단하지만 모든 속성이 공개된다는 단점이 존재
- 독립된 모듈은 자체적으로 필요한 내부 변수 및 내부 함수가 필요하므로 클로저 필요

<br>

---

> **[참고]**<br>https://velog.io/@recordboy/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%ED%8C%A8%ED%84%B4<br>https://medium.com/%EC%98%A4%EB%8A%98%EC%9D%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EB%AA%A8%EB%93%88-%ED%8C%A8%ED%84%B4-d5ba2c94eeb5
