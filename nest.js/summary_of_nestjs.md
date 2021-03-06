# Nest JS 개요

## **Nest JS 소개**

- 효율적이고 확장 가능한 _Node.js_ 서버 측 애플리케이션 구축을 위한 프레임워크
- **_Typescript_**를 기본적으로 지원하고 원한다면 Javascript도 가능
- **_OOP_, _FP_, _FRP_**의 요소를 결합

<br>

- **철학**
  - JS 서버 개발을 위한 도구 중 아키텍처 문제를 해결한 것이 없음
  - *Angular*에서 영감을 받았고 다음과 같은 아키텍처를 제공하고자 함
    - 테스트 가능성이 높고 확장 가능
    - 느슨하게 결합되며 유지 관리가 쉬우며 즉시 사용 가능

<br>

- **설치**

  - **_Node.js_**가 운영체제에 설치되어야 함<br>(단, 10.13.0 이하의 버전이나 13 버전은 제외)
  - **_Nest CLI_**를 사용하면 간단하게 프로젝트 생성 가능
    ```sh
    $ npm i -g @nestjs/cli
    $ nest new project-name
    ```
  - 생성되는 핵심 파일 개요

    |          파일명          | 설명                                                                               |
    | :----------------------: | :--------------------------------------------------------------------------------- |
    |   `app.controller.ts`    | 단일 경로가 있는 기본 컨트롤러                                                     |
    | `app.controller.spec.ts` | 컨트롤러에 대한 단위 테스트                                                        |
    |     `app.module.ts`      | 애플리케이션의 루트 모듈                                                           |
    |     `app.service.ts`     | 하나의 방법으로 기본 서비스를 제공                                                 |
    |        `main.ts`         | 핵심 함수 `NestFactory`를 사용하여 Nest 애플리케이션 인스턴스를 생성하는 항목 파일 |

<br>

---

## **Main.ts 파일**

```ts
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

- `main.ts`는 애플리케이션을 `bootstrap()`하는 비동기 함수를 포함
- 애플리케이션 인스턴스를 생성하기 위해 코어 `NestFactory` 클래스를 사용
- `NestFactory`의 정적 메서드 `create()`을 사용하여 어플리케이션 객체를 반환

> **_Tip_**<br>기본적으로 APP 생성 중 오류 발생 시 앱이 코드와 함께 종료됨 <br> 오류를 발생시키고 싶다면 `abortOnError`옵션을 비활성화 할 것 <br> `NestFactory.create(AppModule, { abortOnError: false })`
