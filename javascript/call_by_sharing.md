# **Call by Sharing**

## **Call by Value**

- argument로 값이 넘어올 때 **복사된 값**이 넘어옴
- `caller`가 인자를 복사해서 넘겨줘서 `callee`에서 해당 인자를 변경해도 `caller`는 영향 받지 않음

```js
let a = 1;

// callee
function addOne(b) {
  b = b + 1;
}

// caller
addOne(a);

console.log(a); // 1
```

- a라는 변수를 인수로 넘겨줌
- 이때 b에는 **a의 값 1이 복사**되어 넘겨짐
- a, b의 값은 같지만 둘 다 다른 메모리 공간을 차지하게 되어 별개의 존재
- b가 변경되어도 a는 그대로

---

<br>

## **Call by Reference**

- argument로 값이 넘어올 때 **reference**가 넘어옴
- 참조 주소를 복사하므로 결국 가리키는 값이 동일
- `caller`가 참조하는 값을 복사하지 않으므로 `callee`에서 해당 인자 변경 시 `caller` 영향 받음

<br>

- Reference
  - 값에 대한 참조 주소
  - 메모리 주소를 담고있는 변수

<br>

```js
const me = {
  name: 'Jimmy',
};

// callee
function changeName(person) {
  person.name = 'Joo';
}

console.log(me); // Jimmy

// caller
changeName(me);

console.log(me); // Joo
```

- 매개변수 person에 인수로 넘겨진 me의 참조값이 전달
- person과 me는 같은 참조값 지님
- `changeName` 함수 내에서 `person.name` 변경 시 `me.name`도 변화

---

<br>

## **Call by Sharing**

- argument로 값이 넘어올 때 **reference**가 넘어옴
- 참조 주소를 복사하므로 가리키는 값이 동일
- 인자를 변경하는 것이 아닌 **새로운 참조값을 변수에 할당**
- 결국 `caller`가 참조하는 값이 `callee`가 참조하는 값과 달라짐
- 인자를 받을 때는 `Call by Reference`처럼 받고 결과는 `Call by Value`처럼 나옴

```js
const me = {
  name: 'Jimmy',
};

// callee
function changeName(person) {
  person = { name: 'Joo' };
}

console.log(me); // { name: 'Jimmy' }

// caller
changeName(me);

console.log(me); // { name: 'Jimmy' }
```

<br>

## ![image](https://user-images.githubusercontent.com/60606025/156913908-c6b1536b-8cde-49ed-b9dd-2db52124cffd.png)

<br>

> **[참조]** > <br>https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Call-by-Value-Call-by-Reference
