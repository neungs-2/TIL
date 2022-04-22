# **_Array.push()_ with _Array.reduce()_**

## **문제**

- reduce 함수에서 previous value를 배열로 두고 그 배열에 push 함수를 사용할 경우 Error 발생
- 코드를 바꿔서 실행해보니 변수 pre를 Iterable로 평가하지 않음을 알 수 있음
- **Error Message**<br>`TypeError: pre.push is not a function`

```js
const arr = [[1, 2], [3], [4, 5, 6]];
const reduceArr = arr.reduce((pre, curr) => pre.push(curr.length), []);

reduceArr(arr); //TypeError: pre.push is not a function
```

<br>

## **해결 방법**

- Array.push는 push 이후 새로운 배열의 길이를 반환
- 따라서 변수 push 이용 시 pre는 숫자 --> Iterable이 아님
- **push 대신 spread 문법 사용**

```js
const arr = [[1, 2], [3], [4, 5, 6]];
const reduceArr = arr.reduce((pre, curr) => [...pre, curr.length], []);

reduceArr(arr);
```

> https://stackoverflow.com/questions/32722435/push-wont-work-as-expected-in-reduce

<br>

**_Tip_**

- **Iterable**: Symbol.iterator가 구현된 객체
- **유사배열**: index, length 프로퍼티가 있어서 배열처럼 보이는 객체
