# abbr(abbreviation)

- 축약어를 쓸 때 사용하는 태그다.
title 속성을 사용하여 축약어의 설명을 써준다.

사용법
```html
<abbr title="버스정류장">버정<abbr>
<abbr title="버스카드">버카<abbr>
<abbr title="문화센터">문센<abbr>
<abbr title="block formatting context">BFC<abbr>
<abbr title="Britain + Exit">Brexit</abbr>
```

이런식으로 화면에 노출시켜주는 text를 태그 사이에 적어주고 title의 값으로 축약어에 대한 설명 또는 풀네임을 적어준다. 웹에서 abbr요소에 마우스를 올려놓으면 title에 써놓은 값이 노출되게 된다.

`<abbr>`의 display속성
```css
abbr{
  display: inline;
}
```
