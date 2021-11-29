# **Promisify**

- util module에 내장된 함수
- 비동기로 돌리려는 함수를 promise로 감싸지 않고 사용 가능
- 비동기 처리를 위한 async - await 구문 사용 시 await은 Promise에 사용해야 함
- `new Promise()`를 사용하여 프로미스를 만들 수 있음
- `Promisify()`를 사용하면 구문을 보다 간결하게 표현 가능

<br>

**_new Promise() 사용 시_**

```js
async() => {
  const res = await new Promise((resolve, reject) => {
    fn(err, val) => {
      if(err) {
        console.error(err)
      }else{
        resolve(val)
      }
    }
  })
```

<br>

**_Promisify() 사용 시_**

```js
const proPromise = util.promisify(fn)
const res = await proPromise().then(Exception Handling);
```
