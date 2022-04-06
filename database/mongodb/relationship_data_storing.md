# **Relationship Data Storing**

- 관계데이터 저장 방법은 2가지 존재
  - **Reference**
  - **Embedded**

<br>

## **Reference**

- Document를 통째로 저장하는 것이 아니라 참조할 수 있도록 id를 저장
- 관계형 DB와 유사
- 아래에서 `movie_id` 항목이 `movie` 컬렉션을 참조

```js
//movie
{
     "_id" :  1 ,
     "title" :  "The Arrival of a Train" ,
     "year" :  1896 ,
}

//movie_detail
{
    "_id" :  156 ,

    "movie_id" :  1 ,  // movie Colletion 참조 (*)

    " plot " :  "한 무리의 사람들이 기차역에서... " ,
    "fullplot" : "한 무리의 사람들이 기차역에서 플랫폼을 따라 다가오는 기차를 기다리고 있습니다."
    "type": "movie",
    "directors": [ "Auguste Lumière", "Louis Lumière" ],
    "imdb": {
      "rating": 7.3,
      "votes": 5043,
      "id": 12
    },
    "countries": [ "France" ],
    "genres": [ "Documentary", "Short" ],
}
```

## **Embedded**

- 두 종류의 Document에서 하나의 Document를 다른 Document에 포함시키는 방법
- 아래에서 user.address에 Document가 통째로 저장

```js
//User
{
	_id : "hanganda",
  	name : "yuno",
  	address : {
            street : "Banpo dae-ro",
            city : "Seoul",
            zip : "111111",
    	},
}
```

<br>

### **_Embedded Documemt_**

- Document 안 배열 형태로 있는 Document
- 주로 one-to-many 관계를 표현하기 위해 사용
- `$elemMatch`를 사용하여 조건이 배열 안의 요소와 일치하는 필드를 선택

```js
{
  "_id": "joe",
  "name": "Joe Bookreader",
  "addresses": [
    {
      "street": "123 Fake Street",
      "city": "Faketon",
      "state": "MA",
      "zip": "12345"
    },
    {
      "street": "1 Some Other Street",
      "city": "Boston",
      "state": "MA",
      "zip": "12345"
    }
  ]
}

$> db.book.find({"address": { $elemMatch: {"city": "Faketon"} } })
```

**[참고]**<br>https://www.mongodb.com/docs/manual/tutorial/model-embedded-one-to-many-relationships-between-documents/<br>https://inpa.tistory.com/entry/MONGO-%F0%9F%93%9A-Embedded-%EB%B0%B0%EC%97%B4-%EA%B0%9D%EC%B2%B4-%EA%B2%80%EC%83%89-%EC%BF%BC%EB%A6%AC
https://velog.io/@hanganda23/MongoDB-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81
