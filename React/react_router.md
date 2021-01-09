# React-Router

> - 개요
> - 설치
> - 사용

- [react-router :: 1장. 리액트 라우터 사용해보기](https://velopert.com/3417)

## 개요

- SPA(Single Page Application): 페이지가 하나인 애플리케이션
- React 등의 라이브러리/프레임워크를 이용해 렌더링을 브라우저가 담당하도록 하고, 필요한 정보만 표시
- 페이지를 새로고침하지 않아도 되어서 이득

### 라우팅 (Routing)

- 화면이 하나만 있는 것은 아님
- 라우팅: 다른 주소에 따라 다른 뷰를 보여주는 것
- react-router: 서드파티 리액트 라우팅 라이브러리

## 설치

```Bash
yarn add react-router-dom  # yarn
# npm i react-router-dom --save  # npm
```

## 사용

현재 필자의 GitHub Pages 구성 기준.

- src/routes.tsx

```TSX
import {
  BrowserRouter as Router,
  Switch,
  Route,
} from "react-router-dom";
import { Home, Resume } from "./pages";

const Routes = () => {
  return (
    <Router>
      <Switch>
        <Route path="/resume">
          <Resume/>
        </Route>
        <Route path="/">
          <Home/>
        </Route>
      </Switch>
    </Router>
  );
};

export default Routes;
```

- ``<Switch>``를 통해 URL에서 라우팅할 경로 결정
- ``<Router>`` 안의 ``<Route path="">``로 라우팅

- src/App.tsx

```TSX
import { Suspense } from "react";

import Routes from "./routes";
import "./utils/i18n";

const App = () => {
  return (
    <div className="App">
      <Suspense fallback={null}>
        <Routes/>
      </Suspense>
    </div>
  );
};

export default App;
```

- ``<Suspense fallback={}>`` 필요 (Suspense 상태? 더 공부해야겠다)

- src/components/Category.tsx

```TSX
import { Link } from "react-router-dom";
import { useTranslation } from "react-i18next";

import "../styles/category.css";

const Category = () => {
  const { t } = useTranslation();
  return (
    <div className="App-content category">
      <p>{t('category.title')}</p>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/resume">Resume</Link></li>
      </ul>
    </div>
  );
};

export default Category;
```

- ``<Link to="">``를 통해 다른 페이지로 이동할 링크 제공 가능

- 이슈: /resume로 접속하였을 때 404 발생. 해결할 방법은 없을까?
