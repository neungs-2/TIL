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

- _Heroes_ 타입에 맵드 타입을 적용하여 _HeroProfiles_ 타입을 생성
- `[K in Heroes]`는 `for ... in` 문법과 유사하게 동작

  ```ts
  // 아래 줄별로 1~3번째 순회
  {
    Hulk: number;
  }
  {
    Thor: number;
  }
  {
    Capt: number;
  }

  // HeroProfiles 타입
  type HeroProfiles = {
    Hulk: number;
    Thor: number;
    Capt: number;
  };
  ```

<br>

**_맵드 타입 실용 예제1_**

- 실제 서비스 개발 시에는 `Heros`같은 유니온 타입 보다 아래처럼 객체의 *key*를 이용하는 경우가 많음

```ts
type Subset<T> = { [K in keyof T]?: T[K] };

interface Person {
  age: number;
  name: string;
}

// Subset 타입을 적용한 예시들
const ageOnly: Subset<Person> = { age: 23 };
const nameOnly: Subset<Person> = { name: 'Tony' };
const ironman: Subset<Person> = { age: 23, name: 'Tony' };
const empty: Subset<Person> = {};
```

<br>

**_맵드 타입 실용 예제2_**

- 맵드 타입을 사용해서 코드를 줄이는 예시
- 다음과 같이 `fetchUserProfile`에서 `UserProfile`을 사용하고 `updateUserProfile`에서 `UserProfileUpdate`를 사용함
- 두 *interface*가 _Optional parameter_ 여부만 다르기 때문에 반복 선언을 피해야 함

```ts
interface UserProfile {
  username: string;
  email: string;
  profilePhotoUrl: string;
}

function fetchUserProfile(): UserProfile {
  // ...
}

interface UserProfileUpdate {
  username?: string;
  email?: string;
  profilePhotoUrl?: string;
}

function updateUserProfile(params: UserProfileUpdate) {
  // ...
}
```

- 아래와 같이 중복 선언을 피하고 코드를 줄일 수 있음

```ts
// 이미 선언된 UserProfile을 이용하여 중복 선언을 피함
type UserProfileUpdate = {
  username?: UserProfile['username'];
  email?: UserProfile['email'];
  profilePhotoUrl?: UserProfile['profilePhotoUrl'];
};

// Mapped type을 활용해서 코드를 줄일 수 있음
type UserProfileUpdate = {
  [p in 'username' | 'email' | 'profilePhotoUrl']?: UserProfile[p];
};

// keyof를 적용해서 코드를 더 줄일 수 있음
type UserProfileUpdtate = {
  [p in keyof UserProfile]?: UserProfile[p];
};
```
