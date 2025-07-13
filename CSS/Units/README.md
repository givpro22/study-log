> [프론트엔드 필수 반응형 CSS 단위 총정리 (EM과 REM) | Responsive CSS Units](https://www.youtube.com/watch?v=7Z3t1OWOpHo&t=8s)
> 영상을 참고해서 CSS Units에 대해 정리해보았다.

---

## 절대적 VS 상대적

![절대 유닛 설명](https://velog.velcdn.com/images/givpro22/post/714205b0-ea11-48d1-adf7-e00aa5bdb647/image.png)

CSS에서 사용하는 단위는 **크기를 지정하는 기준**에 따라  
크게 두 가지로 나뉜다: **절대 단위**와 **상대 단위**

- 절대 단위는 **고정 크기** → 정확하고 예측 가능하지만 유연성 부족
- 상대 단위는 **비율 기반 크기** → 환경에 따라 유동적이며 반응형에 적합

실무에서는 대부분 `rem`, `vw`, `%` 같은 상대 단위를 적절히 섞어서 사용한다.

---

## 절대적 단위

![px 단위 문제점](https://velog.velcdn.com/images/givpro22/post/b7afc37e-c3e1-4dd7-a07c-40240dedac23/image.png)

- `px`는 **고정된 크기**를 가진 단위로, 반응형 디자인에서 유연하게 대응하지 못함
- 컨테이너의 크기가 변경되더라도 요소는 **변화 없이 고정된 값**으로 유지됨
- 또한 사용자가 브라우저에서 **기본 폰트 크기 설정을 변경해도 무시됨**
- 이로 인해 접근성과 유연성 측면에서 단점이 있다

그래서 반응형을 고려할 때는 `px` 대신 **상대 단위**를 사용하는 것이 일반적이다

---

## 상대적 단위

![상대 유닛 설명](https://velog.velcdn.com/images/givpro22/post/ee1b319a-f042-4f78-bc38-e95b53091a45/image.png)

상대 단위는 요소의 **부모나 루트 요소**, 혹은 **뷰포트**를 기준으로 크기가 설정된다.  
대표적인 단위로는 `em`, `rem`, `%`, `vw`, `vh`가 있다.

---

### em

`em`은 **부모 요소의 font-size를 기준으로 상대적인 크기**를 가진 단위이다.  
부모 요소가 변경되면 자식 요소도 그 영향을 받아 누적된다.

<table>
  <tr>
    <td><img src="https://velog.velcdn.com/images/givpro22/post/61d56d86-4e0d-4b95-9eed-758314d546bf/image.png" width="100%"></td>
    <td><img src="https://velog.velcdn.com/images/givpro22/post/309f6306-7027-4052-b68e-dfd9ac9dd2a4/image.png" width="100%"></td>
  </tr>
</table>

위 그림은 `1em`, `2em`, `3em`의 크기가  
**부모 요소의 font-size를 기준으로 배수로 적용**되는 구조를 보여준다.

- 예를 들어 부모의 `font-size: 20px`이면
  - `1em` = 20px
  - `2em` = 40px
  - `3em` = 60px

즉, **em은 절대값이 아니라 부모 기준의 상대값**이다.

---

em 단위가 **중첩될 경우 누적되는 현상**

- `div` 안의 `p` 태그가 `1.2em`
- `p` 안의 `span`이 다시 `1.2em`이면
- 실제 `span`의 크기는 `1.2 × 1.2 = 1.44em` (즉, 44% 증가)

이처럼 em은 중첩된 구조에서 **예상보다 커지는 결과**를 만들 수 있다.

![em padding 예시](https://velog.velcdn.com/images/givpro22/post/cbd5c320-acdf-4331-8067-244fd9015d32/image.png)

- `em`은 **부모 요소의 font-size를 기준으로 상대적 크기**를 설정
- 예: 부모의 `font-size`가 16px이면, `1em`은 16px, `2em`은 32px
- 하지만 **중첩될수록 누적되어 계산**되기 때문에 예상치 못한 크기 변화가 발생할 수 있다
- 스타일을 계층적으로 적용할수록 관리가 복잡해질 수 있음

---

### rem

![rem 개념](https://velog.velcdn.com/images/givpro22/post/a38205a8-a048-444d-bab1-15d04b017cf3/image.png)

`rem`은 **root em**의 줄임말로,  
`html` 요소의 `font-size`를 기준으로 크기를 계산하는 단위이다.

![rem vs em 차이](https://velog.velcdn.com/images/givpro22/post/d4cb3344-1c35-46b0-a968-63f0d2f02fae/image.png)

- `rem`은 **루트 요소(html)의 font-size를 기준**으로 크기를 계산
- `html { font-size: 16px }`일 때 `1rem = 16px`
- 부모 요소에 영향받지 않고 **예측 가능한 크기 조절**이 가능
- 그래서 실무에서 **em보다 rem을 선호**하는 경우가 많다

여기서 중요한 점은:
**부모 요소의 영향을 전혀 받지 않고, 오직 root(html)의 font-size만을 기준**으로 한다는 것!

---

### vw / vh

- `vw`는 **뷰포트의 너비(Viewport Width)** 기준
  - `100vw` = 브라우저 너비 100%
- `vh`는 **뷰포트의 높이(Viewport Height)** 기준
  - `100vh` = 브라우저 높이 100%

반응형 화면에서 뷰포트를 기준으로 요소의 크기나 배경을 설정할 때 자주 사용

---

### %

- `%`는 **부모 요소의 크기를 기준으로 비율을 설정**
- 예: 부모가 `width: 500px`일 때 `width: 50%`는 250px
- 주로 너비, 높이, margin, padding 등에 사용되며,
  **컨테이너의 변화에 따라 자동으로 대응**할 수 있어 유용

---

## 정리

| 단위  | 기준 요소              | 특징                           |
| ----- | ---------------------- | ------------------------------ |
| `px`  | 고정값                 | 반응형에 불리함, 유연하지 않음 |
| `em`  | 부모의 font-size       | 중첩에 따라 값이 누적됨        |
| `rem` | 루트(html)의 font-size | 예측 가능, 실무에서 선호       |
| `%`   | 부모 요소 크기         | 비율 기반, 반응형에 유용       |
| `vw`  | 브라우저 너비          | 전체 뷰포트 기준               |
| `vh`  | 브라우저 높이          | 전체 뷰포트 기준               |

---

단위에 대한 개념을 명확히 이해하면  
**반응형 레이아웃을 설계하거나 접근성을 고려할 때** 더 유리하다.  
특히 `rem`과 `vw/vh` 조합은 실무에서도 매우 자주 사용된다.
