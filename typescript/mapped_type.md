# **Mapped Type**

- 기존에 정의되어 있는 타입을 새로운 타입으로 변환해주는 문법
- JS의 map 함수를 타입에 적용한 것

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
