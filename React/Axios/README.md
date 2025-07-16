## Axios - 가장 많이 사용되는 비동기 처리 라이브러리

`fetch`나 `XMLHttpRequest`를 직접 사용해서 요청을 보낼 수도 있지만,  
편의성과 기능 확장을 위해 별도의 비동기 처리 라이브러리를 사용하는 경우가 많다.

그중 가장 널리 사용되는 라이브러리가 **Axios**다.

[Axios 공식 문서 보기](https://axios-http.com/kr/docs/intro)  
![](https://velog.velcdn.com/images/givpro22/post/e89ada5b-4f13-4da8-bbd9-d9002a46eb47/image.png)

---

### Axios란?

Axios는 브라우저와 Node.js 환경 모두에서 사용할 수 있는 **HTTP 클라이언트 라이브러리**다.  
비동기적으로 HTTP 요청을 보내고, 응답을 받을 수 있게 도와준다.

내부적으로는 `Promise` 기반으로 작성되어 있어 `then`, `catch` 구문을 사용할 수 있으며,  
`async/await` 문법과도 자연스럽게 호환된다.

---

### Axios의 주요 특징

1. **브라우저와 Node.js에서 모두 사용 가능**  
   환경에 관계없이 동일한 API로 작성할 수 있다.

2. **Promise 기반 구조**  
   HTTP 요청 결과가 `Promise` 객체로 반환된다.  
   따라서 `.then()`, `.catch()` 또는 `async/await`으로 처리할 수 있다.

3. **자동 JSON 변환**  
   요청 시 데이터를 자동으로 JSON 문자열로 변환해주고,  
   응답 역시 자동으로 JSON 객체로 변환해준다.

4. **요청 및 응답 인터셉터 제공**  
   요청을 보내기 전 또는 응답을 받기 전에 특정 로직을 삽입할 수 있다.  
   예를 들어, 인증 토큰 삽입, 에러 처리 공통화 등에 활용할 수 있다.

5. **요청 취소 기능 지원**  
   사용자가 원할 경우 HTTP 요청을 중간에 취소할 수 있다.

---

> Axios 사용 시 주의할 점
> Axios는 다양한 기능을 제공하지만, 상대적으로 파일 크기가 큰 편이다.  
> 그래서 **경량성**, **간결한 문법**, **모듈화** 등을 중요하게 생각한다면  
> `ky` 같은 대체 라이브러리를 고려할 수도 있다.

---

## 실제로 사용해 보자

### Axios 설치

```javascript
npm install axios
```

### 요청 파라미터

```javascript
axios(url, {
  // 설정 옵션
});
```

fetch처럼 HTTP 메서드를 명시하지 않으면 기본적으로 GET 요청을 보낸다.

```javascript
axios(url);
```

필요한 경우 두 번째 인자를 사용해서 요청 방식을 설정할 수도 있다.

```javascript
axios(url, {
  method: "get", // 다른 옵션도 가능합니다 (post, put, delete, etc.)
  headers: {},
  data: {},
});
```

#### 들어갈 수 있는 설정!

| config          | description                                                                    |
| --------------- | ------------------------------------------------------------------------------ |
| **method**      | 요청을 보낼 URL 주소를 지정. 엔드포인트를 가리킨다.                            |
| **url**         | HTTP 요청 메서드로 GET이 기본적으로 사용된다.                                  |
| params          | URL의 쿼리 매개변수를 설정                                                     |
| headers         | 요청 헤더. 요청 헤더에 사용자 정의 헤더를 추가 가능                            |
| data            | 요청 body에 보낼 데이터를 설정한다.                                            |
| baseURL         | 공통적으로 사용하는 기본 URL 주소를 설정                                       |
| responseType    | 서버에서 받을 응답의 데이터 형식을 설정해준다.                                 |
| withCredentials | Axios 요청에서 쿠키 및 HTTP 인증 정보를 포함하도록 설정하는 옵션               |
| auth            | 요청에 대한 HTTP 기본 인증을 설정한다. 사용자 이름과 암호를 제공하여 인증 수행 |

### 사용예시

```javascript
// GET 요청
axios({
  method: "get",
  url: url,
  params: {
    id: 1,
    category: "review",
  },
  headers: {
    Authorization: "Bearer YourAccessToken",
  },
})
  .then((res) => {
    console.log(res.data);
  })
  .catch((err) => {
    console.error(err);
  });
```

```javascript
// POST 요청
axios({
  method: "post",
  url: url,
  data: {
    name: "Dev",
    title: "good review",
  },
});
```

---

### 요청 메서드 명령어들

아래와 같이 HTTP 메서드를 붙일 수도 있다.

```javascript
axios.get(url, {
  // 설정 옵션
});
```

편의에 따라 사용할 수 있는 다양한 요청 메서드가 있다.  
이 중에서 가장 많이 사용되는 형태는 GET, POST, PUT, DELETE 요청에 대한 단축 메서드이다.

- `axios.request(config)`
- `axios.get(url[, config])`
- `axios.delete(url[, config])`
- `axios.post(url[, data[, config]])`
- `axios.patch(url[, data[, config]])`
- `axios.put(url[, data[, config]])`
- `axios.head(url[, config])`
- `axios.options(url[, config])`

---

## get

```javascript
const url = "https://jsonplaceholder.typicode.com/todos";

axios.get(url).then((response) => console.log(response.data));
```

Axios를 사용하면 응답 데이터를 기본적으로 **JSON 형태**로 사용할 수 있다.  
응답 데이터는 항상 응답 객체의 `data` 프로퍼티를 통해 접근할 수 있다.

또한, 설정 옵션에서 `responseType`을 지정하면  
기본 JSON 타입 외에 다른 형식으로도 응답 데이터를 받을 수 있다.

```javascript
axios.get(url, {
  responseType: "json", // options: 'arraybuffer', 'document', 'blob', 'text', 'stream'
});
```

## post

이제 JSONPlaceholder API를 사용해서 데이터를 전송해보자.

이를 위해서는 데이터를 JSON 문자열로 직렬화해야 하는데,  
`POST` 메서드로 JavaScript 객체를 전송하면 **Axios가 자동으로 문자열로 변환**해준다.

아래는 Axios로 POST 요청을 보내는 예제 코드다.

```javascript
const url = "https://jsonplaceholder.typicode.com/todos";

const todo = {
  title: "A new todo",
  completed: false,
};

axios
  .post(url, {
    headers: {
      "Content-Type": "application/json",
    },
    data: todo,
  })
  .then(console.log);
```

Axios로 post요청을 할 때 요청 본문(request body)으로 보내고자 하는 data는 data 프로퍼티에 할당한다. 컨텐츠 유형 헤더도 설정할 수 있다.

> 핵심 !
> 기본적으로 axios는 Content-Type을 application/json으로 설정한다.

---

## 고급(?) 활용 해보기

Axios를 좀 더 효율적으로 사용하는 방법에 대해 정리해보자.

### 1. 일반적인 Axios 사용 방식

예를 들어, 글 작성 API를 호출한다고 가정해보자.  
보통은 아래처럼 요청을 작성하게 된다.

```javascript
import axios from "axios";
import cookies from "js-cookie";

const SERVER = "http://localhost:8080/api/v1";

const requestPost = async (postDto) => {
  const res = await axios.post(`${SERVER}/write-post`, postDto, {
    headers: {
      access_token: cookies.get("access_token"),
    },
  });
};
```

만약 기능을 위처럼 구현한다고 가정하면, 일일히 headers에 토큰을 넣는 코드를 작성하고, 기본 서버 URL을 계속해서 작성하는 코드를 넣게 됩니다.
그런데 이 작업들을 일일히 하지 않고, 자동으로 값이 삽입이 된다면 어떨까요?

그래서 저희는 이 axios를 커스텀하여 사용해보도록 하겠습니다.

### 2. customAxios.ts 파일 생성하기

먼저 Axios 인스턴스를 정의할 파일을 만들어보자.  
예를 들어 `src/lib` 경로에 `customAxios.ts` 파일을 생성한다고 가정하자.  
이 파일에서는 기본 설정이 적용된 Axios 인스턴스를 만들어서 내보낼 예정이다.

```javascript
import axios, { AxiosInstance } from "axios";
import cookies from "js-cookie";

export const customAxios: AxiosInstance = axios.create({
  baseURL: `${SERVER_ADDRESS}`, // 기본 서버 주소 입력
  headers: {
    access_token: cookies.get("access_token"),
  },
});
```

위 코드에서 사용된 axios.create 함수의 설정 항목들은 다음과 같은 역할을 한다.

- baseURL: 모든 요청의 기본 URL이 된다. 실제 요청 시에는 이 경로를 제외한 부분만 작성하면 된다.
- headers: 모든 요청에 자동으로 포함될 헤더를 정의한다. 예를 들어 사용자 인증 토큰을 자동으로 넣을 수 있다.

이제 이 customAxios를 export했으니, 실제 API 요청에 바로 사용할 수 있다.

### 3. customAxios 활용하기

앞에서 예시로 사용했던 글 작성 API를 customAxios를 활용해서 다시 구현해보자.

```javascript
import { customAxios } from "lib/customAxios";

const requestPost = async (postDto) => {
  const res = await customAxios.post("/write-post", postDto);
};
```

이전의 일반적인 axios 코드와 비교해보면 다음과 같은 차이점이 있다.
• baseURL은 Axios 인스턴스에 이미 설정되어 있으므로, 경로의 뒷부분만 작성하면 된다.
• headers에 토큰도 자동으로 포함되므로 매번 수동으로 추가할 필요가 없다.

이처럼 Axios를 커스터마이징해서 사용하면 반복되는 코드를 줄이고,
더 깔끔하고 일관된 API 요청 코드를 작성할 수 있다.

개인적으로는 이 두 가지 이유만으로도 커스텀 인스턴스를 사용할 충분한 가치가 있다고 생각한다.

---

## 참고

> [React에서 Axios사용하기](https://ramincoding.tistory.com/entry/React-Axios-간단하게-사용하기)

> [[번역] 입문자를 위한 Axios vs Fetch](https://velog.io/@eunbinn/Axios-vs-Fetch)

> [React에서 axios 모듈화 하기 ](https://velog.io/@yiyb0603/React에서-axios-커스텀하기)
