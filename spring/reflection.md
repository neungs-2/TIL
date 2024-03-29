# **Reflection API**

## **_리플렉션이란?_**

- 클래스 로더를 통해 읽어온 클래스 정보를 사용하는 기술
- 리플렉션을 사용해 클래스를 읽기, 인스턴스 생성, 메소드 실행, 필드 값 가져오기/변경 가능

<br>

## **_사용하는 경우_**

- 일반적으로 JVM에서 실행되는 애플리케이션의 런타임 동작을 검사하거나 수정하는 기능이 필요한 프로그램에서 사용
- 특정 어노테이션이 붙어있는 필드 또는 메소드 읽어오기 (JUnit, Spring)
- 특정 이름 패턴에 해당하는 메소드 목록 가져와 호출하기 (getter, setter)
- **실제로 코드 작성 시 리플렉션을 쓸 일은 많지 않음**
- 애플리케이션 개발보다 **프레임워크나 라이브러리**에서 많이 사용
  - 사용자가 어떤 클래스를 만들지 예측할 수 없기 때문
  - 규모가 큰 개발 단계에서는 수많은 객체와 의존관계를 파악하기 어려움
    - 이때 리플렉션을 사용하면 동적으로 클래스를 만들어서 의존 관계를 맺어줄 수 있음
- IDE 자동완성 기능을 위해서도 많이 사용
- ***예시***
  - **Spring**의 **Bean Factory**에서 어노테이션을 붙이면 클래스를 생성 및 관리해줌
  - 해당 클래스를 알려주지 않았지만 리플랙션을 통해 해당 클래스의 인스턴스를 동적 생성을 했기 때문

<br>

## **_Reflection 장단점_**

***장점***
- 런타임 시점에서 클래스의 인스턴스를 생성하고 접근 제어자와 관계 없이 필드와 메소드에 접근하여 필요한 작업을 수행할 수 있는 유연성

<br>

***단점***
- 리플렉션은 강력한 기능이지만 무분별한 사용을 지양해야 함
- **성능 오버헤드**
  - 리플렉션은 동적으로 해석되는 타입을 포함하므로 특정 JVM 최적화를 수행할 수 없음
- **보안 제한**
  - 리플렉션에는, 보안 관리자에서 실행할 때 존재하지 않을 수 있는 런타임 권한이 필요
- **내부 노출**
  - 리플랙션 사용 시, 예기치 못한 _side effect_ 발생할 수 있음
  - 리플렉션 코드는 추상화를 깨고 플랫폼 업그레이드에 따라 동작이 변경될 수 있음
  - 리플렉션 사용 시, 논리플렉션 코드에서는 부정한 동작이 발생할 수 있음
    - _private_ 필드 및 메서드에 엑세스
