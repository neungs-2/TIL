# 다형성과 제네릭

## **다형성 (Polymorphism)**

- 하나의 객체가 여러 가지 타입을 가질 수 있는 것을 의미
- 다형성과 관련된 문제
  - 아래의 코드에서 배열에 저장된 객체의 형태를 알려주지 않아서 에러발생

```ts
type Filter = {
  (array: number[], f: (item: number) => boolean): number[]
  (array: string[], f: (item: string) => boolean): string[]
  (array: object[], f: (item: object) => boolean): object[]
}

function filter(array, f) {
  let result = []
  for (let i = 0; i < array;[i]; i++) {
    let item = array[i]
    if (f(item)) {
      result.push(item)
    }
  }
}

filter([1, 2, 3, 4], _ => _ < 3) // [1, 2] --> 문제 없음

let name = [
  {firstName: 'beth'}
  {firstName: 'caitlyn'}
  {firstName: 'xin'}
]

let result = filter(names, _ => _.firstName.startsWith('b')) // Error: firstName 프로퍼티는  object 타입에 존재하지 않음
result[0].firstName // 같은 이유로 에러
```

- 이 경우 `제네릭`을 이용하여 문제를 해결할 수 있음

```js
type Filter = {
  <T>(array:T[], f:(item:T) => boolean): T[]
}

let filter: Filter = (array, f) => {
  let result = []
  for (let i = 0; i < array;[i]; i++) {
    let item = array[i]
    if (f(item)) {
      result.push(item)
    }
  }
}


filter([1, 2, 3, 4], _ => _ < 3) // T는 number로 한정
filter(['a','b'], _ => _ != 'b') // T는 string로 한정
filter(names, _ => _.firstName.startsWith('b')) // T는 {firstName: string}로 한정
```

**_타입스크립트가 T를 특정 타입으로 한정하는 과정_**

- filter의 타입 시그니처를 통해 array가 타입이 T인 요소인 것 확인
- 전달되는 인수를 통해 T의 타입을 추론
- 모든 T를 추론한 타입으로 대치함

<br>

## **제네릭 (Generic)**

- 제네릭 타입 매개변수
  - 여러 장소에 타입 수준의 제한을 적용할 때 사용하는 플레이스홀더 타입(Placeholder type)
  - 다형성 타입 매개변수라고도 부름 (Polymorphic type parameter)
- 지금까지 사용한 모든 타입들은 구체 타입(concrete type)
- 하지만 어떤 타입을 사용할지 미리 알 수 없을 때 제네릭 타입을 사용
- 꺽쇠괄호(<>)로 제네릭 타입 매개변수임을 선언

<br>

**_언제 제네릭 타입이 한정되는가?_**

- 제네릭 타입의 선언 위치에 따라 **타입의 범위**와 **언제 구체 타입으로 한정**하는지 결정
- 보통 제네릭 구체 타입을 한정

```ts
// <T>를 호출 시그니처의 일부로 선언
type Filter = {
  <T>(array:T[], f:(item:T) => boolean):T[]
}

let filter: Filter = (array, f) => {...}

// <T>의 범위를 Filter의 타입 별칭으로 한정
type Filter<T> = {
  (array:T[], f:(item:T) => boolean):T[]
}

let filter: Filter<number> = (array, f) => {...}
```

<br>

**_제네릭을 어디에 선언할 수 있는가?_**

> 위의 내용과 연결됨.

- 호출 시그니처를 정의 하는 방법에 따라 제네릭을 추가하는 방법이 정해짐
- T의 범위에 따라 달라짐
  - 개별 시그니처로 한정 = <T>를 호출 시그니처의 일부로 선언
  - 모든 시그니처로 한정 = <T>의 범위를 Filter의 타입 별칭으로 한정

<br>

|      (T의 범위) ->       | 개별 시그니처로 한정 | 모든 시그니처로 한정 |
| :----------------------: | :------------------: | :------------------: |
| 함수 선언시 \<type> 명시 |          X           |          O           |
|   구체 타입 한정 시기    |     함수 호출 시     |     함수 선언 시     |
|   호출된 함수 인스턴스   |  각자 개별 T 한정값  | 모든 인스턴스 T 공유 |

```ts
// T의 범위 개별 시그니처로 한정
type Filter = {
  <T>(array:T[], f:(item:T) => boolean):T[]
}
// 위와 같지만 단축 호출 시그니처를 이용하면
type Filter = <T>(array:T[], f: (item: T) => boolean) => T[]

let filter: Filter = (array, f) => {...}

// <T>의 범위 모든 시그니처로 한정
type Filter<T> = {
  (array:T[], f:(item:T) => boolean):T[]
}
// 위와 같지만 단축 호출 시그니처를 이용하면
type Filter<T> = (array:T[], f: (item: T) => boolean) => T[]

let filter: Filter<number> = (array, f) => {...}
```
