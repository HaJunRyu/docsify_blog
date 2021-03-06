# Vertical-align

## 사용법 및 정의

- vertical-align은 inline요소 또는 table-cell에서의 수직정렬을 위해 사용한다.

- inline level과 table-cell 요소외에는 적용되지 않는 속성이다.(block level은 적용 안됨)

- 수직정렬 하고싶은 inline요소를 선택 후 `vertical-align : baseline` 이런식으로 선언해준다. (값은 유동적으로)

## vertical-align의 값들

- `baseline`
부모의 baseline에 맞추어 해당 요소의 baseline을 정렬한다. 예외가 있는데 재정의된 요소들(iframe, video, img등)의 baseline은 브라우저마다 다르게 작동할 수 있다.

- `top`
해당 요소의 top과 이것의 자손들의 top을 linebox의 top으로 정렬한다.

- `bottom`
해당 요소의 bottom과 이것의 자손들의 bottom을 linebox의 bottom으로 정렬한다.

- `text-top`
해당 요소를 부모요소의 font크기의 top으로 정렬한다.
(strut의 top정렬)

- `text-bottom`
해당 요소를 부모요소의 font크기의 bottom으로 정렬한다.
(strut의 bottom정렬)

- `middle`
아마 middle속성을 쓰다보면 많이 헷갈릴것이다. middle은 flex의 center속성과는 다르게 linebox의 가운데 정렬이 아니다. 공식이 따로 존재한다.
[MDN](https://developer.mozilla.org/ko/docs/Web/CSS/vertical-align)의 명세에 따르면 middle값은 부모의 baseline + x-height / 2 의 위치에 정렬된다고 되어있다. 하지만 부모의 baseline은 내부에 어떤 요소가 들어오느냐에 따라 유동적으로 바뀌는경우가 있어서 개발자의 의도대로 100% 제어하기 힘든부분도 있다.(동작에서 헷갈리는부분이 많음)

- `sub`
해당 요소의 baseline을 부모 폰트의 아랫첨자 위치로 정렬한다.

- `super`
해당 요소의 baseline을 부모 폰트의 윗첨자 위치로 정렬한다.

- 숫자(px, cm등)
 해당 요소의 baseline을 부모의 baseline에서 부여한 값만큼 위로 정렬한다.(0px은 baseline과 같은 값이고 음수값도 가능하다.)

 - percentage(%)
 숫자값을 주는것과 마찬가지로 해당 요소의 baseline을 부모의 baseline에서 부여한 %만큼 위로 정렬한다.(음수값도 가능하다.)

 ## vertical-align을 이해하려면...

 - vertical-align은 단순히 인라인 요소의 수직정렬을 위한 속성이지만 작동원리는 생각보다 단순하지 않다.
 linebox의 생성원리도 알아야하고 부모요소의 font-size와 line-height에 의해서 strut이 생성되는것도 이해해야한다.

 - inline level요소들은 각자의 기본 CSS속성도 다르며 변수가 정말 많다. x-height 및 decender-line에 의한 img태그를 썻을때 알 수 없는 하단 공백도 생각해야하는 등 신경써야할것들이 많다.

 - 다음에 linebox의 생성과 strut의 생성과 함께 vercal-align에 대해서 다시 제대로 포스팅을 해야겠다.