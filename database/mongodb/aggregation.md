# 집계 프레임워크

## **파이프라인, 단계 및 조정 가능 항목**
- **집계 프레임워크**: 하나 이상의 Collection에 있는 Document에 대한 분석 수행
- **파이프라인** 개념 기반
  - 집계 파이프라인을 통해 MongoDB Collection에서 입력받음
  - 컬렉션에서 나온 Document에 하나 이상의 단계(작업) 거침
  - 각 단계는 **전단계 출력을 입력**으로 받음
  - 모든 단계의 **입/출력은 Document**
- 집계 파이프라인의 개별 단계는 **데이터 처리 단위**(data processing unit)
  - 한번에 도큐먼트 스트림을 하나씩 가져와서 처리 => 출력 도큐먼트 스트림 하나 생성
  - 각 단계는 **knobs** 또는 **tunables** 셋을 제공
    - 이 항목들을 조정하여 각 단계를 매개변수로 지정
  - **tunables**: 필드 수정, 산술 연산, 도큐먼트 재구성, 누산 작업 등 작업을 수행하는 연산자의 형태

> 동일한 유형의 단계를 단일 파이프라인에 여러 번 포함하는 것이 효율적인 경우 존재!! <br> ex) 필터링을 위한 동일한 일치 작업 복수 번 수행

<br>

## **단계 시작하기: 익숙한 작업들**
***일치(match)***
- SQL문의 `WHERE` 같은 **필터링** 명령어
- `find()`로 조회 명령어와 유사
```js
db.collection.aggregate([
  {$match: {founded_year: 2004}},
])

// 같은 결과를 반환하는 find 문
db.companies.find({founded_year: 2004})
```

<br>

***선출(projectioin)***
- 노출 시킬 필드 선택 (0: false, 1: true)
- `_id` 필드는 default **1**, 나머지 필드는 default **0**

```js
db.collection.aggregate([
  {$match: {founded_year: 2004}},
  {$project: {_id: 0, name: 1, founded_year: 1}}
])
```

<br>

***제한(limit)***
- 결과 셋의 개수를 제한
- 로직에서 가능한 빠르게 제한하는 것이 효율적
  - 아래에서 선출단계 이전에 제한단계 먼저 수행
```js
db.collection.aggregate([
  {$match: {founded_year: 2004}},
  {$limit: 5},
  {$project: {
    _id: 0,
    name: 1
  }}
])
```

<br>

***건너뛰기(skip)***
- 건너뛰기 단계 수행 시 결과 셋에서 정해진 개수만큼 건너뛰고 출력

```js
db.collection.aggregate([
  {$match: {founded_year: 2004}},
  {$skip: 10}
  {$limit: 5},
  {$project: {
    _id: 0,
    name: 1
  }}
])
```
<br>

***정렬(sort)***
- 정렬 시 제한단계 이전에 수행해야 하는 것 주의!!
```js
db.collection.aggregate([
  {$match: {founded_year: 2004}},
  {$sort: {name:1}},
  {$limit: 5},
  {$project: {
    _id: 0,
    name: 1
  }}
])
```

<br>

## **표현식**
- **불리언 표현식 (boolean expression)**: `AND`, `OR`, `NOT` 표현식 사용 가능
- **집합 표현식 (set expression)**: 집합 표현식을 사용 시 배열을 집합으로 사용 가능 (`교집합`, `합집합`, `차집합` 등 집합 연산 가능)
- **비교 표현식 (comparison expression)**: 다양한 유형의 범위 필터 표현 가능
- **산술 표현식 (arithmetic expression)**: `ceiling`, `floor`, `자연로그`, `로그`, `곱하기`, `나누기`, `더학기`, `빼기`, `제곱근` 등 산술연산 수행 가능
- **문자열 표현식 (string expression)**: `연결 (concatenate)`, `하위 문자열 검색 (substring)`, `대소문자 검색`, `텍스트 검색` 등 관련 작업 가능
- **배열 표현식 (array expression)**: 배열 요소의 필터링, 분해, 특정 배열에서 값의 범위를 가져오는 등 배열 조작
- **가변적 표현식(variable expression)**: 리터럴, 날짜 값 구문 분석을 위한 식, 조건식을 사용
- **누산기(accumulator)**: 합계, 기술 통계 및 기타 여러 유형 값을 계산 가능

<br>

## **$project**
- 선출 단계로 재구성 작업

<br>

***중첩 필드 승격 (nested field promoting)***
- 중첩 객체의 경우 **점 표기법**을 이용하여 경로를 선택
  - 출력으로 생성되는 도큐먼트에서 최상위 값이 됨
- `$`문자는 선출 단계에서 ipo, valuation, funders에 대한 값을 지정하는데 사용
- 아래와 같이 funders 선출하는 경우 funding_rounds와 investments가 배열이므로 해당하는 모든 funder(permalink)값들이 이차원 배열로 출력됨 (p238 참고)
```js
db.companies.aggregate([
  {$match: {"funding_rounds.investments.financial_org.permalink": "greylock"}},
  {$projectioin: {
    _id: 0,
    name: 1,
    ipo: "$ipo.pub_year",
    valuation: "$ipo.valuation_amount",
    funders: "$funding_rounds.investments.financial_org.permalink"
  }}
]).pretty()
```

<br>

## **$unwind**

<br>

## **배열 표현식**

<br>

## **누산기**

<br>