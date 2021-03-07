# Virtual DOM

리액트의 주요 특징 중 하나는 Virtual DOM을 사용한다는 것이다.

## DOM
Document Object Model의 약어이다. 직역해보면 문서를 객체로써 표현하는 방법이다. HTML이나 XML로 작성한다.
웹 브라우저는 이 DOM을 이용하여 JavaScript와 CSS를 적용한다. tree구조로 이루어져 있어 특정 node를 CRUD할 수 있고 그래서 DOM tree라고도 부른다.

<center>DOM tree</center>

![DOM tree](https://res.cloudinary.com/practicaldev/image/fetch/s--B2Ts1hyb--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/http://i67.tinypic.com/2nqegt2.jpg)
<cite>사진 출처 - https://dev.to/karaluton/what-exactly-is-the-dom-jhg</cite>

### DOM의 성능
DOM은 동적 UI에 최적화 되어있지 않다. DOM API는 HTML에서 제공하는것이고 HTML은 기본적으로 정적인 문서이다.
이 정적인 문서를 JavaScript에서 DOM API를 이용하여 동적인 UI를 만들어내는것인데 이 과정을 보면 DOM에 변화가 일어나면 브라우저에서 CSS를 다시 연산하고 리플로우가, 리페인팅이 일어나는데 이 과정에서 시간이 많이 걸리게된다.
만약 애플리케이션의 규모가 커지게 되어 DOM의 상태 변화가 잦아진다면 이것이 사용자에게 충분히 체감이 될 것이다.

## Virtual DOM이란?
DOM이 업데이트되면 브라우저가 CSS를 연산하며 리플로우, 리페인팅이 일어나기 때문에 성능하락이 생기는것이라고 했다.
그래서 실제로 DOM이 업데이트 되는것이 아닌 가상의 DOM을 만들어 실제로 브라우저에서 CSS연산과 리플로우, 리페인팅은 일어나지 않게끔 JavaScript 객체로 DOM 업데이트를 추상화하는것인데 이것이 virtual DOM인 것이다.
virtual DOM은 JavaScript객체이기 때문에 리렌더링 될때 실제 DOM보다 가볍고 빠르다. (그렇다고 무조건적으로 엄청나게 빠른것은 아니라고 한다.)
그래서 이 virtual DOM을 이용하여 데이터를 업데이트 하면 리액트의 비교 알고리즘을 통해 이전에 업데이트 됐던 DOM과 비교한 후 실제로 바뀐 부분만을 실제 DOM에 적용시키는것이다.