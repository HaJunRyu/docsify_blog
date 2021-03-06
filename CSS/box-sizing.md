# box-sizing

- box-sizing은 요소의 크기를 계산하는 기준(화면에 표시되는 방식)을 지정해주는 속성이다. 3가지 속성이 있지만 호환성의 문제때문에 대표적으로는 2가지가 쓰인다.

- 아래에서 설명하겠지만 defalut값인 content-box를 쓸 경우 padding과 border를 줬을때 컨텐츠의 크기를 계산하는데 실수를 하는 경우가 있어 `*{box-sizing: border-box;}`로 선언하고 쓰는 경우가 많아졌다.(`*`을 쓰는 이유는 box-sizing은 상속이 되는 속성이 아니기때문에 모든 요소 `*` 를 선택자로 쓰는것이다.)

## content-box
- width와 height값으로 정의한 너비만으로 요소의 크기를 계산하고 padding(안쪽여백)과 border(테두리)의 속성이 추가됐을때 요소의 크기가 늘어난다.

아래의 경우처럼 콘텐츠 요소(.content-box)의 속성을 width와 height를 300px씩으로 만들었을때 padding 20px, border 10px의 속성을 추가해 줄 경우 요소의 가로, 세로의 총 너비는 360px이 된다.
((콘텐츠 요소)300px + (padding)20px`*`2 + (border)10px`*`2)
```html
<style>
  .content-box{
    box-sizing: content-box;
    width: 300px;
    height: 300px;
    padding: 20px;
    border: 10px solid red;
  }
</style>
<div class="content-box">content-box는 대부분 요소들의 기본값입니다.</div>
```

## border-box
- width와 height값으로 지정한 너비가 padding(안쪽여백)과 border(테두리)의 너비까지 포함해서 요소의 크기를 결정한다.(margin은 포함되지 않는다.)

아래의 경우처럼 콘텐츠 요소(.border-box)의 속성을 width와 height를 300px씩으로 만들었을때 padding 20px, border 10px의 속성을 추가해 줄 경우 padding의 너비와 border의 너비만큼 콘텐츠 영역의 너비가 줄어들고 결국 콘텐츠 요소의 가로, 세로의 총 너비는 width와 height에서 지정해준 300px이 된다. (영역과 요소는 다른것을 가르킴)
((콘텐츠 영역)240px + (padding)20px`*`2 + (border)10px`*`2)
```html
<style>
  .border-box{
    box-sizing: border-box;
    width: 300px;
    height: 300px;
    padding: 20px;
    border: 10px solid red;
  }
</style>
<div class="border-box">border-box는 요소의 총 크기를 계산하기 아주 편리합니다.</div>
```

## padding-box
- 패딩박스는 border-box의 개념에서 border를 제외하고 padding영역까지만 요소의 너비를 계산한다고 생각하면 된다.
하지만 브라우저 호환성이 좋지 않아 현재는 실제로 못쓴다고 생각하면 된다.

## input 태그의 type="text"와 type="button" 태그의 차이
- 위에서 content-box는 대부분의 요소에서 default값으로 작용한다고 했다. 하지만 특정 속성에 의해서 box-sizing: boder-box를 명시하지 않아도 default값이 boder-box가 되는 경우가 있는데 [codepen](https://codepen.io/pen/)에서  input태그는 같은 태그에서도 속성에 따라 box-sizing이 변경되는걸 발견했다.

아래처럼 input태그의 text와 button타입에 같은 속성을 주었다. box-sizing은 명시를 하지 않았고 결과를 개발자메뉴에서 확인해보니 text타입은 요소의 총 크기가 360px이 나왔고 button타입은 width와 height에서 명시된 300px로 나왔다.
```html
<style>
  input{
    width: 300px;
    height: 300px;
    padding: 20px;
    border: 10px solid red;
  }
</style>
<input type="text"> <!-- box-sizing: content-box -->
<input type="button"> <!-- box-sizing: border-box -->
```