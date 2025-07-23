> [리액트 쿼리](https://edu.nextstep.camp/s/0eoNzeZS/ls/4CIF8d7M)

현재 비동기 데이터를 효율적으로 관리하는 방법 - React Query

## React Query

React Query는 서버 상태를 관리하는 라이브러리로, 데이터 페칭, 캐싱, 동기화 및 서버 상태의 업데이트를 효율적으로 처리해줘요.

React Query를 사용하는 이유는 꼭 Redux를 대체하기 위함은 아니지만 오늘은 Redux에서 왜 React Query로 넘어가게 되었는지를 중심으로 이야기 해보려고 해요.

### 1. 서버 상태와 클라이언트 상태의 분리

과거에는 전역 상태 관리 라이브러리(예: Redux)를 사용해서 서버 상태와 클라이언트 상태를 모두 관리하는 것이 일반적이었어요. 하지만 서버 상태와 클라이언트 상태는 본질적으로 다르기 때문에 이를 분리해서 관리하는 것이 더 효율적이에요.

클라이언트 상태: 사용자 인터페이스의 일시적 상태를 관리해요. 예를 들어, 모달 창의 열림/닫힘 상태, 폼 입력값 등이 이에 해당해요.
서버 상태: 외부 서버에서 가져온 데이터를 관리해요. 예를 들어, 사용자 프로필 데이터, 게시물 리스트 등이 이에 해당해요.
React Query는 서버 상태 관리를 전문적으로 처리하는 라이브러리로, 서버 상태를 더 효율적으로 관리할 수 있게 해줘요.

### 2. 데이터 페칭 및 캐싱의 자동화

React Query는 데이터 페칭과 캐싱을 자동으로 처리해줘요. 이는 개발자가 수동으로 데이터 페칭 로직을 작성하고 캐시를 관리해야 하는 번거로움을 줄여줘요.

자동 캐싱: React Query는 데이터를 자동으로 캐싱하고, 동일한 요청이 다시 발생할 때 캐시된 데이터를 사용해 불필요한 네트워크 요청을 줄여요.
자동 리페칭: 데이터의 신선도를 유지하기 위해 특정 조건(예: 컴포넌트가 마운트될 때, 일정 시간 간격마다)에서 자동으로 데이터를 리페칭해요.

### 3. 복잡한 비동기 로직의 간소화

Redux와 같은 전역 상태 관리 라이브러리에서는 비동기 로직을 처리하기 위해 미들웨어(Redux Thunk, Redux Saga 등)를 사용해야 했어요. 이는 추가적인 설정과 코드 복잡도를 증가시켰어요.

React Query는 비동기 로직을 훨씬 간단하게 처리할 수 있는 훅(useQuery, useMutation)을 제공해요. 이를 통해 비동기 데이터 페칭과 변이 작업을 간단하게 처리할 수 있어요.

### 4. 옵티미스틱 업데이트와 실시간 데이터 처리

React Query는 옵티미스틱 업데이트를 통해 사용자 경험을 향상시킬 수 있어요. 서버에 데이터가 성공적으로 저장되기 전에 UI를 먼저 업데이트해 사용자에게 빠른 피드백을 제공해요.

또한, 실시간 데이터 업데이트를 쉽게 처리할 수 있는 기능을 제공해요. 폴링, 웹소켓 등을 통해 실시간 데이터를 간단하게 관리할 수 있어요.

### 5. 개발자 도구 지원

React Query는 강력한 개발자 도구(React Query DevTools)를 제공해요. 이를 통해 쿼리 상태, 캐시된 데이터, 리페칭 상태 등을 쉽게 확인하고 디버깅할 수 있어요. 이는 개발 생산성을 크게 향상시켜줘요.

### 6. 커뮤니티와 생태계의 성장

React Query는 점점 더 많은 개발자들이 채택하면서 커뮤니티와 생태계가 성장하고 있어요. 이는 더 많은 리소스, 예제, 플러그인 등을 사용할 수 있게 해줘요.

### 1. 설치

```
npm install @tanstack/react-query
// axios를 사용할 예정이라면 axios도 설치
```

### 2. React Query 설정

React Query를 사용하기 위해서는 QueryClient와 QueryClientProvider를 설정해야 해요. 이를 통해 React Query가 애플리케이션 전반에서 쿼리를 관리할 수 있게 해줘요.

index.tsx 파일이나 App.tsx 파일에 다음과 같이 설정해요.

import React from 'react';
import ReactDOM from 'react-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import App from './App';

const queryClient = new QueryClient();

ReactDOM.render(
<QueryClientProvider client={queryClient}>
<App />
</QueryClientProvider>,
document.getElementById('root')
);

## useQuery

useQuery 훅을 사용해서 데이터를 가져오고, 로딩 상태와 에러 상태를 관리해요.

```
  const { data, error, isLoading } = useQuery<WeatherData>({
    queryKey: ['weatherData'],
    queryFn: fetchWeather
  });
```

- ['weatherData']는 쿼리 키예요. 이 키를 사용해서 쿼리를 식별하고 캐시를 관리해요.
- fetchWeather는 데이터를 가져오는 비동기 함수예요.

> 주의, React Query는 Redux의 무조건적인 대체제는 아니에요!
> React Query가 많은 장점을 가지고 있지만, Redux를 완전히 대체할 수는 없어요. 두 라이브러리는 서로 다른 목적과 사용 사례를 가지고 있기 때문이에요.

## useMutation

`mutationFn`
반드시 필요. 실제 API 요청을 보내는 함수입니다. 보통 axios.post(...) 같은 함수
`onSuccess(data, variables, context)`
요청이 성공했을 때 실행할 콜백. 예: 알림 띄우기, 페이지 이동 등
`onError(error, variables, context)`
요청이 실패했을 때 실행할 콜백. 에러 토스트 띄우기 등에 사용
`onSettled(data, error, variables, context)`
성공하든 실패하든 항상 실행되는 콜백 (예: 로딩 종료 처리 등)
`mutationKey`
이 mutation을 구분하는 고유 키 (캐시나 Devtools에서 구분할 때 사용됨)

## useInfiniteQuery

### 무한스크롤 구현

`queryKey`
쿼리 식별자 (캐싱 키). 일반적으로 배열 형태
`initialPageParam`
useInfiniteQuery에서 페이지네이션을 시작할 때 사용할 초기 커서 값을 설정하는 옵션
`queryFn`
페이지 데이터를 가져오는 비동기 함수 (pageParam을 인자로 받음)
`pageParam`
현재 페이지 커서 값 (초기값은 0, 이후엔 getNextPageParam이 반환하는 값)
`getNextPageParam`
다음 페이지 커서를 추출하는 함수 (마지막 응답 기준으로 다음 요청의 커서 추출)

---

## React Query와 Redux의 차이점

### 목적

- `React Query`: 서버 상태 관리에 특화된 라이브러리로, 비동기 데이터 페칭, 캐싱, 리페칭 등을 자동으로 처리하는 데 중점을 두고 있어요. -`Redux`: 애플리케이션의 전역 상태를 관리하는 라이브러리로, 클라이언트 상태를 중앙 집중식으로 관리하고, 상태 변경을 예측 가능하게 만드는 데 중점을 두고 있어요.

### 사용 사례

- `React Query`: 주로 서버에서 데이터를 가져오고 이를 캐싱하고, 서버 상태를 최신으로 유지해야 하는 경우에 사용해요. 예를 들어, API 요청을 통해 데이터를 가져오고, 이를 여러 컴포넌트에서 재사용해야 할 때 유용해요.
- `Redux`: 클라이언트 상태를 중앙에서 관리하고, 상태 변경을 일관성 있게 추적해야 할 때 사용해요. 예를 들어, 사용자 인증 상태, UI 상태(모달 창 열림/닫힘), 폼 입력값 관리 등 클라이언트 측 상태 관리에 적합해요.

## 왜 React Query가 Redux를 완전히 대체할 수 없을까요?

### 1. 서버 상태와 클라이언트 상태의 차이

- React Query는 서버 상태 관리에 최적화되어 있으며, 클라이언트 상태 관리를 위해 설계되지 않았어요. 반면에 Redux는 클라이언트 상태 관리에 매우 유용해요.
- 애플리케이션에서는 서버 상태와 클라이언트 상태를 모두 관리해야 하는 경우가 많기 때문에, 두 라이브러리를 함께 사용하는 것이 효과적일 수 있어요.

### 2. 전역 상태 관리의 필요성

- Redux는 애플리케이션의 모든 상태를 중앙에서 관리할 수 있게 해줘요. 이는 대규모 애플리케이션에서 상태 관리의 일관성을 유지하고, 상태 변경을 추적하는 데 유용해요.
- React Query는 주로 서버에서 데이터를 가져오고, 이를 컴포넌트 간에 공유하는 데 집중하지만, 클라이언트 상태를 중앙에서 관리하는 데는 적합하지 않아요.

### 3. 미들웨어와 확장성

- Redux는 미들웨어(Redux Thunk, Redux Saga 등)를 통해 비동기 작업과 복잡한 상태 변경 로직을 처리할 수 있어요. 이는 다양한 확장성과 커스터마이징 가능성을 제공해요.
- React Query는 비동기 데이터 페칭에 특화되어 있지만, 복잡한 클라이언트 측 상태 로직을 처리하는 데는 한계가 있어요.
  정리
  React Query와 Redux는 서로 보완적인 도구예요. React Query는 서버 상태 관리에 특화되어 있어 비동기 데이터 페칭과 캐싱을 효율적으로 처리할 수 있고, Redux는 클라이언트 상태 관리에 적합해 전역 상태를 중앙에서 관리하고 상태 변경을 일관성 있게 추적할 수 있어요.
  따라서, 두 라이브러리를 함께 사용하면 애플리케이션의 상태 관리를 더욱 효과적으로 할 수 있어요. React Query가 서버 상태를 효율적으로 관리하는 동안, Redux는 클라이언트 상태를 중앙에서 일관성 있게 관리해 줘요.
