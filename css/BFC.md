# Block Formatting Context

> 웹페이지의 블록 레벨 요소를 렌더링하는데 사용되는 CSS의 비주얼 서식 모델 중 하나이다.

<br>

## 생성 조건
다음 조건 중 하나를 만족하면 BFC가 생성된다.
- float이 none이 아니며 clear되지 않은 경우
- position이 absolute, fixed일 때
- diplay가 inline-block, table-cell, table-caption, flex, inline-flex일 때
- overflow가 visible이 아닐 때

<br>

## 활용
1. 마진겹칩 현상을 제거할 수 있다.
2. float요소들을 포함할 수 있다.

<br>

### 참고
[MDN - Block formatting Context](https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context)