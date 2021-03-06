# 새로운 표준 HTML5

## 시맨틱(semantic)태그를 사용
- 기존의 HTML의 표준과 다르게 의미없는 div태그만을 사용하는게 아닌 콘텐츠마다 의미가 맞는 태그를 사용해줘야 한다.

잘못된 예
```html
<!-- header와 nav영역을 의미없는 div태그로 사용한 경우 -->
<div class="header">
  <div class="nav">navigation var</div>
</div>
```

HTML5의 올바른 예
```html
<!-- 사용하려는 영역에 맞게 header태그와 nav태그를 사용한 경우 -->
<header class="header">
  <nav class="navigation">navigation var</nav>
</header>
```

## 웹페이지의 구조

- 3단 구조

```html
<header>header</header>
<main>main</main>
<footer>footer</footer>
```

- 4단 구조

```html
<header>header</header>
<nav>nav</nav>
<main>main</main>
<footer>footer</footer>
```

만약 nav영역을 footer의 위에 두고 싶다고해도 HTML은 구조를 잡는 용도이기 때문에
태그의 위치는 위와 같이 작성하고 CSS를 이용하여 위치(레이아웃)를 변경해준다.

## 구조적 논리적인 순서

- 대화형 컨텐츠를 만들때 레이아웃을 어떻게 배치하느냐에 상관없이 맥락에 맞게 구조를 짜야한다.

- 로그인 창의 레이아웃을 만들때

잘못된 예
```html
<div class="login_box">
  <form>
    <input class="login_form"/>
    <input type="submit" class="sign_in"/> <!-- 로그인을 오른쪽 옆으로 배치하기 위해 맥락을 중간에 끼워넣음 -->
    <input type="password" class="pwd_form"/>
  </form>
<div>
```

올바른 예
```html
<div class="login_box">
  <form>
    <input class="login_form"/>
    <input type="password" class="pwd_form"/>
    <input type="submit" class="sign_in"/> <!-- id, password 다음에 sign_in 버튼을 배치 -->
  </form>
<div>
```

## CSS 방법론 BEM

[BEM](http://getbem.com/)
[BEM사용법](https://nykim.work/15) 출처 : https://nykim.work/15(nana_log님 블로그)

- css적용을 위해 class명을 지을때 하나의 프로젝트에서 약속하듯이 방법론을 사용한다.
여러가지 방법론(SAMCSS, BEM, OOCSS등)이 존재하지만 BEM을 예시로 들겠다.

부모와 자식을 언더바(underbar) 2개로 구분하여 이름을 지어준다.
```html
<!-- header의 자식 nav, nav의 후손 li -->
<header class="header">
  <nav class="header__nav">
    <ul>
      <li class="header__nav__list-item1"></li>
      <li class="header__nav__list-item2"></li>
      <li class="header__nav__list-item3"></li>
    </ul>
  </nav>
</header>
```

### 수업중 질문

- main 태그는 html하나의 문서에서 하나만 사용할 수 있다.
정답 : O
말 그대로 하나의 문서에서는 main contents의 영역이 하나만 존재할 수 있다.

- header 와 footer태그는 html하나의 문서에서 하나씩만 사용할 수 있다.
정답 : X
header와 footer같은 경우 하나의 문서에서 section을 나눠서 사용할경우 해당 section의 header, footer
를 각각 부여하는 경우도 있기때문에 꼭 한번만 쓰이는것은 아니다.