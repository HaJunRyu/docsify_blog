# dl, dt, dd 태그

- dl, dt, dd는 용어의 설명(설명 목록)을 쓸때 사용한다.

- dl(Description-list)
- dt(Description-Term)
- dd(Description_Description)

## dl, dt, dd 태그의 사용법

- dl은 Desciption-list로 말 그대로 용어들의 목록을 나타낸다. 그 안에 dt(용어)와 dd(용어의 설명)이 한쌍으로 존재하게 되고 key와 value의 묶음이라고 생각하면 쉬울듯하다.

- dl은 한쌍 이상의 dt,dd를 가지고 있어야한다.
```html
<dl>
  <dt>안경</dt>
  <dd>시력이 나쁜 눈을 잘 보이게 하기 위하여나 바람, 먼지, 강한 햇빛 따위를 막기 위하여 눈에 쓰는 물건.</dd>
</dl>
```
이렇게 한쌍 이상 사용해줘야하고 만약 하나의 단어에 2개 이상의 의미가 있다면 dd태그는 하나의 dt태그에 2개이상 사용해줄수도 있다.

- 2개 이상의 의미를 가진 용어의 예
```html
<dl>
  <dt>배</dt>
  <dd>
    사람이나 동물의 몸에서 위장, 창자, 콩팥 따위의 내장이 들어 있는 곳으로 가슴과 엉덩이 사이의 부위.
  </dd>
  <dd>
    사람이나 짐 따위를 싣고 물 위로 떠다니도록 나무나 쇠 따위로 만든 물건. 모양과 쓰임에 따라 보트, 나룻배, 기선(汽船), 군함(軍艦), 화물선, 여객선, 유조선 따위로 나눈다.
  </dd>
  <dd>
    배나무의 열매.
  </dd>
</dl>
```

- 여러 개의 용어가 같은 뜻을 가졌을때 2개 이상의 dt와 하나의 dd를 쌍으로 사용하기도 한다.
```html
<dl>
  <dt>방탄소년단</dt>
  <dt>BTS</dt>
  <dd>한국의 보이그룹</dd>
</dl>
```

## dl태그안에서 사용할 수 있는 태그
- HTML5에서 `<dl>`의 하위요소로 `<dt>`와 `<dd>`만을 허용했다. 하지만 HTML5.2에서 `<div>`를 사용하는걸 허용했다.

- [Mozilla MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Element/dl)문서에서는 `<div>`를 사용하는 목적을 마이크로데이터를 사용할 때, 그룹에 전역 특성을 적용할 때, 아니면 스타일을 적용할 때 유용할 수 있다고 설명하고 있다.
하지만 아무렇게나 막 쓸 수 있는건 아니고 정해진 형식이 있다.
한 쌍의 dt와dd를 자식요소로 사용해야하고 `<div>`와 `<dt>`, `<dd>`가 형제요소로 올 수 없다.
```html
<dl>
  <div>
    <dt></dt>
    <dd></dd>
  </div>
  <div>
    <dt></dt>
    <dd></dd>
  </div>
</dl>
```
`<div>`를 사용하려면 위와같은 방법으로 사용해도 되지만 꼭 사용하지 않아도 될 경우에는
```html
<dl>
  <dt></dt>
  <dd></dd>
  <dt></dt>
  <dd></dd>
</dl>
```
위와 같은 방법으로 사용해도 무관하다.

## 접근성에 관하여
- 요즘 태그들의 속성을 정리할때 접근성에 대해 많이쓰다보니 빼먹을 수 없는 부분인거같다. `<dl>`태그 같은경우 `<ul>`태그와 다르게 모든 환경에서 스크린리더가 리스트로써 인식하지 못하는 경우도 있다.
그래서 항상 용도에 맞게 적절한 태그들을 사용해줘야하고 당연히 화면상 들여쓰기를 할 목적으로 dl이나 ul태그를 사용하면 안된다. markup은 논리적흐름에 맞게 구조를 짜기 위한것이고 항상 레이아웃은 CSS를 이용하는것이 올바른 방법이다. 들여쓰기같은 방법에는 text-indent, margin등 방법을 많을것이다.
