# **Singleton Pattern with Node.js**

## **Singleton Pattern**

- 하나의 프로그램 내에서 하나의 인스턴스가 한 번만 메모리에 할당하여 생성
- 클래스의 인스턴스가 하나만 존재하도록 접근을 중앙 집중화
- 사용할 때마다 객체를 생성하지 않고 필요시 가져다 씀
- 데이터 공유가 쉽고 메모리 낭비 줄임

<br>

**장점**

- _상태정보의 공유_
- _리소스 사용의 최적화_
- _리소스에 대한 접근 동기화_

<br>

## **Node JS와 Singleton**

- Node.js는 한번 require한 모듈을 require.cache에 저장
  - 매번 새로운 인스턴스가 생성되는 것이 아닌 캐싱된 인스턴스를 재사용
- Node.js에서는 굳이 싱글턴을 구현하기보다 module.exports를 사용하는 것이 편리함

<br>
ex) DB 연동 시 최초 require된 시점에서 pool이 생성되고, 그 이후에 require를 수행해도 pool은 더 이상 생성되지 않음. 따라서 DB Pool 생성하는 코드를 모듈화해서 require해서 사용해도 괜찮음

```js
const mysql = require('mysql2/promise');
const config = require('../conf/db');

module.exports = mysql.createPool(config.poolOption);
```

<br>

## **Node에서 Singleton 구현 방법**

<br>

**_IIFE로 객체 생성_**

- 즉시 실행함수를 인라인함수로 만들어서 구현 가능

```js
var singleton = (function () {
  var instance;
  var a = 'singleton';
  function init() {
    return {
      a: a,
      b: function () {
        console.log(a);
      },
    };
  }
  return {
    getInstance: function () {
      if (!instance) {
        instance = init();
      }
      return instance;
    },
  };
})();
var singletone1 = singleton.getInstance();
var singletone2 = singleton.getInstance();
console.log(singletone1 === singletone2); // true;
```

<br>

**_Class 생성자 이용_**

- 클래스의 생성자를 조작하여 싱글톤 구현 가능

```js
// singleton.js
class PrivateSingleton {
  constructor() {
    this.message = 'I am an instance';
  }
}
class Singleton {
  constructor() {
    throw new Error('Use Singleton.getInstance()');
  }
  static getInstance() {
    if (!Singleton.instance) {
      Singleton.instance = new PrivateSingleton();
    }
    return Singleton.instance;
  }
}
module.exports = Singleton;
```

```js
// 인스턴스 불러와서 확인
const Singleton = require('./Singleton');
const object = Singleton.getInstance();
console.log(object.message); // Prints out: 'I am an instance'
object.message = 'Foo Bar'; // Overwrite message property
const instance = Singleton.getInstance();
console.log(instance.message); // Prints out: 'Foo Bar'
```

<br>

**_Cached Singleton_**

- Node JS의 Module 캐싱의 이점을 활용한 방법 --> 추천

```js
class Singleton {
  constructor() {
    this.message = 'I am an instance';
  }
}
module.exports = new Singleton();
```

```js
//require해서 확인
const Singleton = require('./Singleton');
const object = Singleton;

console.log(object.message); // Prints out: 'I am an instance'
object.message = 'Foo Bar'; // Overwrite message property

const instance = Singleton;
console.log(instance.message); // Prints out: 'Foo Bar'
```

> **[참고]**<br> https://nodejs.org/api/modules.html#modules_caching > https://velog.io/@ehgks0000/NodeJS-Singleton-pattern
