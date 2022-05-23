# MongoDB 와 Mongoose 에서 **Update** 시 차이점

- MongoDB와 Mongoose의 `update` 쿼리는 동작 방식이 상이
  - **MongoDB**
    - 업데이트 할 내용을 그대로 _replace_
    - `$set` 연산자를 붙이면 _merge_
  - **Monogoose**
    - 업데이트 할 내용을 기존 데이터에 _merge_
    - 데이터 손실을 예방하기 위해 자동으로 `$set` 연산자 붙임
    - 옵션에 별도로 **overwrite** 항목을 넣으면 _replace_

<br>

```js
// MongoDB에서 collection.update 사용
const MongoClient = require('mongodb').MongoClient;

(async () => {
  const client = await MongoClient.connect('mongodb://localhost');
  const col = client.db('dev').collection('menumongodb');
  await col.insert({ name: 'Americano', price: 4500 });
  await col.insert({ name: 'Latte', price: 5000 });

  // MongoDB Driver API를 이용한 update
  await col.update({ name: 'Americano' }, { price: 10000 });
  const menus = await col.find().toArray();
  console.log(menus);
  await client.close();
})();

// [
//   { _id: 5c807976b79c7f2e34614f1c, price: 10000 },
//   { _id: 5c807976b79c7f2e34614f1d, name: 'Latte', price: 5000 }
// ]
```

<br>

```js
// Mongoose에서 model.update 사용
const mongoose = require('mongoose');

const Menu = mongoose.model(
  'MenuMongoose',
  new mongoose.Schema({
    name: String,
    price: Number,
  })
);

(async () => {
  const conn = await mongoose.connect('mongodb://localhost/dev');
  await new Menu({ name: 'Americano', price: 4500 }).save();
  await new Menu({ name: 'Latte', price: 5000 }).save();

  // Mongoose Model.update를 이용한 update
  await Menu.update({ name: 'Americano' }, { price: 10000 });
  const menus = await Menu.find().lean();
  console.log(menus);
  await conn.disconnect();
})();

// [
//  { _id: 5c807a308c90a52efb4e4003, name: 'Americano', price: 10000, __v: 0 },
//  { _id: 5c807a308c90a52efb4e4004, name: 'Latte', price: 5000, __v: 0 }
// ]
```
