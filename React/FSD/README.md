### 기존 구조

보통 많이 사용되는 프론트엔드 폴더 구조는 아래와 같다.

<img src="https://velog.velcdn.com/images/teo/post/367c59ae-7a1c-47bc-888a-e07d17845b2d/image.png" width="200" />

> `/api` : 백엔드와의 통신을 담당하는 코드  
> `/assets` : 사용되는 이미지, 아이콘 등 정적 파일  
> `/components` : 공통으로 쓰이는 컴포넌트  
> `/config` : 설정 관련 파일  
> `/contents` : 문서나 콘텐츠 파일  
> `/model` : 데이터 구조(interface, types) 정의  
> `/hooks` : 커스텀 훅  
> `/i18n` : 다국어 번역 파일  
> `/layout` : 공통 레이아웃 구성  
> `/libs` : 공통 유틸 함수, 라이브러리  
> `/store` : 상태 관리 관련 로직  
> `/service` : 비즈니스 로직  
> `/utils` : 공통 유틸 함수  
> `/pages` : 라우팅되는 페이지 컴포넌트  
> `/routes` : 라우터 설정  
> `/server` : 서버 관련 파일  
> `/styles` : 전역 스타일 정의  
> `/types` : 전역 타입 정의

---

### 기존 구조의 한계

위 구조는 중소형 규모에서는 잘 작동한다.  
하지만 프로젝트의 규모가 커지게 되면 **한 기능(예: main, order, cart)** 에 대한 코드가  
`components`, `hooks`, `utils`, `pages` 등에 나눠져 흩어지게 되어  
**기능 단위로 코드 흐름을 따라가기 어려워지는 문제**가 생긴다.

![역할별 분리의 문제점](https://velog.velcdn.com/images/teo/post/d8ed3101-76f5-40fb-b212-d30b125b6dc8/image.png)

---

### 그래서 등장한 FSD 아키텍처 (Feature-Sliced Design)

> 공식 사이트: [feature-sliced.design](https://feature-sliced.design)

FSD는 프론트엔드 애플리케이션을 **도메인과 관심사 중심으로 구조화**하는 방법론이다.  
기존처럼 역할별(`/components`, `/hooks`, `/utils`)로만 나누는 것이 아니라,  
**비즈니스 기능 단위로 분리**해서 유지보수성과 확장성을 높인다.

---

### FSD의 세 가지 구조 개념

![](https://velog.velcdn.com/images/givpro22/post/d671725a-325d-491f-ad4d-a8ce830aef35/image.png)

#### 1. 레이어(Layer)

- **app**: 앱의 초기화, 라우터, 전역 설정 등 진입점
- **processes**: 회원가입 등 여러 페이지로 구성된 흐름 처리 (선택적)
- **pages**: 라우팅 되는 페이지 단위
- **widgets**: 페이지 내 사용되는 독립적 UI 구성 요소
- **features**: 사용자 기능(좋아요, 댓글 등) 단위 기능
- **entities**: 도메인 엔티티(사용자, 상품 등) 정의
- **shared**: 공통 컴포넌트, 유틸, 설정 등

> 상위 레이어는 하위 레이어만 참조 가능 → **단방향 흐름**

#### 2. 슬라이스(Slice)

- 도메인 단위로 폴더를 나누는 것
- 예: `/features/LikeButton`, `/entities/User`, `/widgets/Header`

#### 3. 세그먼트(Segment)

> 슬라이스 내부를 기술 관점으로 나눈 구조

| 세그먼트 | 설명                         |
| -------- | ---------------------------- |
| `api`    | 서버 요청 관련               |
| `UI`     | 컴포넌트                     |
| `model`  | 상태, actions, selectors 등  |
| `lib`    | 보조 유틸 함수               |
| `config` | 슬라이스 설정값 (거의 안 씀) |
| `consts` | 상수                         |

---

### 계층 구조에서의 위험도

![FSD 위험도 구조](https://velog.velcdn.com/images/givpro22/post/27de102d-5993-41fe-8d87-08168513b3f8/image.png)

![레이어 구조](https://velog.velcdn.com/images/givpro22/post/68ccf5fc-0079-4370-a3e7-a995f954ad93/image.png)

- 레이어가 **아래에 위치할수록 더 많은 곳에서 사용**됨
- 즉, 하위 레이어(`shared`, `entities`)를 수정할수록 파급 효과가 크다  
  → 구조를 잡을 때 더욱 신중해야 함

![index import 규칙](https://velog.velcdn.com/images/givpro22/post/b1cede12-74f8-4901-b47b-d641f0f2ff7d/image.png)

---

### FSD 규칙 (공개 API)

- 각 슬라이스와 세그먼트는 **index.ts (혹은 .js)** 파일을 통해 외부에 필요한 기능만 export
- 외부에서는 **index를 통해서만 import**해야 한다 → 내부 구현은 숨기고 외부에 노출 X

**장점**

- 인덱스를 통해 필요한 기능만 노출 → 불필요한 의존 제거
- 내부 구현을 숨겨서 추후 변경에 유연하게 대응 가능

**주의사항**

- **index.ts 외 직접 내부 경로 접근 금지**
- 모든 import는 index를 통해서만 접근해야 한다

> ### index.ts에서만 import 하기 추가설명
>
> FSD의 핵심 규칙 중 하나는  
> **슬라이스나 세그먼트의 내부 구현이 아닌, 공개 API(index.ts)만을 통해 접근**하는 것이다.

```ts
// 잘못된 방식
import { something } from "@/features/LikeButton/model/state";
// 올바른 방식
import { something } from "@/features/LikeButton";
```

---

### FSD의 장단점

#### 장점

- 아키텍처 구성 요소의 교체/추가/삭제가 쉬움
- 구조의 명확한 표준화
- 확장성에 강함
- 비즈니스 중심의 설계
- 모듈 간 연결이 명확하고 안전함

#### 단점

- 초반 진입 장벽이 있음
- 팀원 간 개념 공유와 합의가 필요
- 설계 단계에서 고민이 많음 (단, 장기적으로는 이점)

---

### 기존 구조에서 FSD 구조로 옮길 때

간단하게 매핑하면 다음과 같이 구조화할 수 있다:

| 기존                         | FSD 구조                          |
| ---------------------------- | --------------------------------- |
| `/components`                | `entities`, `features`, `widgets` |
| `/utils`, `/hooks`, `/types` | `shared`                          |
| `/layout`                    | `widgets` 또는 `shared/UI`        |
| `/pages`                     | 동일 (`/pages`)                   |
| `/app`                       | 전역 초기화                       |

> 슬라이스 이름은 가능하면 **동사보다 명사 중심**,  
> 하지만 `features`에는 사용자 행동 위주 기능(예: WriteReview) 등을 동사 느낌으로 명명해도 무방

---

### 마무리

기존 구조도 충분히 좋은 구조지만, **프로젝트 규모가 커질수록 관심사 분리가 무너지는 문제**가 반복되기 때문에 FSD처럼 **도메인 중심의 아키텍처**가 좋은 대안이 될 수 있다.

실무나 사이드 프로젝트에서도 점차 이런 방식으로 구조화해보며 익숙해질 필요가 있다는 생각이 들었다

폴더구조에 프로젝트 시작시마다 고민이 들어서 검색을 해봤지만 절대적으로 "이것이 정답"이라고 할 수 있는 구조도 없는것 같다. 결국에 프로젝트 구조에 답은 없다를 완전히 이해하고 받아드리는 것이 중요한 것 같다는 생각이 들었다.

---

### 참고 & 읽어볼거리

> [지역성의 원칙을 고려한 패키지 구조: 기능별로 나누기](https://ahnheejong.name/articles/package-structure-with-the-principal-of-locality-in-mind)

> [프론트엔드 개발자 관점으로 바라보는 관심사의 분리와 좋은 폴더 구조 (feat. FSD)](https://velog.io/@teo/separation-of-concerns-of-frontend)

> [(번역) 기능 분할 설계 - 최고의 프런트엔드 아키텍처](https://emewjin.github.io/feature-sliced-design/?utm_source=substack&utm_medium=email)
