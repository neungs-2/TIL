# **Embedded Document**

## **Embedded Documemt란**

- Document 안 배열 형태로 있는 Document
- 연관된 데이터 간 one-to-many 관계를 표현하기 위해 사용

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
```

**[참고]**<br>https://www.mongodb.com/docs/manual/tutorial/model-embedded-one-to-many-relationships-between-documents/<br>https://inpa.tistory.com/entry/MONGO-%F0%9F%93%9A-Embedded-%EB%B0%B0%EC%97%B4-%EA%B0%9D%EC%B2%B4-%EA%B2%80%EC%83%89-%EC%BF%BC%EB%A6%AC
