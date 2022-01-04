# Prop 'className' did not match Error

<br>

## **문제**

- `Next js`, `Material UI`로 작성한 페이지 새로고침 시 style 적용 안되는 오류 발생
- **Error Message**(브라우저 개발자 도구)
  <br>
  `Warning: Prop `className` did not match. Server: "sc-xxxxxx xxxxx" Client: "sc-yyyyyy yyyyy"`

<br>

**_문제 원인_**

- **babel-plugin-styled-components**가 없기 때문
- Babel 플러그인은 환경과 상관없이 일관된 className을 생성<br>(consistently hashed component classNames between environments)
- styled-components는 styled 함수로 만든 컴포넌트마다 generateId 함수를 이용해 유일한 식별자를 생성
- **컴포넌트가 생성되는 순서에 따라 같은 컴포넌트이더라도 다른 식별자가 붙을 수 있음**
  - SSR과 CSR을 같이 활용 시 서버와 클라이언트가 컴포넌트를 생성하는 순서에 따라 식별자가 달라질 수 있음
- **babel-plugin-styled-components**는 이런 식별자 생성 과정을 **정규화**

---

<br>

## **해결 방법**

- 플러그인 설치<br> `npm i --save-dev babel-plugin-styled-components`
- 프로젝트 루트의 **.babelrc**에 코드 추가 <br>`"plugins": ["babel-plugin-styled-components"]`
- **page/\_document.js** 파일 생성 후 다음 코드 삽입<br>

```js
import React from 'react';
import Document, {
  DocumentContext,
  Head,
  Html,
  Main,
  NextScript,
} from 'next/document';
import { ServerStyleSheets } from '@material-ui/styles';

class AppDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const sheet = new ServerStyleSheets();
    const originalRenderPage = ctx.renderPage;

    ctx.renderPage = () =>
      originalRenderPage({
        enhanceApp: (App) => (props) => {
          return sheet.collect(<App {...props} />);
        },
      });

    const initialProps = await Document.getInitialProps(ctx);
    return {
      ...initialProps,
      styles: (
        <>
          {initialProps.styles}
          {sheet.getStyleElement()}
          {/*{sheetStyled.getStyleElement()}*/}
        </>
      ),
    };
  }

  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default AppDocument;
```
