[CSS Grid 완전 정리 끝판왕](https://www.youtube.com/watch?v=nxi1EXmPHRs&t=1079s)  
영상을 참고해서 CSS Grid에 대해 정리해보았다.

---

## 기본 개념

CSS Grid는 2차원 레이아웃 시스템으로, 행(row)과 열(column)을 기준으로 요소를 배치할 수 있다.  
Flexbox가 1차원(가로 혹은 세로)에 특화되어 있다면, Grid는 **가로+세로 동시 조절**이 가능하다.

---

## 학습 이미지

![grid 설명 1](https://velog.velcdn.com/images/givpro22/post/7c0f767e-160a-47cc-bd08-2803ed89765a/image.png)  
![grid 설명 2](https://velog.velcdn.com/images/givpro22/post/bfe785d4-61c5-413b-963d-3a49fd3d4f60/image.png)  
![grid 설명 3](https://velog.velcdn.com/images/givpro22/post/3045a731-b459-40da-b8f3-0dee05a02748/image.png)

---

## grid-template-columns / grid-template-rows

```css
grid-template-columns: 20% 20% 20% 20% 20%;
grid-template-rows: 20% 20% 20% 20% 20%;
```

- 열과 행의 크기를 개별적으로 설정할 수 있다.
- `px`, `%`, `em` 등의 단위뿐만 아니라, 새로운 단위인 `fr`도 사용할 수 있다.
- 예: `grid-template-columns: 1fr 3fr;` → 첫 번째 요소는 1/4, 두 번째 요소는 3/4 공간을 차지

---

## repeat 함수

동일한 크기의 열을 반복해서 작성하는 경우 `repeat()` 함수를 활용하면 간단하게 표현할 수 있다.

```css
/* 아래 두 코드는 동일한 결과 */
grid-template-columns: 20% 20% 20% 20% 20%;
grid-template-columns: repeat(5, 20%);
```

---

## grid-column-start / grid-column-end

- 특정 요소가 시작할 그리드 선과 끝날 선을 지정할 수 있다.
- 예: `grid-column-start: 2;` `grid-column-end: 4;`
- 반복되는 입력이 번거로울 경우 단축 속성을 사용할 수 있음

```css
grid-column: 2 / 4; /* 2번 선부터 4번 선까지 차지 */
```

- `span` 키워드를 사용하면 상대적인 너비 설정도 가능

```css
grid-column-end: span 2; /* 현재 위치부터 열 2칸 차지 */
```

---

## grid-row와 grid-column 단축: grid-area

네 개의 속성(`grid-row-start`, `grid-column-start`, `grid-row-end`, `grid-column-end`)을 한 번에 지정할 수 있는 단축 속성

```css
grid-area: 1 / 1 / 3 / 6;
/* 1행 1열에서 시작 → 3행 6열까지 영역 차지 */
```

---

## grid-template

`grid-template-rows`와 `grid-template-columns`를 합쳐서 한 줄로 정의할 수 있음

```css
grid-template: 50% 50% / 200px;
/* 50%씩 두 행, 200px 너비의 한 열 */
```

---

Grid는 다양한 레이아웃 구성이 가능한 만큼 처음에는 복잡하게 느껴질 수 있지만,  
`grid-template`, `repeat`, `fr`, `area` 등의 개념을 익히면 오히려 Flex보다 더 강력한 도구가 된다.

---

## 실습 사이트

> GRID 개념을 실습하며 게임처럼 익힐 수 있는 사이트  
> 👉[링크텍스트](https://codepip.com/games/grid-garden/#ko)

![grid 설명 4](https://velog.velcdn.com/images/givpro22/post/65fcba9c-1242-48d7-81cc-867483565a7c/image.png)

이론만 보는 것보다 실제로 손으로 코드를 짜면서 체득하는 게 훨씬 눈에 익숙해진다(?).

---
