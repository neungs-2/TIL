# **JAVA 사전 개념 정리**

## **class 파일**

![image](https://user-images.githubusercontent.com/60606025/183914221-22992f6c-6795-48f1-930e-ac7a91bea872.png)

- 개발자가 IDE(Integrated Development Environment)에서 구현한 `.java` 확장자를 갖는 소스코드는 기계가 해석할 수 없음
- JAVA는 **JVM**에 의해 프로그램이 동작되므로 JVM이 해석할 수 있는 **바이트코드**로 소스코드를 **컴파일**
- `.java` 파일을 바이트코드로 컴파일 시 `.class` 파일로 변환
- 컴파일은 **JDK**(Java Development Kit) 설치 시 `/bin` 폴더에 있는 **javac** 프로그램에 의해 수행

---

<br>

## **class 파일 적재**

- 자바 어플리케이션을 실행 시, 컴파일 된 `.class`파일들이 **클래스 로더**(class loader)에 의해 **JVM 메모리 중 메소드 영역에 적재**
