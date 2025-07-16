## 라우팅(Routing)이란?

웹 애플리케이션을 개발할 때, **라우팅(Routing)**이란  
특정 경로로 들어오는 요청을 **어떻게 처리할지 결정하는 방식**을 말합니다.

```html
<nav>
  <ul>
    <li><a href="/">카카오 테크 월드</a></li>
    <li><a href="/jun">준 월드</a></li>
    <li><a href="/poco">포코 월드</a></li>
    <li><a href="/gongwon">공원 월드</a></li>
  </ul>
</nav>
```

![](https://velog.velcdn.com/images/givpro22/post/cedd57e5-9205-4b1a-8736-af1b0f9958b9/image.png)

### 라우팅을 위해 필요한 것들

라우팅이 어떤 개념인지 이해했다면,
이제 실제로 라우팅을 구현하기 위해 어떤 작업이 필요한지 정리해보겠습니다.

대표적으로 다음과 같은 세 가지 요소가 필요합니다. 1. 요청받은 URI를 분석하는 로직 2. 경로별로 어떤 화면을 보여줄지 연결하는 설정 3. URL 변경 내역을 관리하는 히스토리 관리 기능

> 참고 - URI의 구성
> ![](https://velog.velcdn.com/images/givpro22/post/ef2c36a3-c27f-4d70-abfd-89b945dfd2fa/image.png)

---

---

## React에서 Router 사용하기

![](https://velog.velcdn.com/images/givpro22/post/7719e3fd-4286-4371-a4b8-1cbd83243a22/image.png)

원래 라우팅은 앞서 살펴본 것처럼 **서버에서 처리하는 작업**이었습니다.  
하지만 **SPA(Single Page Application)**의 등장으로,  
**클라이언트 측에서 자체적으로 라우팅 시스템을 구성해 사용하는 방식**이 함께 사용되기 시작했습니다.

SPA에서 라우팅을 사용하는 경우,  
초기 진입 시 애플리케이션에 포함되어 전달되는 리소스들 안에  
**라우팅 처리를 담당하는 클라이언트 라우터**가 포함되어 있습니다.

이후 사용자가 경로를 이동하는 등의 작업을 수행하면,  
**서버로 요청을 보내는 대신 클라이언트 라우터가 직접 경로 변경을 처리**하게 됩니다.

---

### React-Router? React-Router-Dom?

#### React Router

- React Router DOM의 상위 패키지이자 라우팅을 위한 핵심 패키지
  브라우저 기반의 라우팅을 처리하며, 웹 애플리케이션에서 사용됩니다.
  주요한 컴포넌트로는 Router, Route, Switch, Link, Redirect 등이 있습니다.

#### React Router DOM

-> React Router의 확장버전

- 웹 애플리케이션을 위한 추가 컴포넌트를 제공 (BroswerRouter, NavLink 등) React Router의 기능을 포함하면서 브라우저에서 동작하는 기능을 지원

---

## React의 라우팅 시스템

React는 기본적으로 라우팅 기능을 내장하고 있지 않아요.  
따라서 클라이언트 라우팅을 구현하기 위해 **서드파티 라이브러리**를 사용하는 것이 일반적입니다.

그중 대표적인 라이브러리가 바로 **react-router**입니다.

---

#### React Router 주요 패키지

`react-router` : 웹과 앱 모두에서 사용할 수 있는 기본 라우터 라이브러리
`react-router-dom` : 웹 전용 라우터 라이브러리
`react-router-native` : 모바일 앱 전용 라우터 라이브러리

#### react-router

react-router는 SPA 환경에서 라우팅을 쉽게 구현할 수 있게 도와주는 라이브러리예요.

![](https://velog.velcdn.com/images/givpro22/post/8b0c520b-5eb5-4a3b-93a9-5b2ec3ef3484/image.png)

> [React-Router 공식 홈페이지](https://reactrouter.com)

설치

```
npm install react-router-dom
```

---

### 종류

react-router에는 크게 두 가지 방식이 있습니다:

- **BrowserRouter**:  
  HTML5 History API를 사용하여 브라우저 주소를 감지합니다. (일반적인 웹 주소 형태)

- **HashRouter**:  
  주소에 `#`(해시)를 포함한 방식으로 라우팅합니다. (예: `/#/home`)  
  구형 브라우저에서도 동작이 보장되긴 하지만, 최근에는 거의 사용되지 않아요.

---

## BrowserRouter 방식

BrowserRouter 기준으로 라우터를 설정하는 방법은 크게 **2가지 방식**이 있습니다.

### 1. `createBrowserRouter` 방식

```tsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

const router = createBrowserRouter([
  {
    path: "/",
    element: <MainPage />,
  },
  {
    path: "/jun",
    element: <JunWorld />,
  },
  // ...
]);

<RouterProvider router={router} />;
```

    •	라우팅을 객체 배열로 정의할 수 있어 JSON 형태로 관리가 용이함
    •	코드 스플리팅 등 다양한 고급 기능과 궁합이 좋음
    •	대규모 프로젝트에서 라우팅 로직을 분리하고 싶을 때 유용함

### 2. BrowserRouter 방식

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

<BrowserRouter basename="/app">
  <Routes>
    <Route path="/" element={<MainPage />} />
    <Route path="/jun" element={<JunWorld />} />
    {/* ... */}
  </Routes>
</BrowserRouter>;
```

    •	JSX 안에서 직접 라우트를 정의하는 방식
    •	컴포넌트 안에서 라우팅 구조를 바로 볼 수 있어 직관적
    •	작은 규모의 프로젝트나 빠른 프로토타입 개발에 적합

---

## 직접 사용해보자

👉 본문에서는 실무에서 더 자주 사용되는 **BrowserRouter 기준으로 설명**합니다.  
왜냐하면 실제 웹사이트 주소 구조와 동일하게 동작하기 때문에 사용자 경험이 더 자연스럽기 때문이에요.

### 기본 세팅 (모듈 3가지)

`BrowserRouter` : 라우팅을 설정하고 URL을 관리하는 최상위 컴포넌트
`Routes` : 여러 Route를 포함하여 경로에 따라 렌더링할 컴포넌트를 결정
`Route` : 특정 경로에 대한 컴포넌트를 정의하는 컴포넌트

```tsx
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* 
Routes안에 이렇게 작성합니다. 
Route에는 react-router-dom에서 지원하는 props들이 있습니다.

path는 우리가 흔히 말하는 사용하고싶은 "주소"를 넣어주면 됩니다.
element는 해당 주소로 이동했을 때 보여주고자 하는 컴포넌트를 넣어줍니다.
                 */}
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};
```

### `<BrowserRouter>`

라우팅을 설정하고 URL을 관리하는 **최상위 컴포넌트**입니다.  
브라우저의 주소 표시줄과 애플리케이션의 라우트 상태를 **동기화**합니다.

속성 `basename`

모든 경로 앞에 붙는 **기본 경로**를 설정할 수 있습니다.  
예: `basename="/app"` → 모든 경로가 `/app`으로 시작됩니다.

---

### `<Routes>`

여러 개의 `<Route>`를 감싸서  
**경로에 따라 올바른 컴포넌트를 렌더링**합니다.

---

### `<Route>`

특정 경로(`path`)와 해당 경로에 매핑할 컴포넌트(`element`)를 지정합니다.  
경로는 **위에서 아래로 순차적으로 비교**됩니다.

#### 주요 속성

| 속성명          | 설명                                   |
| --------------- | -------------------------------------- |
| `path`          | 경로 설정                              |
| `element`       | 해당 경로에서 렌더링할 컴포넌트        |
| `index`         | 기본 경로(`/`)일 때 렌더링할 컴포넌트  |
| `caseSensitive` | 경로 대소문자 구분 여부                |
| `errorElement`  | 라우팅 중 에러 발생 시 보여줄 컴포넌트 |

> `path` 작성 방식

- 명시적 경로: `/index`
- 와일드카드: `*`, `/index/*`
- 옵셔널: `/index/test?`
- 파라미터: `/index/:id`

---

### 중첩 라우트

- 경로가 비슷한 경우 중첩 라우트를 사용하면  
  **중복을 줄이고 효율적으로 관리**할 수 있습니다.

- 중첩 라우트는 부모 `<Route>` 안에 자식 `<Route>`를 중첩하여 정의합니다.  
  `<Outlet />` 컴포넌트를 통해 자식 라우트를 렌더링할 위치를 지정합니다.

#### `<Outlet />`

- 중첩된 자식 라우트를 표시하는 **렌더링 위치**를 지정합니다.  
  부모 `<Route>`의 `element` 속성 안에 포함되어야 하며  
  부모는 자식보다 **상단에 정의**되어야 합니다.

> 사용 예시

```tsx
<Route
  path="/board"
  element={
    <>
      <header>헤더</header>
      <Outlet /> {/* 중첩된 컴포넌트 표시 */}
      <footer>푸터</footer>
    </>
  }
>
  <Route path="write" element={<div>Write</div>} /> {/* /board/write */}
  <Route path="read" element={<div>Read</div>} /> {/* /board/read */}
</Route>
```

---

### `<Link>`

`<Link>`는 페이지를 새로 고침하지 않고 경로만 변경합니다.

- 페이지를 새로고침하지 않고 클라이언트 라우팅 수행
- `<BrowserRouter>`의 `basename`은 자동으로 처리되므로, `<Link>`에서는 별도로 신경 쓸 필요 없음

#### 주요 속성

| 속성                 | 설명                                                        |
| -------------------- | ----------------------------------------------------------- |
| `to`                 | 이동할 경로 설정                                            |
| `relative`           | 경로를 절대 경로(`'path'`) 또는 상대 경로(`'route'`)로 설정 |
| `preventScrollReset` | 페이지 이동 시 스크롤 위치가 초기화되지 않도록 설정         |

> 사용 예시

````jsx
<Link to="/board/write">작성</Link>
// 절대 경로
<Link to="write" relative="route">작성</Link>
// 상대 경로
<Link to="write" relative="route" preventScrollReset>작성</Link>
// 상대 경로 + 스크롤 위치 유지

---

### `<NavLink>`

현재 경로에 따라 **동적 스타일링이 가능한 링크 컴포넌트**입니다.
`<Link>`와 유사하지만, **활성화된 경로**에 따라 스타일을 동적으로 변경할 수 있는 점이 특징입니다.


#### 차이점

- `style`과 `className` 속성에 **함수를 전달**할 수 있습니다.
- 이를 통해 경로 상태에 따라 다양한 스타일을 적용할 수 있습니다.


#### `<NavLink>` 주요 속성

| 속성               | 설명 |
|--------------------|------|
| `isActive`         | 현재 경로가 활성화된 상태인지 확인 |
| `isPending`        | 경로가 로드 중인지 확인 |
| `isTransitioning`  | 경로 전환 중인지 확인 |
| `end`              | `to` 속성과 정확히 일치할 경우에만 활성화 상태로 간주 (부분 일치 방지) |

>예시 코드
```jsx
<NavLink
  to="read"
  style={({ isActive }) => ({
    color: isActive ? 'red' : 'black'
  })}
>
  읽기
</NavLink>
// 현재 경로가 'read'일 경우 빨간색으로 표시됨



---


### 참고

>[[React] 리액트 라우터 돔(React Router DOM) - BrowserRouter, Routes, Route, Outlet, Link, NavLink](https://creative103.tistory.com/235)

> 카카오테크 캠퍼스 3기 프론트엔드 2단계 React-Router-Dom



````
