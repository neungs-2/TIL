# **Factory Pattern**

- 생성자 디자인 패턴 중 하나

- **특정 구현으로부터 객체의 생성을 분리**
- 팩토리는 함수이기 때문에 사용자에게 더 적은 유연성을 제공
- 클래스보다 적은 면만을 노출하는 것이 가능
- 클로저를 활용하여 캡슐화를 강제할 수 있음
---

<br>

## ** 객체 생성과 구현의 분리**
- Javascript에서 순수 객체 지향 디자인보다 **함수형 방식**이 더 선호
  - 단순성, 유용성, 작은 노출 면 때문
- 새로운 객체의 인스턴스 생성 시에 특히 함수형 방식 선호
- `new`연산자, `Object.create()`를 사용하여 클래스에서 새로운 객체를 만드는 것보다 **팩토리를 호출하는 것이 편리하고 유연**
- **팩토리는 새 인스턴스 생성을 감싸**서 객체 생성 시 더 많은 유연성과 제어를 제공
  - 팩토리 내에서 **`new`로 클래스의 새 인스턴스를 생성**
  - 팩토리 내에서 **조건에 따라 다른 유형의 객체를 반환**
  - 팩토리 내에서 **클로저로 상태를 기억하는 객체 리터럴을 동적으로 작성**
- 팩토리를 사용하는 사람은 **인스턴스 생성 방식을 알 수 없음**

<br>

***코드 예시***
- `new` 연산자로 직접 인스턴스 생성 시 코드를 특정 유형의 객체에 바인딩
- 변경 및 확장 시 코드를 다시 확인하고 수정해야 함 => 유연성이 떨어짐
```js
const image = new Image(name);

// 이미지 형식마다 하나의 클래스를 지원하기 위해 Image 클래스를 쪼갠 경우
// 코드 필요 부분마다 다음과 같이 알맞은 클래스로 인스턴스 생성
const imageJPEG = new ImageJPEG(name);
......
const imageGIF = new ImageGIF(name);
......
const imagePNG = new ImagePNG(name);
......
```

<br>

- 변경 가능한 부분을 바뀌지 않는 부분과 분리
- 팩토리는 클래스를 숨겨 클래스가 확장하거나 수정하는 것을 막아줌
  - 클래스를 비공개로 유지 가능
```js
// 팩토리 예시
function createImage(name) {
  return new Image(name);
}

const image = createImage('photo.jpeg');

// 이미지 형식마다 하나의 클래스를 지원하기 위해 Image 클래스를 쪼갠 경우
// 기존 코드는 그대로 두고 Factory만 수정
function createImage(name) {
  if (name.match(/\.jpe?g$/)) {
    return new ImageJPEG(name);
  } else if (name.match(/\.gif$/)) {
    return new ImageGIF(name);
  } else if (name.match(/\.png$/)) {
    return new ImagePNG(name);
  } else {
    throw new Error('Unsupported format');
  }
}
```

---

<br>

## **캡슐화를 강제할 수 있는 매커니즘**
- 팩토리는 **클로저** 덕분에 **캡슐화** 매커니즘으로 사용 가능
  - **캡슐화**: 외부 코드가 컴포넌트 내부 핵심에 직접 접근하여 조작하는 것을 방지하기 위해 접근을 제어하는 것
  - 공용 인터페이스를 통해서만 컴포넌트와 상호작용 가능
  - 외부 코드를 컴포넌트 상세 구현과 분리
  - **객체지향 디자인의 기본원칙**: 캡슐화, 상속, 다형성, 추상화
- JS에서 **스코프와 클로저**를 이용하여 캡슐화를 구현
- **private 클래스 필드**를 이용하여 외부에서 사용할 수 없게 캡슐화를 구현할 수 있음

```js
// 클로저를 사용하여 2개의 객체를 생성
// 1) 팩토리가 반환하는 퍼블릭 인터페이스인 person 객체
// 2) 외부에서 접근 불가능하고 person 객체의 인터페이스를 통해서만 접근 가능한 privateProperties

function createPerson(name) {
  const privateProperties = {};

  const person = {
    setName(name) {
      if (!name) {
        throw new Error('A person must have a name');
      }
      privateProperties.name = name;
    },

    getName() {
      return privateProperties.name;
    }
  }

  person.setName(name);
  return person;
}
```

---

<br>

## ***간단한 코드 프로파일러 만들기***



---

<br>


## **Tip.**
***생성자 디자인 패턴***
- 객체의 생성과 관련된 문제들을 해결
- 객체의 생성/합성하는 방법, 객체의 표현 방법과 시스템을 분리
- 하위에 포함된 디자인 패턴
  - **Factory Pattern**: 함수 내에서 객체 생성을 캡슐화
  - **Revealing Constructor Pattern**: 생성 중에만 객체의 속성 및 함수들을 노출하는 것이 가능
  - **Builder Pattern**: 복잡한 객체 생성을 단순화
  - **Singleton Pattern** & **Dependency Injection**: 애플리케이션 내에서 모듈들을 연결 시 복잡성 제거턴


<br>

> Node.js Design Pattern Bible <br>
> https://jusungpark.tistory.com/14