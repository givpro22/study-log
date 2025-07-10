[CSS Flexbox 완전 정리. 포트폴리오 만드는 날까지! | 프론트엔드 개발자 입문편: HTML, CSS, Javascript](https://www.youtube.com/watch?v=7neASrWEFEM&t=2s)  
영상을 보며 CSS Flexbox의 기본 개념과 자주 사용하는 속성들에 대해 정리해보았다.

---

## 기본 개념

Flexbox는 **레이아웃 구성에서 요소들의 정렬과 간격을 간편하게 제어**할 수 있도록 도와주는 CSS 모듈이다.  
특히 **가로 정렬 / 세로 정렬 / 공간 분배**를 유연하게 처리할 수 있어 실무에서도 자주 사용된다.

---

## 학습 스크린샷

![flexbox 구조 설명](https://velog.velcdn.com/images/givpro22/post/11f5175f-a8d6-4e96-aaa0-ae0be16b4bcf/image.png)

Flexbox에서는 **컨테이너(container)**와 **아이템(item)**의 역할이 명확히 나뉜다.  
컨테이너에는 레이아웃을 정의하는 속성들을, 아이템에는 개별 요소에 대한 정렬 및 확장 속성들을 적용한다.

아래 이미지 기준으로,  
**왼쪽**은 `container`에 적용할 수 있는 속성들,  
**오른쪽**은 `item`에 적용할 수 있는 속성들을 정리한 표이다.

<table>
  <tr>
    <td><img src="https://velog.velcdn.com/images/givpro22/post/707b7eed-83dc-4536-a544-d62871ac4859/image.png" width="80%"></td>
    <td><img src="https://velog.velcdn.com/images/givpro22/post/fa752c9f-ea68-49a5-8360-ace2a369ffa5/image.png" width="100%"></td>
  </tr>
</table>

아래는 각 flex속성의 중심축과 보조축 개념을 시각적으로 정리한 도식이다.

![flexbox 속성 시각화](https://velog.velcdn.com/images/givpro22/post/6ad490df-5e57-400c-89cb-5c94711fdf87/image.png)

## Flexbox의 중심축(main axis)과 보조축(cross axis)

Flexbox에서 요소의 정렬 방식은 **축의 개념**을 기준으로 이해하는 것이 중요하다.  
이 축은 `flex-direction` 속성에 따라 **주축(main axis)**과 **교차축(cross axis)**으로 나뉜다.

---

### 주축 (Main Axis)

- Flex 항목들이 **배치되는 기본 방향의 축**
- `flex-direction`에 따라 방향이 결정됨
- `justify-content` 속성으로 정렬 방식 조절

| flex-direction 값 | 주축 방향        |
| ----------------- | ---------------- |
| `row`             | 가로 (왼 → 오)   |
| `row-reverse`     | 가로 (오 → 왼)   |
| `column`          | 세로 (위 → 아래) |
| `column-reverse`  | 세로 (아래 → 위) |

---

### 교차축 (Cross Axis)

- 주축에 **수직인 방향의 축**
- `align-items`, `align-content` 속성으로 정렬 방식 조절
- 주축이 수평이면 교차축은 수직, 주축이 수직이면 교차축은 수평

### 시각적으로 정리

| flex-direction | 주축 (main axis) | 교차축 (cross axis) |
| -------------- | ---------------- | ------------------- |
| `row`          | 가로 (왼 → 오)   | 세로 (위 → 아래)    |
| `column`       | 세로 (위 → 아래) | 가로 (왼 → 오)      |

---

### 예시

```css
.container {
  display: flex;
  flex-direction: row;
  justify-content: center; /* 주축 정렬 */
  align-items: center; /* 교차축 정렬 */
}
```

---

---

## 자주 사용하는 속성들 정리

### `justify-content`

Flex 요소들을 **가로축 기준**으로 정렬할 때 사용

- `flex-start` (기본값)
- `flex-end`
- `center`
- `space-between`
- `space-around`
- `space-evenly`

### `align-items`

Flex 요소들을 **세로축 기준**으로 정렬할 때 사용

- `flex-start`
- `flex-end`
- `center`
- `baseline`
- `stretch` (기본값)

### `flex-direction`

아이템들의 배치 방향

- `row` (기본값)
- `row-reverse`
- `column`
- `column-reverse`

### `flex-wrap`

한 줄에 다 넣을지, 여러 줄로 나눌지 결정

- `nowrap` (기본값)
- `wrap`
- `wrap-reverse`

### `align-content`

여러 줄일 때 세로 방향 정렬을 조절

- `flex-start`
- `flex-end`
- `center`
- `space-between`
- `space-around`
- `space-evenly`
- `stretch` (기본값)

---

## 실습 사이트

> Flexbox 개념을 실습하며 게임처럼 익힐 수 있는 사이트  
> 👉 [https://flexboxfroggy.com/#ko](https://flexboxfroggy.com/#ko)

![flexbox froggy](https://velog.velcdn.com/images/givpro22/post/0afab593-95f0-4612-b219-641f120714c3/image.png)

이론만 보는 것보다 실제로 손으로 코드를 짜면서 체득하는 게 훨씬 눈에 익숙해진다(?).

---
