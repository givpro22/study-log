## Custom Hook 만들기

React에서 Custom Hook은 공통된 상태 로직을 추출하고 재사용할 수 있도록 도와주는 훅입니다. 이를 활용하면 컴포넌트 간 중복 코드를 줄이고, 코드의 가독성과 유지보수성을 높일 수 있습니다.

## Custom Hook이란?

일반 함수처럼 생긴 React Hook입니다.
이름은 use로 시작하도록 하여 Hook 으로 인식하도록 합니다. 예: useFetch, useForm, useToggle
내부에서 다른 React Hook(useState, useEffect, useReducer 등)을 호출하여 상태 관리나 부수 효과를 처리합니다.
데이터를 캡슐화하거나 특정 동작을 쉽게 재사용하도록 설계됩니다.

## Custom Hook 작성 시 고려사항

- `use로 시작`
  React가 Hook으로 인식하기 위해 이름을 반드시 use로 시작해야 합니다. use를 사용하므로 Hook 규칙의 위반 여부를 자동으로 체크할 수 있습니다.

- `React Hook 규칙 준수`
  Custom Hook 내부에서도 React Hook 규칙을 따라야 합니다.
  최상위 레벨에서만 호출 (조건문, 반복문 내에서 호출 금지)
  React 컴포넌트 또는 다른 Hook 내부에서만 호출
  훅의 책임 분리
  한 Custom Hook은 하나의 책임만 가지도록 설계합니다. 예: useFetch는 데이터를 가져오는 역할에만 집중.

- `매개변수와 반환값 명확화`
  Custom Hook이 어떤 데이터를 소비하고, 어떤 값을 반환할지 명확히 설계합니다.

---

### (1) 값을 토글하는 Custom Hook

#### 목적 : true와 false 값을 쉽게 토글할 수 있도록 하는 Hook.

#### 구현

```tsx
import { useState } from "react";

function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = () => {
    setValue((prevValue) => !prevValue);
  };

  return [value, toggle];
}

export default useToggle;
```

#### 사용하기

```tsx
import React from "react";
import useToggle from "./useToggle";

function App() {
  const [isOn, toggleIsOn] = useToggle(false);

  return (
    <div>
      <p>The light is {isOn ? "On" : "Off"}</p>
      <button onClick={toggleIsOn}>Toggle Light</button>
    </div>
  );
}

export default App;
```

### (2) 데이터를 가져오는 Custom Hook

#### 목적 API 요청과 로딩 상태를 관리하는 Hook.

#### 구현

```tsx
import { useState, useEffect } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setIsLoading(true);
        const response = await fetch(url);
        if (!response.ok) throw new Error("Failed to fetch data");
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, isLoading, error };
}

export default useFetch;
```

#### 사용하기

```tsx
import React from "react";
import useFetch from "./useFetch";

function App() {
  const { data, isLoading, error } = useFetch(
    "https://jsonplaceholder.typicode.com/posts"
  );

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default App;
```

### (3) 폼 상태 관리 Custom Hook

#### 목적

여러 입력 필드를 쉽게 관리할 수 있도록 하는 Hook.
구현

```tsx
import { useState } from "react";

function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);

  const handleChange = (event) => {
    const { name, value } = event.target;
    setValues({ ...values, [name]: value });
  };

  const resetForm = () => {
    setValues(initialValues);
  };

  return { values, handleChange, resetForm };
}

export default useForm;
```

#### 사용하기

```tsx
import React from "react";
import useForm from "./useForm";

function App() {
  const { values, handleChange, resetForm } = useForm({
    username: "",
    email: "",
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(values);
    resetForm();
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Username:
        <input
          name="username"
          value={values.username}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Email:
        <input name="email" value={values.email} onChange={handleChange} />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default App;
```

---

## 커스텀 훅의 장점

- `코드 재사용성`
  커스텀 훅은 로직을 캡슐화하여 다양한 컴포넌트에서 재사용할 수 있습니다. 여러 컴포넌트에서 동일한 상태 관리나 로직이 필요할 때, 중복된 코드를 줄일 수 있습니다.

- `분리된 관심사`
  커스텀 훅을 사용하면 컴포넌트의 UI와 비즈니스 로직을 분리할 수 있습니다. 훅은 로직에만 집중하고, 컴포넌트는 UI 렌더링에만 집중할 수 있어 코드 가독성이 높아집니다.

- `독립적인 테스트 가능성`
  커스텀 훅은 독립적으로 테스트할 수 있습니다. 훅이 상태 관리나 API 호출 등의 로직을 처리하므로, 각 훅을 별도로 테스트할 수 있어 코드 품질을 유지할 수 있습니다.

---

"왜 이 부분을 분리했나요?"

"기능 추가를 하기 위해서 페이지에 직접 기능을 추가해봤으나, 코드의 응집도가 낮고 함수가 단일 기능을 하지 못하는 등 추후에 버그가 생길 우려가 있고 유지보수 시간이 결과적으로 더 높아질 것이라고 판단해서 커스텀 훅을 만들어 높은 레벨의 추상화 작업을 진행했습니다."

## 참고 && 읽어볼거리

> [React 공식: 커스텀 훅 만들기](https://react.dev/learn/reusing-logic-with-custom-hooks)

> [React Hooks 기본 개념](https://ko.legacy.reactjs.org/docs/hooks-intro.html)

> [Awesome Custom Hooks 모음](https://usehooks.com)

> [좋은 코드란 무엇일까 - jbee](https://jbee.io/articles/etc/좋은%20코드란%20무엇일까)

> [React와 춤을 ch10 Hook 심화 3. Custom Hook 만들기](https://wikidocs.net/273977)

> [리액트 커스텀 훅을 만들어보자.](https://velog.io/@vvsogi/리액트-커스텀-훅을-만들어보자)
