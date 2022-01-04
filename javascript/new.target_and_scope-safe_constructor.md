# **new.target** and **Scope-safe Constructor**

두 방법을 이용하여 생성자 함수가 new 연산자 없이 호출되는 것 방지<br>
조건문을 걸어서 new 연산자 없이 호출해도 생성자 함수로서 호출되게 코딩

<br>

## **new.target**

- 생성자 함수로서 호출 시 `new.target`은 함수 자신을 가리킴
- new 연산자 없이 일반함수로 호출 시 `new.target`은 undefined

```js
function Circle(radius) {
  // 조건문을 걸어줌!
  if (!new.target) return new Circle(radius);

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

<br>

## **스코프 세이프 생성자 패턴**

- new.target이 ES6에서 도입되어 브라우저에서 지원하지 않는 경우 사용

```js
function Circle(radius) {
  // 조건문을 걸어줌!
  if (!(this instanceof Circle)) return new Circle(radius);

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

---

<br>

## **객체 생성 방법**

- Object 생성자 함수
  - `const a = new Object();`로 생성
  - 이후 동적 프로퍼티 추가
  - 유용하지 않음
- 객체 리터럴
  - `const a = {firstName:'Logan', lastName:'Lee'};`
  - 일반적으로 사용
  - 다만 하나의 객체만 생성
- **생성자 함수**
  - 함수도 객체이므로 생성자 함수를 사용하여 객체 생성 가능
  - 템플릿(클래스) 역할하여 프로퍼티 구조 동일한 다수의 객체 생성 가능

```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const c1 = new Circle(5);
const c2 = new Circle(10);

console.log(c1.radius); // 5
```

<br>

## **Constructor function**

- [[Construct]]를 갖는 함수 객체 --> **constructor**
  - 일반 함수(함수 선언문, 함수 표현식, 클래스(클래스도 함수임))
  - 메서드(ES6 메서드 축약 표현만 인정), 화살표 함수는 non-constructor
- 일반 함수를 **new** 연산자와 함께 호출하면 생성자 함수로 동작
- 생성자 함수는 **파스칼 케이스**로 정의
- return을 생략하는 것을 권고
  - return 생략 시 this를 암묵적으로 반환
  - return 기입 시
    - 원시값 반환은 무시되고 암묵적 this 반환
    - 객체 반환 시 객체가 반환
- 인스턴스의 this 바인딩은 런타임 이전에 실행
- 이후 런타임 때 인스턴스 초기화
  - 프로퍼티, 메서드 추가

---

<br>

## Tip.

- `this`는 자기 참조 변수
- this가 가리키는 값(this 바인딩)은 호출 방식에 따라 동적으로 결정

|  함수 호출 방식  | this 바인딩                            |
| :--------------: | :------------------------------------- |
|  일반 함수 호출  | 전역객체                               |
|   메서드 호출    | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수 호출 | 생성할 인스턴스                        |
