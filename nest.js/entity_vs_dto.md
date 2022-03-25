# **Entity vs DTO**

![image](https://user-images.githubusercontent.com/60606025/160065520-0cc90f34-0db2-491f-a6b2-368e54b0971c.png)

<br>

## **Entity**

- 실제 DB 테이블과 매칭될 객체
- 실질적인 스키마로 각 속성으로 이루어진 테이블 구조
- *NestJS*에서는 Entity를 사용하여 DTO 생성 가능

```ts
export class Movie {
  id: number;
  title: string;
  year: number;
  genres: string[];
}
```

---

  <br>

## **Data Transfer Object (DTO)**

- 계층간 데이터 교환을 위한 객체
- DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체
- 데이터가 네트워크를 통해 전송되는 방법을 정의하는 객체
- interface나 class를 이용해서 정의 (Nest에서는 Class 추천)
  - Class는 ES6 표준의 일부로 컴파일된 JS에서 실제 엔티티로 유지
  - Interface는 TS의 일부로 트랜스 파일 중에 제거되어 Nest에서 참조 불가
  - 파이프 같은 기능을 런타임에서 사용할 수 있으므로 런타임 시 사용가능한 Class 추천
- DTO 사용 이유
  - 효율적인 데이터 유효성 체크 (*NestJS*에서 `class-validator` 사용)
  - 코드를 더 안정적으로 만들어 줌
  - Typescript의 타입으로도 사용됨

<br>

```ts
import { IsNumber, IsString } from 'class-validator';

export class CreateMovieDto {
  @IsString()
  readonly title: string;
  @IsNumber()
  readonly year: number;
  @IsString({ each: true })
  readonly genres: string[];
}
```

---

<br>

## **Entity, DTO 분리 이유**

- View / DB Layer의 역할을 철저히 분리하기 위함
- Entity 클래스는 실제 테이블과 매핑되어 변경 시 여러 클래스에 영향
  - DTO는 View와 통신하는 부분은 자주 변경되므로 분리

<br>

### **Entity는 DB와 1:1 매칭이 되는 class이므로 자주 변경되는 것을 지양**

### **DTO는 통신간 데이터 형태를 정의한 것으로 수시로 변경될 수 있음**

### **따라서 Entity와 DTO 분리해야 함**
