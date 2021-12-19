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
