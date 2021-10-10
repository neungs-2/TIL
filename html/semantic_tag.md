# Semantic tag

태그 이름 자체만으로 의미를 전달할 수 있는 태그

<br>

- 종류
  - header
  - nav
  - section
  - article
  - aside
  - footer

<br>

### **_header_**

- 머릿말, 제목, 상단 영역을 표현하기 위한 태그

<br>

### **_nav_**

- 콘텐츠를 담고 있는 문서를 연결하는 링크 역할을 담당
- 주로 메뉴에 사용

<br>

### **_main_**

- 문서의 메인 콘텐츠 지정 시 사용

### **_section_**

- 본문 콘텐츠를 담음
- 서로 관계 있는 문서를 분리하는 역할
- 문서를 다른 주제로 구분짓기 위해 사용
- block 속성

<br>

### **_article_**

- 내용이 독립적인 콘텐츠 담음
- 주로 기사, 블로그 글 등
- block 속성

<br>

### **_aside_**

- 본문 이외의 내용을 담고 있는 태그
- 주로 본문 옆에 광고나 링크
- 사이드바

<br>

### **_footer_**

- 화면 구조 중 제일 아래에 위치
- 회사소개/ 저작권/ 약관/ 제작정보 위치
- 연락처는 \<adress> 태그 사용하여 표시

<br><br>

## **Article vs Section**

- 컨텐츠가 독립된 자체만으로 의미가 통하면 \<article>
- 컨텐츠가 주제와 관련된 것 \<section>
- 서로 관련된 내용을 구분 \<section>
- 의미적으로 관계없는 내용 구분 \<div>

```html
<article>
  <h1>Heading</h1>
  <section>
    <h2>Section Heading</h2>
    <p>Content...</p>
  </section>
  <section>
    <h2>Section Heading</h2>
    <p>Content...</p>
  </section>
  <section>
    <h2>Section Heading</h2>
    <p>Content...</p>
  </section>
</article>
```

<br>

## Other semantic tags

- details : 추가적인 세부사항을 기재 (사용자에게 노출 or 숨김)
- figure : 일러스트, 다이어그램, 사진 등
- figcaption : figure 요소 캡션 정의
- mark : 표시/강조된 텍스트 정의
- summary : details 요소에 대한 표시 가능한 제목 정의
- time : 날짜/시간 정의
