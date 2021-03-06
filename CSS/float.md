# float

## float의 정의와 사용 방법

- 요소를 좌우 방향으로 띄우는 속성(수평 정렬)

요소를 부유시키는(띄우는)용도이지만 수평 정렬을 하는 용도로 더 많이 새용해왔다.

- float의 작동원리는 인접요소에 linebox가 생성되는데 left값을 줬을때 그 linebox의 왼쪽으로 붙어서 배치가 되는것이다.

- 선언 방식
```css
div {
  float: left; /*요소를 왼쪽으로 띄움 (또는 왼쪽으로 정렬)*/
}
```

- display속성이 수정된다.

float속성이 추가된 요소는 display속성의 값이 대부분 block으로 수정된다.(flex, flex-inline등의 요소는 수정되지 않는다.)

예를 들어 span태그에 float속성을 주면 block요소로 바뀌어 width와 height, margin등의 속성값도 적용이 가능해진다.

```html
<style>
  .float-left {
    float: left;
    width: 300px;
    height: 200px;
    margin: 10px;
  }
</style>
<span class="float-left">
현재 이 요소는 block요소의 상태가 되었습니다.
</span>
```

## float의 해제

- float의 해제방법에는 대표적으로 3가지가 있다.

### 다음 형제요소에서 clear시키는 방법

- 간단하게 float이 적용된 요소의 다음 형제요소에서 claer를 해주면된다.

```html
<style>
  .float-left {
    float: left;
  }
  .clearfix{
    clear: both;
  }
</style>
<div>
  <span class="float-left">float영역입니다.</span>
  <span class="clearfix">그 다음 형제 요소입니다.</span>
</div>
```

### 부모요소에서 가상클래스(after)를 사용하는 방법

위와 개념은 비슷하지만 가상요소선택자로 markup을 더 효율적으로 작성할 수 있는 장점이 있고 만약 float영역 다음에 형제요소가 존재하지 않을때도 자유롭게 쓸 수 있는 방법이다.

```html
<style>
  .float-left {
    float: left;
  }
  .clearfix::after{
    content: ""; /*가상요소선택자는 content속성이 없으면 작동하지 않는다.*/
    clear: both; /*left, right 모두 float을 해제해주는 값이다.*/
    display: block; /*가상요소선택자로 생성된 요소는 기본적으로 inline속성을 가졌기 때문에 
    원치않는 레이아웃 구조로 표시될수도 있으므로 block속성을 추가해준다.*/
  }
</style>
<div class="float-left clearfix">
  <span>float영역1 입니다.</span>
  <span>float영역2 입니다.</span>
  <span>float영역3 입니다.</span>
</div>
```

- 단 이 방법으로 float을 해제했을때 일어날 수 있는 현상은 원치않는 요소가 생기기때문에 flex등의 방법으로 레이아웃을 짜려고할때 개발자가 원치않는 구조로 결과가 나올수도 있다.

### 부모요소에 overflow속성을 사용하여 해제하는 방법

- overflow라는 속성이 float와는 관련없는 속성이지만 [BFC](https://blueshw.github.io/2020/05/17/know-css-block-formatting-context/)(block formatting context) (출처 : bobo's blog)에 의하여 부모요소가 float의 너비를 인식하게끔 할 수있다.
overflow의 값으로는 visible을 제외한값은 모두 가능하다.

```html
<style>
  .overflow {
    overflow: hidden;
  }
  .float-left {
    float: left;
  }

</style>
<div class="overflow">
  <span class="float-left">float영역 입니다.</span>
</div>
```