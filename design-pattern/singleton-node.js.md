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

> **[참고]**<br> https://nodejs.org/api/modules.html#modules_caching > https://velog.io/@ehgks0000/NodeJS-Singleton-pattern
