# a11y-hidden

- 접근성을 위해 스크린리더가 읽을수 있게끔 해당 요소의 위치의 변경은 없지만 화면에서 감출때 사용하는 방법이다.

- a11y는 accessibility 를 의미한다. a11y-hidden은 html이나 css에서 정해진 특정 요소는 아니지만 말 그대로 접근은 가능하지만 숨기겠다는 의미로 class이름에 명시적으로 사용중이다.

- h1~6, label, legend 등등 접근성과 문서의 논리적인 구조를 위해 작성해놓은 텍스트들을 화면에서만 숨길때 사용하게 되는데 잘못 사용되고 있는 예를 몇가지 살펴보자.

## display: none;
```css
.hidden {
  display: none;
}
```
- display값을 조정하여 숨김처리를 하는경우가 실제로 많다. 아마 접근성을 아예 고려하지않았을때 쓰는 속성이 아닐까 싶다. 요소 자체를 없애버리기 때문에 스크린리더 및 검색엔진에도 정보를 제공하지 못하게된다.


## visibility: hidden;
```css
.hidden {
  visibility: hidden;
}
```
- 투명처리를 한것처럼 자리는 차지하고 눈으로 보이는 요소만 숨김을 했지만 이 속성도 동일하게 스크린리더와 검색엔진에 정보를 제공하지 못한다.

## text-indent: -9999px;
```css
.hidden {
  text-indent: -9999px;
}
```
- text를 화면밖으로 이탈시켜 눈에 보이지 않게 하는것이다.
물론 이 방법을 사용하면 스크린리더가  읽을수는 있게된다.
하지만 focus가 일어났을때 화면이 바깥으로 튕겨나가는 현상이 발생하기도 하고 [구글 검색엔진 가이드라인](https://support.google.com/webmasters/answer/66353)에서는 CSS를 사용하여 텍스트를 화면에 보이지 않도록 배치하는것을 [웹마스터 가이드라인](https://support.google.com/webmasters/answer/35769)에 위반되는것이라고 명시해놨다.

## width, height, font-size 0으로 설정
```css
.hidden {
  width: 0;
  height: 0;
  font-size: 0;
}
```
- 이 방법은 환경(브라우저 및 디바이스)에 따라 display:none; 처럼 아예 인식이 안되는 경우가 있다.

## opacity: 0;
```css
.hidden {
  opacity: 0;
}
```
- 요소를 투명하게 만드는방법인데 이 방법도 동일하게 환경에 따라서 스크린리더가 읽지 못하는문제가 발생한다.

## positioning screen out
```css
.hidden{
  position: absolute;
  top: -9999px;
  left: -9999px;
}
```
- 아마 가장 많이 사용되는 방법일거 같다. 이 방법은 실제 이슈가 됐었는데 문서의 가장 하단의 요소를 위와 같은 방법으로 숨기려고 했을때 스크린리더의 오버를 받았을때 top: 9999px;이란 값 때문에 문서의 위쪽으로 화면이 튕겨 올라가버리는 현상이 발생한다. 그리고 text-indent에서 설명했듯이 [웹마스터 가이드라인](https://support.google.com/webmasters/answer/35769)에 위반되는 사항이다.

## a11y-hidden
- 이렇게 많은 속성들이 숨김처리를 할 수 있지만 접근성을 고려했을때 사용할 수 없는 속성들이다.
단일로 지원하는 속성은 아니지만 a11y-hidden이라고 알려진 방법을 통해 해결할 수 있는 방법이 있다.
```css
  .a11y-hidden{
    position: absolute; /*공간을 차지않게 해주며 요소의 display속성을 block으로
     변경해준다.(width와 height값 등을 조절할수 있다)*/
    overflow: hidden; /*container밖으로 빠져나가는 내부 요소들을 숨김처리 해준다.*/
    border: 0; /*테두리 삭제*/
    margin: 0; /*바깥 여백 삭제*/
    width: 1px; /*box의 가로너비를 최소한의 크기로 줄여줌*/
    height: 1px; /*box의 세로너비를 최소한의 크기로 줄여줌*/
    clip-path: polygon(0 0, 0 0, 0 0);/*polygon의 값을 3개로 주어 삼각형 세 꼭지점의
     xy축의 값을 0으로 지정해 각 축은 생성되어 요소는 실제로 있지만 보이지않는 요소로 만들어준다. */

    /* 구형 브라우저를 위해서 아래 코드를 추가해줘도 좋다. */
    clip: rect(0 0 0 0);
    clip: rect(0, 0, 0, 0);
  }
```

  - 위의 코드처럼 a11y-hidden으로 불리는 방법도 공식적으로 의미가있는 요소를 숨기기위해 나온것은 아니다.
  나중에는 접근성을 위해 숨기는 요소를 위해 사용할 수 있는 속성이 지원되면 좋겠다는 생각을 한다.