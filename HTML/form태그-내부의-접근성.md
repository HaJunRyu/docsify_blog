# form태그 내부의 접근성

- form태그는 HTML문서내에서 입력서식 등 사용자가 입력한값을 다양한 형식으로 서버와 소통하기 위해 사용되는 태그이다.

- form태그 내부의 주요 속성들이나 태그들도 중요하지만 이번엔 잘 모를수있는 form태그내에서 웹 접근성을 고려한 구조에 대해 다뤄보려고 한다.

## fieldset

- fieldset태그는 관련있는 서식들끼리 그룹화해줄 수 있는 요소이다. 열리는 태그`<fieldset>`와 닫히는 태그`</fieldset>`가 존재하며 내부 요소들로 관련있는 서식들끼리 그룹화 해줄 수 있다.

- 하지만 그냥 묶어주는것만으로는 웹 접근성과 아무 관련이 없을수 있다. 그래서 fieldset을 쓸때는 legend속성을 꼭 같이 사용해줘야 한다. legend태그도 동일하게 열리는 태그`<legend>`와 닫히는 태그`</legend>`가 있고 내부에 fieldset으로 묶어준 서식그룹의 설명을 적어준다.
```html
<form action="login.do" method="POST">
  <fieldset>
    <legend>회원 로그인 폼</legend>
    <input type="text" name="id" placeholder="아이디를 입력해주세요.">
    <input type="password" name="pwd" placeholder="비밀번호를 입력해주세요.">
  </fieldset>
</form>
```
- 위 코드와 같이 fieldset과 legend 태그를 사용해주면 저시력자 등 장애가 있는 사용자가 이용해도 스크린리더로 '회원 로그인 폼' 인것을 인지할수있게된다.

- 하지만 위의 코드에서 또 잘못된것이 있다. input태그를 보면 placeholder로 아이디와 비밀번호를 입력하는 서식이라고 명시해놨지만 placeholder는 입력서식의 힌트역할만 할 뿐 의미를 부여하진 못한다. 일반 사용자들은 문제없이 사용할수도 있겠지만 장애가 있는 사용자들은 어떤것을 입력해야하는곳인지 알아채지 못한다는것이다.

- input요소가 스크린리더 환경에서도 어떤 입력서식인지 알려주기 위해서는 label태그를 이용해야한다. 명시적으로 표기해주는 방법과 암묵적으로 표기해주는 방법이 있다.

- <u>명시적 표기법</u>
```html
<form action="login.do" method="POST">
  <fieldset>
    <legend>회원 로그인 폼</legend>
    <div>
      <label for="id">아이디</label>
      <input id="id" type="text" name="id" placeholder="아이디를 입력해주세요.">
    </div>
    <div>
      <label for="pwd">비밀번호</label>
      <input id="pwd" type="password" name="pwd" placeholder="비밀번호를 입력해주세요.">
    </div>
  </fieldset>
</form>
```
위 코드처럼 input의 id속성의 값과 label의 for속성의 값을 똑같이 적어줘 연결해주는 방법이 명시적 표기법이다.

- <u>암묵적 표기법</u>
```html
<form action="login.do" method="POST">
  <fieldset>
    <legend>회원 로그인 폼</legend>
    <div>
      <label>아이디
        <input type="text" name="id" placeholder="아이디를 입력해주세요.">
      </label>
    </div>
    <div>
      <label>비밀번호
        <input type="password" name="pwd" placeholder="비밀번호를 입력해주세요.">
      </label>
    </div>
  </fieldset>
</form>
```
위 코드처럼 id와 for속성을 따로 명시하지 않고 label태그 내부 요소로 input을 사용해주는것이다.

- 위의 방법을 쓰면 원치않는 레이아웃 결과가 나올수도 있다. 만약 그렇게 된다면 요소들을 숨김처리 해줘야하는데 접근성을 고려해서 숨김처리를 하려면 [a11y-hidden](https://lovediv.tistory.com/2)방식을 사용해야하는데 width와 height값을 조정해야 한다. 하지만 legend의 기본display속성은 table이다. 이같은 경우엔 특정 속성을 사용하여 inline-block이나 block으로 display속성을 변경해야 숨김처리가 가능하다.