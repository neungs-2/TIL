# **Mapped Type**

- 기존에 정의되어 있는 타입을 새로운 타입으로 변환해주는 문법
- JS의 map 함수를 타입에 적용한 것

**_기본 문법_**

- `[P in K] : T` 형태로 사용
  - 괄호 부분은 `for문`과 유사하게 동작
  - **P**의 타입들을 순회하며 T 타입을 갖는 객체의 키로 정의

```ts
// readonly, optional parameter 적용하거나 안한 모습
{ [P in K] : T };
{ [P in K] ?: T };
{ readonly [P in K] : T };
{ readonly [P in K] ?: T };
```

<br>

**_기본 예제_**

```ts
type Heroes = 'Hulk' | 'Thor' | 'Capt';
type HeroProfiles = { [K in Heroes]: number };
const heroInfo: HeroProfiles = {
  Hulk: 54,
  Thor: 1000,
  Capt: 33,
};
```
