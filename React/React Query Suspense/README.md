---
## Suspense
Suspense는 React에서 비동기 컴포넌트를 렌더링 대기시키는 메커니즘.

```tsx
<Suspense fallback={<div>로딩중...</div>}>
<SomeComponent />
</Suspense>
```
SomeComponent에서 suspension(promise throw) 발생 시, fallback이 대신 보여져요.
React Query는 이를 활용해 로딩 상태를 자동 처리할 수 있어요.
---

## 명령형방식과 선언적방식

### Imperative (명령형) 방식:

```tsx
const [product, setProduct] = useState(null);
const [error, setError] = useState(null);
const [loading, setLoading] = useState(true);

useEffect(() => {
  fetch("/api/product/1")
    .then((res) => res.json())
    .then((data) => {
      setProduct(data);
      setLoading(false);
    })
    .catch((err) => {
      setError(err);
      setLoading(false);
    });
}, []);

if (loading) return <p>로딩중...</p>;
if (error) return <p>에러 발생</p>;
return <ProductView data={product} />;
```

### Declarative (선언적) 방식:

React Query + Suspense + ErrorBoundary 조합

```tsx
// 1. 에러 바운더리 정의
class MyErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  render() {
    if (this.state.hasError) {
      return <p>에러가 발생했습니다.</p>;
    }
    return this.props.children;
  }
}

// 2. Suspense를 통한 대기 처리 + useQuery 사용
import { useQuery } from "@tanstack/react-query";

function ProductComponent() {
  const { data } = useQuery({
    queryKey: ["product", 1],
    queryFn: () => fetch("/api/product/1").then((res) => res.json()),
    suspense: true, // 핵심!
  });

  return <ProductView data={data} />;
}

// 3. 구조적으로 선언
function Page() {
  return (
    <MyErrorBoundary>
      <React.Suspense fallback={<p>로딩중...</p>}>
        <ProductComponent />
      </React.Suspense>
    </MyErrorBoundary>
  );
}
```

---

## React Query에서 Suspense를 사용하는 이유

### 기존방식과의 차이

| 기존 방식 (`isLoading`)                       | Suspense 방식                                        |
| --------------------------------------------- | ---------------------------------------------------- |
| `isLoading`, `isFetching` 등의 상태 직접 관리 | `Suspense`가 로딩 UI 처리                            |
| `<>{isLoading ? <Spinner /> : <UI />}</>`     | `<Suspense fallback={<Spinner />}><UI /></Suspense>` |
| 로직이 컴포넌트마다 중복됨                    | 로딩 처리 일관성 증가                                |
| 조건부 렌더링 복잡                            | UI 구조가 간결해짐                                   |

---

## 사용방법

React Query에서 useSuspenseQuery를 사용.

```tsx
// weather api
interface WeatherData {
  name: string;
  main: {
    temp: number;
  };
  weather: {
    description: string;
  }[];
}

const fetchWeather = async (): Promise<WeatherData> => {
  const { data } = await axios.get(
    "https://api.openweathermap.org/data/2.5/weather?q=seoul&units=metric&lang=kr&appid=ce11b6c52899b8370598cc770b885399"
  );
  return data;
};
```

```tsx
// WeatherComponent (use useSusepnseQuery)

const WeatherComponent: React.FC = () => {
  const { data } = useSuspenseQuery<WeatherData>({
    queryKey: ["weatherData"],
    queryFn: fetchWeather,
  });

  return (
    <div>
      <h1>{data.name}</h1>
      <p>Temperature: {data.main.temp}°C</p>
      <p>Weather: {data.weather[0].description}</p>
    </div>
  );
};
```

```tsx
// WeatherPage (use Suspense)

const WeatherPage = () => {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <WeatherComponent />
    </Suspense>
  );
};
```

---

## useSuspenseQuery 주요 특징

기본적인 사용법은 useQuery와 동일해요.
다만 내부 동작 방식은 상이해요. Suspense 특성상 더이상 Loading이 불필요해지고, data에 값이 존재하는 것이 보장되기 때문에 사용하는 곳에서 data?.name 처럼 Optional Chaining을 할 필요가 없어요.

위와 같은 배경으로 useSuspenseQuery에는 useQuery에서 제공하던 일부 내용들을 제공하지 않아요.

## Suspense와 useSuspenseQuery를 사용하며 명심해야 할 것

### 폭포수 (waterfall)

Suspense의 동작 원리를 간단하게 짚어보면 promise throw를 만나면 다음 코드 읽는 것을 멈추고 Suspense fallback을 동작시켜요.
즉, 아래와 같이 useSuspenseQuery를 사용한다면?

````tsx
const { data: a } = useSuspenseQuery({});
const { data: b } = useSuspenseQuery({});
const { data: c } = useSuspenseQuery({});
const { data: d } = useSuspenseQuery({});

```a 요청이 끝난 후, b 요청, c 요청, d 요청 등 순차적으로 진행돼요.

위와 비슷하게 suspense 중첩이 발생하면 동일한 현상이 발생 할 수 있겠죠?

```tsx
const AComponent = () => {
  const { data: a } = useSuspenseQuery({});

  return (
    <suspense>
      <BComponent />
    </suspense>
  )
};

const BComponent = () => {
  const { data: b } = useSuspenseQuery({});

  return (
    <suspense>
      <CComponent />
    </suspense>
  )
};


const Main = () => {
  return (
    <suspense>
      <AComponent />
    </suspense>
  )
}

````

그렇기 때문에 이를 해결하기 위해 각각의 데이터가 어디서 사용되는지? 어떻게 Suspense 단위를 나눌지 고민이 필요해요.

1. 여러 컴포넌트에서 동일한 데이터가 필요한 경우.

- useSuspenseQueries

2. 각각의 컴포넌트에서 각기 다른 데이터가 필요한 경우

- suspense (청크) 단위를 잘 쪼개기

```tsx
<suspense>
  <AComponent />
</suspense>
<suspense>
  <BComponent />
</suspense>
<suspense>
  <CComponent />
</suspense>
```

Suspense 동작에 대해 잘 이해하고 싶다면?
React Fiber 에 대해 공부해보시면 좋아요.

## 참고링크

> [리액트홈페이지 Suspense](https://react.dev/reference/react/Suspense)

> [리액트홈페이지 ErrorBoundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

> [리엑트 Suspense 딥다이브](https://velog.io/@jay/Suspense)

---

> [(Slash 21) 프론트엔드 웹 서비스에서 우아하게 비동기 다루기](https://www.youtube.com/watch?v=FvRtoViujGg)

> [React에서 선언적으로 비동기 다루기](https://jbee.io/articles/react/React에서%20선언적으로%20비동기%20다루기)
