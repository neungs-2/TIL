# E11000 duplicate key error collection

## **문제**

- API를 통해 DB에 데이터 입력 시 에러 발생
- `Unique Index`로 지정한 필드에 중복 값이 입력된 오류
- 기존에 생성해둔 *Collection*에 새로운 *Schema*를 적용하여 데이터를 입력한 것이 원인
  - 이전에 걸어둔 `Unique Index`가 존재하는 상황에서 새로운 스키마에는 `Unique Index`를 제거했기 때문에 문제 발생
- **Error Message**<br>
  `E11000 duplicate key error collection: cccv.spcEvent index: tokenId_1 dup key: { tokenId: undefined }`

<br>

## **해결방법**

- 기존에 만들어둔 *Unique Index*를 삭제
