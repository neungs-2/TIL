# Core API vs Callback API

- *MongoDB*는 트랜잭션 사요이을 위한 API로 ***Core API***와 ***Callback APi***를 제공
- ***Core API***는 관계형 데이터베이스와 유사한 구문 (ex. start_transaction, commit_transaction)
- ***Callback API***는 트랜잭션 사용에 권장되는 API

<br>

***Core API***
- 대부분의 오류에 재시도 로직을 제공하지 않음
- 개발자가 작업 관련 로직, 트랜잭션 커밋 함수, 재시도 및 오류 로직을 직접 작성해야 함
- 예시 코드 (Python)
  ```py
  def update_orders_and_inventory(my_session):
    orders = session.client.webshop.orders
    inventory = session.client.webshop.inventory

    with session.start_transaction(
      read_concern = ReadConcern('snapshot'),
      write_concern = WriteConcern(w='majority'),
      read_preference = ReadPreference.PRIMARY
    );

    orders.insert_one({'sku': 'abc123', 'qty': 100}, session=my_session)
    inventory.update_one({'sku': 'abc123', 'qty': {'$gte': 100}}, {'$inc':{'qty': -100}}, session=my_session)
    commit_with_retry(my_session)
  ```

<br>

***Callback API***
- *MongoDB* 4.2 버전에 추가 됨
- 코어 API에 비해 많은 기능을 래핑하는 단일 함수를 제공
  - 지정된 논리 세션과 관련된 트랜잭션 시작
  - 콜백 함수로 제공된 함수 실행
  - 트랜잭션 커밋 (또는 오류 시 중단)
  - **커밋 오류를 처리하는 재시도 로직**
- 애플리케이션 개발은 단순화
- 트랜잭션 오류를 처리하는 애플리케이션 재시도 로직을 쉽게 추가 가능
- 예시 코드 (Python)
  ```py
  def update_orders_and_inventory(my_session):
    orders = session.client.webshop.orders
    inventory = session.client.webshop.inventory

    orders.insert_one({'sku': 'abc123', 'qty': 100}, session=my_session)
    inventory.update_one({'sku': 'abc123', 'qty': {'$gte': 100}}, {'$inc':{'qty': -100}}, session=my_session)
  ```
<br>

***Core API vs Callback API***

||Core API|Callback API|
|:---:|:---:|:---:|
|Commit|트랜잭션을 시작하고 커밋하려면 명시적인 호출 필요|트랜잭션 시작하고 지정된 작업을 실행하면 커밋(또는 오류 시 중단)|
|`TransientTransactionError` 및 `UnknownTransactionCommitResult`에 대한 오류|처리 로직 통합 X, 사용자 지정 오류 처리를 통합|처리 로직을 자동으로 통합|
|특정 트랜잭션을 위해 API로 전달되는 명시적인 논리 세션 필요|O|O|
