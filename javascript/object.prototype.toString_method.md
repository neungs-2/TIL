# **Object.prototype.toString( ) Method**

## **`Object.prototype.toString()` 이란?**

- 모든 객체에 사용되어 해당 객체의 **클래스의 타입**을 반환하는 메서드
- `Function.prototype.call`이나 `Function.prototype.apply`를 함께 사용해야 함
- 프로토타입 메서드로 **오버라이딩** 가능 (`typeof`는 연산자로 오버라이딩 불가능)

```js
Object.prototype.toString.call(''); // '[object String]'
Object.prototype.toString.call(1); // '[object Number]'
Object.prototype.toString.call(true); // '[object Boolean]'
Object.prototype.toString.call(Symbol('symbol')); // '[object Symbol]'
Object.prototype.toString.call(BigInt(1e10)); // '[object BigInt]'
Object.prototype.toString.call([]); // '[object Array]'
Object.prototype.toString.call({}); // '[object Object]'
Object.prototype.toString.call(null); // '[object Null]'
Object.prototype.toString.call(undefined); // '[object Undefined]'
```

<br>

- 객체가 아닌 경우 프로토타입 체인에서 `String.prototype.toString`, `Number.prototype.toString` 처럼 다른 `toString` 메서드가 먼저 걸림

```js
const str = 'abc';
const num = 34;
const abj = { a: 1, b: 'abc' };

str.toString(); // 'abc'
num.toString(); // '34'
obj.toString(); // '[object Object]'
```

<br>

## **자주하는 실수**

- 객체를 문자열화 시키기 위해 `toString` 사용 시 `[object Object]`가 반환
- 이 경우 `JSON.stringify`를 사용할 것

```js
const obj = { a: 1, b: 'abc' };

obj.toString(); // '[object Object]'
JSON.stringify(obj); // '{ "a": 1, "b": "abc" }'
```
