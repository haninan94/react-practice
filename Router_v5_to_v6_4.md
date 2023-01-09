# 2023-01-09 TIL

## React Router 관리 v.6.4 이전

src

┝━ index.js

┕━ App.js

- 기존에는 index.js 에서 app.js 컴포넌트를 import 한후에 App컴포넌트내에 Router관리를 래 코드와 같이 진행했습니다.

```jsx
// App.js

import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

import Home from "./routes/Home";
import Detail from "./routes/Detail";
import router from "./Router";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/movie/:id" element={<Detail />}></Route>
        <Route path="/" element={<Home />}></Route>
      </Routes>
    </Router>
  );
}

export default App;
```

## React Router 관리 v.6.4 이후

src

┝━ index.js

┝━ App.js

┕━ Router.js

- 이전 버전과 다르게 RouterProvider 로 별도로 Router.js 파일을 관리합니다.
- router 를 계층관리할 수 있는 children 기능은 별도 추후 추가 예정

```jsx
// App.js

import { RouterProvider } from "react-router-dom";
import router from "./Router";

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

```jsx
// Router.js

import { createBrowserRouter } from "react-router-dom";

import Home from "./routes/Home";
import Detail from "./routes/Detail";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />,
  },
  {
    path: "/movie/:id",
    element: <Detail />,
  },
]);

export default router;
```
