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

**_모듈 패턴 - 클로저 사용_**

```js
// 클로저를 사용한 모듈 패턴: 객체를 반환
const module = (function () {
  // 모듈 패턴을 구현할 클로저 코드
  // 은닉할 맴버 정의
  let privateKey = 0;
  function privateMethod() {
    return privateKey++;
  }

  return {
    publicKey: privateKey,
    publicMethod: function () {
      return privateMethod();
    },
  };
})();

console.log(module.publicMethod()); // 0
console.log(module.publicMethod()); // 1
```

- 모듈 패턴의 반환값은 함수가 아닌 객체
- 익명합수가 자동으로 실행되고 반환된 객체를 module 변수에 할당

<br>

```js
// 단순 클로저
function func() {
  let private = 0;
  return function () {
    private++;
    return private;
  };
}

const val = func();
console.log(val()); // 1
console.log(val()); // 2
```

- 모듈 패턴을 적용하지 않고 함수를 리턴하는 코드

<br>

```js
// 여러 개의 인스턴스를 생성하기 위한 모듈 패턴: 익명함수 대신 생성자 함수 방식
const Module = function () {
  let privateKey = 0;
  function privateMethod() {
    return privateKey++;
  }

  return {
    publicKey: privateKey,
    publicMethod: function () {
      return privateMethod();
    },
  };
};

const obj1 = Module();
console.log(obj1.publicMethod()); // 1
console.log(obj1.publicMethod()); // 2

const obj2 = Module();
console.log(obj2.publicMethod()); // 1
console.log(obj2.publicMethod()); // 2
```

- 익명함수 대신 생성자 함수 방식 사용 시 여러 개의 인스턴스 생성 가능
- 클로저 인스턴스와의 차이점은 함수가 아닌 객체 반환

---

<br>

## **Tip**

**_싱글톤 패턴_**

```js
const singleton = (function () {
  let instance;
  const private = 0;
  function init() {
    return {
      publicKey: private,
      publicMethod: function () {
        return publicKey;
      },
    };
  }
  return function () {
    // 싱글톤 패턴이므로 처음 인스턴스 생성 시 다시 인스턴스 생성 X --> 기존 인스턴스 반환
    if (!instance) {
      instance = init();
    }
    return instance;
  };
})();

const singleton1 = singleton();
const singleton2 = singleton();
console.log(singleton1 === singleton2); // true
```

- 모듈 패턴은 여러개의 인스턴스를 생성하는 식으로 구현 가능
- 싱글턴 패턴은 인스턴스를 하나만 생성한다는 차이점 존재

<br>

> **[참고]**<br>https://velog.io/@recordboy/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%ED%8C%A8%ED%84%B4<br>https://medium.com/%EC%98%A4%EB%8A%98%EC%9D%98-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EB%AA%A8%EB%93%88-%ED%8C%A8%ED%84%B4-d5ba2c94eeb5
