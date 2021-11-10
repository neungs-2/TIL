# 브라우저 동작 과정

<br>

### **Browser**

![브라우저의 동작 과정](https://user-images.githubusercontent.com/60606025/141062172-51a191cf-42fa-4898-acc4-0f5e32b4af7e.png)

- 웹 서버를 이동하며 쌍방향으로 통신하고 HTML 문서나 파일을 출력하는 GUI 기반의 응용 소프트웨어

---

<br>

## **브라우저의 기본 구조**

![브라우저 구조](https://d2.naver.com/content/images/2015/06/helloworld-59361-1.png)

**_User Interface_**

- 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분
- ex) 주소표시줄, 이전/다음 버튼, 북마크 메뉴 등

**_Browser Engine_**

- UI와 렌더링 엔진 사이의 동작을 제어

**_Rendering Engine_**

- 요청한 콘텐츠 표시
- HTML 요청 시 HTML, CSS 파싱하여 화면에 표시

**_Network_**

- HTTP 요청 같은 네트워크 호출에 사용

**_JavaScript Interpreter_**

- JS 코드 해석 및 실행
- JS Engine이라고도 함

**_UI Backend_**

- 기본적인 위젯을 그리는 인터페이스

**_Data Storage_**

- Cookie, Local Storage 처럼 브라우저 메모리를 활용하여 데이터 저장

---

<br>

## **렌더링**

- 요청 받은 내용을 브라우저 화면에 표시
- HTML, XML 이미지를 표시하고 확장기능 사용 시 PDF 등의 문서도 표시 가능
- 엔진 종류
  - Webkit: 크롬, 사파리에서 사용
  - Gecko: 파이어폭스에서 사용

![웹킷 랜더링 동작과정](https://d2.naver.com/content/images/2015/06/helloworld-59361-3.png)
<br>웹킷 랜더링 동작 과정

![게코 랜더링 동작과정](https://d2.naver.com/content/images/2015/06/helloworld-59361-4.png)
<br>게코 랜더링 동작 과정

---

<br>

## **렌더링 엔진 동작 과정**

![렌더링 과정](https://gloriajun.github.io/assets/images/post/browser-rendering.png)

**_DOM(Document Object Model) Tree_**

- HTML 태그를 파싱하여 DOM 트리 구성

**_CSSOM(CSS Object Model) 생성_**

- 스타일 정보를 통하여 스타일 구조체를 생성
- 처리 단계
  - 브라우저 자체에 포함된 기본 스타일 정보
  - 사용자 정의 스타일
  - HTML 태그에 style 속성을 이용하여 정의된 인라인 스타일 정보

**_Render tree construction (렌더 트리 생성)_**

- 렌더링 트리는 페이지에 표시되는 모든 DOM 컨텐츠와 각 노드에 대한 모든 스타일 정보 포함
- DOM 트리의 루트에서 시작하여 각 노드에 일치하는 스타일 구조의 규칙을 붙이며 생성
- 화면에 표시되지 않는 노드는 랜더 트리에 포함하지 않음
  - \<head\>, \<title\>, \<script\>, display: none
  - visibility: hidden 은 렌더 트리에 포함
- 렌더 트리에서 각 노드는 frame, box로 부름

**_Layout of the render tree(렌더 트리 배치)_**

- 렌더 트리에서 엘리먼트 위치, 크기 정보를 계산하여 생성
- 이 과정에서 상대 값(%)은 절대 값(px 등)으로 변환

**_Painting the render tree(페인팅)_**

- 렌더 트리를 순회하며 페인트 함수를 호출하여 화면에 표함(픽셀로 변환)

---

<br>

## **브라우저 동작 과정 정리**

- 주소창에 URL 입력 시 **서버에 요청 전송**
- 해당 페이지에 여러 자원(text, image...) 전송
- 렌더링 엔진이 렌더 트리 생성
  - HTML 파싱 --> DOM 트리 생성
  - CSS 파싱 --> CSSOM 트리 생성
  - DOM, CSSOM 두 트리를 결합하여 렌더링 트리 생성
- 화면에 배치를 시작하고 UI 백엔드가 노드를 돌며 형상을 그림

---

<br>

**[출처]**

> - https://d2.naver.com/helloworld/59361<br>
> - https://gloriajun.github.io/frontend/2018/10/23/frontend-reflow-repaint.html
