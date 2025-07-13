레이아웃과 라우터를 **어떻게 구조화하면 재사용성과 유지보수성이 좋을지**에 대해 정리해보았다.  
특히 **페이지마다 공통되는 Navigation 바를 어떻게 깔끔하게 관리할지**,  
**Layout을 어떻게 설계할지**가 고민이었고, 이번 기회에 스스로 정리하고자 한다.

---

## 과정

1. 현재 진행 중인 카카오테크 캠퍼스에서 멘토님의 코드 분석
2. Layout을 가독성 좋고 재사용성 높게 구성하는 구조 연구

---

## 공통 네비게이션 바 관리

![상단 네비게이션 바](https://velog.velcdn.com/images/givpro22/post/cf1403b0-848e-48d5-8b60-8ca81ae80376/image.png)

위 사진처럼, **페이지마다 항상 보여지는 상단 Navigation 바**가 있다.  
하지만 그 안의 내용은 페이지마다 다를 수 있으므로,  
**컴포넌트로 만들어서 재사용 가능한 구조로 관리**하는 것이 중요하다.

---

## App.tsx 구조

```tsx
const App = () => {
  return (
    <BrowserRouter>
      <ThemeProvider theme={theme}>
        <BaseLayout header={<Navigation />}>
          <Routes />
        </BaseLayout>
      </ThemeProvider>
    </BrowserRouter>
  );
};
```

- 핵심은 `BaseLayout`에 `header`라는 이름으로 Navigation을 넘긴 부분
- 이렇게 넘기면 Layout 내부에서 header 유무에 따라 조건부 렌더링이 가능해진다

---

## BaseLayout.tsx 구조

```tsx
export const BaseLayout = ({ maxWidth = 720, header, children }: Props) => {
  return (
    <Wrapper>
      {header && <FixedHeader maxWidth={maxWidth}>{header}</FixedHeader>}
      <Container maxWidth={maxWidth} hasHeader={!!header}>
        {children}
      </Container>
    </Wrapper>
  );
};
```

- `header &&` 조건문으로 header가 있을 때만 렌더링
- 유동적으로 header 유무를 판단해 재사용성 높은 레이아웃 구현

```tsx
const Container = styled.div<ContainerProps>(
  ({ maxWidth, hasHeader, theme }) => ({
    maxWidth: `${maxWidth}px`,
    width: "100%",
    minHeight: "100vh",
    height: "100%",
    backgroundColor: theme.colors.semantic.background.default,
    paddingTop: hasHeader ? "2.75rem" : "0",
  })
);
```

- `paddingTop: hasHeader ? '2.75rem' : '0'`  
  → header가 있으면 그만큼의 공간을 아래에 확보해준다

---

## 느낀 점

```tsx
<BaseLayout header={<Navigation />}>
```

이런 식으로 컴포넌트를 **props로 넘겨서 재사용**하는 방식이 신선했다.  
Prop Drilling처럼 복잡해질 수 있지만, **가독성 높고 명확하게 컴포넌트를 구성**할 수 있어 좋았다.

---

## Navigation.tsx 구조 분석

```tsx
export const Navigation = () => {
  const navigate = useNavigate();
  const location = useLocation();

  const handleBackClick = () => {
    navigate(-1);
  };

  const isLoginPage = location.pathname === ROUTE_PATH.LOGIN;

  return (
    <BaseNavigation
      left={
        <button onClick={handleBackClick}>
          <ChevronLeft size={28} strokeWidth={1.8} />
        </button>
      }
      center={
        <Link to={ROUTE_PATH.HOME}>
          <Logo src={logo} alt="카카오 선물하기 로고" />
        </Link>
      }
      right={
        isLoginPage ? (
          <UserRound />
        ) : (
          <Link
            to={`${ROUTE_PATH.LOGIN}?redirect=${encodeURIComponent(
              window.location.pathname
            )}`}
          >
            <UserRound />
          </Link>
        )
      }
    />
  );
};
```

- `BaseNavigation` 컴포넌트에 `left`, `center`, `right`로 나눠서 props 전달
- 덕분에 **화면에 따라 버튼이나 구성 요소를 유동적으로 변경 가능**
- 이 방식도 재사용성 높은 패턴이라고 느껴졌다

---

## -> 다음에 정리할 내용

## 라우트 경로 상수화

```tsx
import { ROUTE_PATH } from "@/pages/Routes";

export const ROUTE_PATH = {
  HOME: "/",
  LOGIN: "/login",
  NOT_FOUND: "*",
};
```

### 이렇게 경로를 상수로 관리하는 장점

- 경로가 변경되었을 때 **한 군데만 수정하면 전체 반영 가능**
- 하드코딩된 문자열(`/login`, `/home`)을 줄여 **오타 방지**
- 코드 가독성과 유지보수성이 훨씬 좋아짐

```tsx
const navigate = useNavigate();
const location = useLocation();
```

위 코드는 `react-router-dom`에서 제공하는 훅으로  
라우팅 관련 동작을 구현할 때 자주 사용된다.

다음은 `useNavigate`, `useLocation` 등  
React Router의 핵심 기능들에 대해 정리할 계획이다.
