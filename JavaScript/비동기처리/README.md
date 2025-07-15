## 동기와 비동기

`동기식 처리 모델(Synchronous processing model)`은 태스크(task)를 **직렬적으로 순차 실행**한다.  
즉, 하나의 작업이 끝나기 전까지 **다음 작업은 대기**하게 된다.

반면 `비동기식 처리 모델(Asynchronous processing model)` 또는 Non-Blocking 모델은  
**병렬적으로 작업을 처리**하며, **기존 작업의 완료 여부와 관계없이** 다음 작업을 실행한다.

---

### 전통적인 콜백 패턴이 가진 단점?

#### 단점 1. 콜백 헬 (Callback Hell)

비동기 함수의 결과를 가지고 **또 다른 비동기 함수를 호출해야 하는 경우**,  
함수 호출이 중첩(nesting)되어 **가독성이 떨어지고**,  
복잡도가 높아져 **실수와 에러 처리의 어려움**이 생긴다.

> 콜백 헬 예시

```javascript
step1(function (value1) {
  step2(value1, function (value2) {
    step3(value2, function (value3) {
      step4(value3, function (value4) {
        step5(value4, function (value5) {
          // value5를 사용하는 처리
        });
      });
    });
  });
});
```

#### 단점 2. 에러 처리의 한계

```javascript
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  console.log("에러를 캐치하지 못한다..");
  console.log(e);
}
```

위 코드는 `setTimeout` 내부에서 예외가 발생하지만, `try...catch`로 **에러를 잡지 못한다.**

- 이유는 예외는 **호출자 방향**으로 전파되는데, `setTimeout` 콜백 함수는 비동기적으로 실행되므로  
  `try` 블록을 벗어나게 된다.
- 따라서 콜백 내부의 에러는 `catch`로 잡히지 않고, 프로세스를 종료시키게 된다.

---

## 프로미스란?

자바스크립트는 전통적으로 **비동기 처리를 위해 콜백 함수**를 사용해 왔다.  
하지만 콜백 패턴은 **콜백 헬, 에러 처리 한계, 병렬 처리의 어려움** 등의 문제를 가진다.

이를 보완하기 위해 **ES6부터 도입된 비동기 처리 패턴**이 바로 **Promise**이다.

Promise는 **비동기 처리의 시점을 명확하게 표현**할 수 있어  
복잡한 비동기 작업을 더 구조적으로 관리할 수 있다.

> 실제로 대부분의 DOM 이벤트, setTimeout / setInterval, Ajax 요청 등은 비동기적으로 동작한다.

---

### 프로미스를 적용해보자

Promise는 `new Promise()` 생성자 함수로 만들 수 있으며,  
비동기 작업을 수행할 **콜백 함수**를 인자로 받고, 그 콜백 함수는 `resolve`, `reject`를 전달받는다.

```javascript
const promise = new Promise((resolve, reject) => {
  // 비동기 작업 수행

  if (/* 성공 */) {
    resolve('result');
  } else {
    reject('failure reason');
  }
});
```

---

### Promise의 상태 (State)

Promise는 비동기 처리의 상태를 나타내는 값을 갖는다.

| 상태        | 의미                                            | 설명                                              |
| ----------- | ----------------------------------------------- | ------------------------------------------------- |
| `pending`   | 아직 처리되지 않은 상태                         | `resolve` 또는 `reject`가 호출되지 않은 초기 상태 |
| `fulfilled` | 비동기 처리가 성공적으로 완료된 상태            | `resolve`가 호출된 상태                           |
| `rejected`  | 비동기 처리가 실패한 상태                       | `reject`가 호출된 상태                            |
| `settled`   | 성공 또는 실패 여부와 관계없이 처리 완료된 상태 | `resolve` 또는 `reject`가 호출된 상태             |

---

### 프로미스의 후속 처리 메소드

비동기 함수가 Promise를 반환하면,  
해당 Promise는 후속 처리 메소드(`then`, `catch`)를 통해 결과값 또는 에러를 처리할 수 있다.

- `then()`

  - 첫 번째 인자: `resolve` 시 실행할 콜백
  - 두 번째 인자: `reject` 시 실행할 콜백
  - `.then()`은 **새로운 Promise를 반환**한다.

- `catch()`
  - `.then()`에서 발생한 에러 포함, 전체 비동기 처리 중 발생한 에러를 처리
  - `catch()`도 **Promise를 반환**한다.

---

> ### 참고: then 대신 catch 사용을 권장하는 이유

```javascript
promiseAjax("https://jsonplaceholder.typicode.com/todos/1")
  .then((res) => console.xxx(res)) // ❌ 의도적 에러
  .catch((err) => console.error(err)); // ✅ catch가 모든 에러를 잡아줌
```

- `.catch()`는 `reject`뿐 아니라 `.then()` 내부에서 발생한 에러도 모두 처리할 수 있다.
- `.then(성공, 실패)` 형태보다 `.then().catch()` 패턴이 **더 명확하고 가독성이 좋다**.

---

Promise를 통해 비동기 작업을 체계적으로 다루고,  
에러 처리를 안정적으로 구성할 수 있다는 점이 가장 큰 장점이다.

### 프로미스 심화(?)

#### 프로미스의 정적 메소드

`Promise`는 주로 생성자 함수로 사용되지만, 함수도 객체이기 때문에 자체 메소드를 가질 수 있다.  
`Promise` 객체는 다음과 같은 **4가지 정적 메소드**를 제공한다.

---

##### 1. Promise.resolve / Promise.reject

- `Promise.resolve(value)`  
  전달된 값을 resolve 상태로 감싸는 프로미스를 생성한다.

```javascript
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```

위 예제는 아래와 같은 코드와 동일하게 동작한다.

```javascript
const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]
```

- `Promise.reject(error)`  
  전달된 에러를 reject 상태로 감싸는 프로미스를 생성한다.

```javascript
const rejectedPromise = Promise.reject(new Error("Error!"));
rejectedPromise.catch(console.log); // Error: Error!
```

아래와 같은 코드와 동일한 동작을 한다.

```javascript
const rejectedPromise = new Promise((resolve, reject) =>
  reject(new Error("Error!"))
);
rejectedPromise.catch(console.log); // Error: Error!
```

---

##### 2. Promise.all

`Promise.all()`은 여러 개의 프로미스를 병렬로 실행하고,  
**모든 프로미스가 성공(resolve)** 되었을 때 **하나의 배열로 결과를 반환**한다.

```javascript
Promise.all([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
])
  .then(console.log) // [1, 2, 3]
  .catch(console.log);
```

- 각 프로미스는 병렬로 실행되며,
- 모든 작업이 성공했을 경우 배열로 결과를 반환하고,
- 하나라도 실패하면 `.catch()`로 에러를 처리한다.

---

##### 3. Promise.race

`Promise.race()`는 여러 개의 프로미스 중  
**가장 먼저 처리된 하나의 결과만 반환**하는 정적 메소드이다.

```javascript
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
  new Promise((resolve) => setTimeout(() => resolve(2), 1000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error("Fail")), 2000)),
])
  .then(console.log) // 2
  .catch(console.log);
```

- 위 예제에서는 `2`가 가장 빨리 resolve되기 때문에 해당 값이 출력된다.
- 만약 가장 먼저 끝난 프로미스가 실패(reject)했다면, 즉시 에러를 반환한다.
- `Promise.all`과 다르게 **하나만 완료되면 바로 반환**되는 점이 핵심이다.

---

## async와 await

`async`와 `await`는 프로미스를 **더 간결하고 직관적으로 사용할 수 있는 문법**이다.  
`promise.then()` 대신 `await`을 사용하면 **동기 코드처럼 작성 가능**해 가독성이 훨씬 좋아진다.

---

### async

`function` 앞에 `async` 키워드를 붙이면 다음 두 가지 특징이 생긴다.

1. 해당 함수는 무조건 **Promise를 반환**한다.
2. 함수 내부에서 **await 사용이 가능**해진다.

---

### await

`await`는 **Promise가 처리될 때까지 기다렸다가** 그 결과값을 반환한다.  
`then()`보다 훨씬 깔끔하고 이해하기 쉬운 방식이다.

```javascript
async function getData() {
  try {
    const response = await fetch("/api/data");
    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
```

- `await`는 비동기 처리의 **성공 결과는 그대로 반환**하고,
- **에러가 발생하면 예외(throw)** 로 이어지므로 `try/catch`로 감싸서 사용한다.

---

### async/await 정리

- `async/await`는 `promise.then/catch`보다 **가독성**이 좋고 **에러 처리**도 직관적이다.
- 다만 최상위 스코프에서는 `await`를 사용할 수 없으므로 이럴 땐 `then/catch`를 사용할 수밖에 없다.
- 여러 비동기 작업이 있을 때는 `Promise.all()`과 함께 사용하면 유용하다.

```javascript
async function loadAll() {
  const [data1, data2] = await Promise.all([fetchData1(), fetchData2()]);
}
```

> `async/await`도 결국 `Promise`를 기반으로 동작하므로, 그 구조를 이해하고 사용하는 것이 중요하다.

---

### 참고 && 읽을거리

> [동기식 처리 모델 vs 비동기식 처리 모델](https://poiemaweb.com/js-async)

> [프로미스 (callback 지옥에서 벗어나기)](https://poiemaweb.com/es6-promise)

> [async와 await](https://ko.javascript.info/async-await)

> [예제로 이해하는 async/await](https://docs.tosspayments.com/blog/async-await-example)
