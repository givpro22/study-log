![](https://velog.velcdn.com/images/givpro22/post/d8429b71-4fad-414e-a181-3ab531a3fc78/image.png)

### 박스 모델(Box Model) 구성 요소 정리

HTML/CSS에서 박스 모델은 요소의 레이아웃과 간격을 이해하는 데 핵심적인 개념이다.  
오늘은 그 구성 요소인 `Content`, `Padding`, `Border`, `Margin`의 역할을 정리해보았다.

---

- **`Content`**  
  텍스트나 이미지가 들어있는 HTML 요소의 **실질적인 내용**

- **`Padding`**  
  Content와 Border 사이의 영역으로, 쉽게 말해 **안쪽 여백**이다.

- **`Border`**  
  Content를 감싸는 **테두리** 역할을 한다.

- **`Margin`**  
  테두리와 이웃하는 요소 사이의 간격으로, 쉽게 말해 **바깥쪽 여백**이다.

---

### Box Model CSS 속성

박스 모델은 속성을 지정하지 않으면 브라우저가 선언한 기본값으로 셋팅되며, 우리는 CSS 속성으로 박스모델 값을 변경할 수 있습니다.  
여기에서는 간단히만 알아보고 밑에서 더 자세히 알아보도록 하겠습니다.

---

### Content

- `width` - Content 영역은 `width` 값을 이용하여 **가로 너비**를 지정할 수 있습니다.
- `height` - Content 영역은 `height` 값을 이용하여 **세로 너비**를 지정할 수 있습니다.

```css
width: 200px;
height: 200px;
```

### 바깥 여백 `Margin`

- `margin` 속성은 HTML 요소의 **바깥 여백**을 지정합니다.
- `margin-top`, `margin-bottom`, `margin-left`, `margin-right` 속성을 사용하여 각각 지정할 수도 있습니다.

#### 예시

```css
margin: 20px;

margin: 10px 20px;
/* 위아래 10px, 좌우 20px */

margin: 4px 2px 2px 4px;
/* 위 4px, 오른쪽 2px, 아래 2px, 왼쪽 4px */

margin-top: 20px;
```

---

### 마진 중첩 (Margin Collapse)

HTML 요소를 **세로로 배치할 경우**,  
`margin`과 `margin`이 만날 때 **margin 값이 큰 쪽으로 겹쳐집니다.**

즉, 두 요소 사이의 여백이 `20px`과 `30px`이라면 합쳐서 `50px`이 되는 것이 아니라 **큰 값인 30px만 적용**됩니다.

요소를 **가로로 배치할 경우에는 상관없지만**,  
**세로로 배치할 경우에만 값이 큰 `margin`만 적용**된다는 점을 기억해야 합니다.

---

### box-sizing 속성

박스 모델 계산 기준을 바꾸는 속성입니다.

```css
box-sizing: content-box; /* 기본값 */
box-sizing: border-box;
```

- `content-box`: width, height는 Content만 포함합니다. → padding, border는 별도 계산
- `border-box`: width, height에 padding, border까지 포함됩니다. → 실무에서 더 자주 사용됨

---

#### 실무에서 자주 쓰는 초기화 예시

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

모든 요소의 `box-sizing`을 `border-box`로 통일해 예측 가능한 레이아웃 설계를 도와줍니다.
