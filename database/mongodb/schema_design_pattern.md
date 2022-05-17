# MongoDB Schema Design Pattern

## **다형성 패턴**(polymorphic pattern)
- 컬렉션 내 모든 도큐먼트가 유사하지만 동일하지 않은 구조
- 단일 컬렉션에서 정보에 액세스(쿼리)하려는 경우에 유용
- 실행하려는 쿼리를 기반으로 문서를 그룹화하면(테이블이나 컬렉션 간에 개체를 분리하는 대신) 성능 개선

```js
{
  "sport": "ten_pin_bowling",
  "athlete_name": "Earl Anthony",
  "career_earnings": {value: NumberDecimal("1441061"), currency: "USD"},
  "300_games": 25,
  "career_titles": 43,
  "other_sports": "baseball"
}
{
  "sport": "tennis",
  "athlete_name": "Martina Navratilova",
  "career_earnings": {value: NumberDecimal("216226089"), currency: "USD"},
  "event": [
    {
      "type": "singles",
      "career_tournaments":390,
      "career_titles": 167
    },
    {
      "type": "doubles",
      "career_tournaments": 233,
      "career_titles": 177,
      "partner": ["Tomanova", "Fernandez", "Morozova", "Evert", ...]
    }
  ]
}
```

> https://www.mongodb.com/developer/how-to/polymorphic-pattern/
---

<br>

## **속성 패턴**(attribute pattern)
- 다음의 경우에 적합한 패턴
  - (공통 특성을 갖는) 정렬하거나 쿼리하려는 도큐먼트에 필드의 서브셋이 있는 경우
  - 정렬하려는 필드가 도큐먼트의 서브셋에만 존재하는 경우
- 데이터 하위 집합을 키-값 하위 문서로 이동
  - 비결정적 필드 이름 사용
  - 정보에 추가 한정자를 추가
  - 원래 필드와 값의 관계를 명확하게 설명 가능
- **필요한 익덱스의 수가 적어지므로 쿼리가 더 간단하고 빨라짐**

```js
// 속성 패턴 적용 전
// 검색을 위해 필요한 색인: {release_US: 1}, {release_France: 1}, {release_Italy: 1}, ...
{
  title: "Star Wars",
  director: "George Lucas",
  ...
  release_US: ISODate("1977-05-20"),
  release_France: ISODate("1977-05-20"),
  release_Italy: ISODate("1977-05-20"),
  release_UK: ISODate("1977-05-20"),
  ...
}

// 속성 패턴 적용 후
// 검색을 위해 필요한 색인: {"releases.location": 1, "releases.date": 1}
{
  title: "Star Wars",
  director: "George Lucas",
  ...
  releases: [
    {
      location: "USA", 
      date: ISODate("1977-05-20")
    },
    {
      location: "France", 
      date: ISODate("1977-05-20")
    },
    {
      location: "Italy", 
      date: ISODate("1977-05-20")
    },
    {
      location: "UK", 
      date: ISODate("1977-05-20")
    },
    ...
  ],
  ...
}
```

> https://www.mongodb.com/developer/how-to/attribute-pattern/
---

<br>

## **버킷 패턴**(bucket pattern)
- 일정 기간 동안 스트림으로 들어오는 데이터(시계열 데이터)에 적합
- 특정 시간 범위의 데이터를 각각 보유하는 도큐먼트 셋으로 **버킷화**하는 것이 시간/데이터 포인트의 포인트 당 도큐먼트 생성보다 효율적
  - 1시간 버킷 사용시 해당 시간 동안의 모든 판독 값을 단일 도큐먼트 내 배열에 배치
- 컬렉션 전체 문서 수를 줄이고 인덱스 성능 개선
- 사전 집계를 활용한 데이터 엑세스 단순화

```js
// 버킷 패턴 적용 전
{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:00:00.000Z"),
  temperature: 40
}

{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:01:00.000Z"),
  temperature: 40
}

{
  sensor_id: 12345,
  timestamp: ISODate("2019-01-31T10:02:00.000Z"),
  temperature: 41
}

// 버킷 패턴 적용 후
{
  sensor_id: 12345,
  start_date: ISODate("2019-01-31T10:00:00.000Z"),
  end_date: ISODate("2019-01-31T10:59:59.000Z"),
  measurements: [
    {
      timestamp: ISODate("2019-01-31T10:00:00.000Z"),
      temperature: 40
    },
    {
      timestamp: ISODate("2019-01-31T10:01:00.000Z"),
      temperature: 40
    },
    ...
    {
      timestamp: ISODate("2019-01-31T10:42:00.000Z"),
      temperature: 42
    }
  ],
  transaction_count: 42,
  sum_temperature: 2413
}
```

> https://www.mongodb.com/developer/how-to/bucket-pattern/
---

<br>

## **이상치 패턴**(outlier pattern)
- 도큐먼트의 쿼리가 애플리케이션의 정상적인 패턴을 벗어날 때 사용
- *플래그(flag)*를 사용해 도큐먼트가 이상점임을 나타냄
  - 플래그는 애플리케이션 코드에서 오버플로 도큐먼트를 검색하기 위한 추가 쿼리를 만드는데 사용
- **추가 오버플로**를 하나 이상의 도큐먼트에 저장
  - 추가 오버플로: "_id"를 통해 첫 번째 도큐먼트를 다시 참조
- 인기도가 중요한 SNS에서 많이 사용

```js
{
  "_id": ObjectID("507f191e810c19729de860ea"),
  "title": "Harry Potter, the Next Chapter",
  "author": "J.K. Rowling",
  ...,
  "customers_purchased": ["user00", "user01", "user02", ..., "user999"],
  "has_extras": "true" // 플래그
}
```

> https://www.mongodb.com/developer/how-to/outlier-pattern/
---

<br>

## **계산된 패턴**(computed pattern)
- 반복적으로 계산이 필요한 데이터가 있거나 데이터 접근 패턴이 읽기 집약적일 때 사용
- 주요 도큐먼트가 주기적으로 갱신되는 백그라운드에서 계산을 수행하도록 함
- 동일한 계산이 반복되는 것을 방지
- CPU 워크로드를 줄이고 애플리케이션 성능 향상

> https://www.mongodb.com/developer/how-to/computed-pattern/
---

<br>

## **서브셋 패턴**(subset pattern)
- 장비에 RAM 용량을 초과하는 작업 셋이 있을 때 사용
- 자주 사용하는 데이터와 아닌 데이터를 두 개의 **별도 컬렉션에 분리**
  - 전자상거래에서 상위 10개 리뷰를 저장, 이외의 리뷰는 다른 컬렉션에 분리 후 필요 시에만 쿼리

> https://www.mongodb.com/developer/how-to/subset-pattern/
---

<br>

## **확장된 참조 패턴**(extended reference pattern)
- **확장된 참조 패턴**은 자주 엑세스하는 필드만 복사 (최소한의 중복만 허용)
- 고유한 컬렉션이 있는 여러 논리 엔티티(사물)가 있고 특정 기능을 위해 이 엔티티들을 모을 때 사용
- 반복적인 Join 작업이 많을 때 적합
- 엔티티를 모두 나누면 데이터 중복은 발생하지 않지만 Join이 많아져 성능 하락
- 개별 컬렉션에서 모든 정보를 포함 시 Join은 줄어들지만 중복정보가 발생하여 성능 하락 가능성 존재
- **확장된 참조 패턴**은 보다 적은 Join과 빠른 엑세스를 통해 성능 향상

