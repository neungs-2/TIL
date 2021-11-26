# MySQL Operand Columns Error

## **문제**

- 서버에서 DB의 데이터를 조회하는 중 문제 발생
- MySQL Error: 1241 에러
- 쿼리문은 아래와 같음
  - `SELECT (col_1, col_2, col_3) FROM table WHERE col_1 = 'col_1'`
- **Error Message**<br>
  `sqlMessage: 'Operand should contain 1 column(s)'`

<br>

## **해결방법**

- 서브 쿼리에서 부정확한 수의 컬럼을 제시할 시에 발생하는 에러
- 괄호가 서브쿼리로 인식 것으로 보임
- 쿼리문 SELECT 부분에 괄호를 제거
  - `SELECT col_1, col_2, col_3 FROM table WHERE col_1 = 'col_1'`
- 사용 목적이 비교인 경우 여러 개의 컬럼을 리턴하는 서브쿼리를 사용 가능
- 다른 목적인 경우 서브쿼리는 scalar 연산 함수여야 함
