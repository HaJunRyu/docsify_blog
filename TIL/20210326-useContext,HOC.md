# 2021년 03월 25일 금요일 TIL (useContext, HOC)

## 오늘 한 일과 느낀점
- 오전 10시 ~ 오후 5시 리액트 강의  
어제 다뤘던 Context API를 그냥 단순히 Provider와 Consumer를 이용하여 값을 공유하는것이 아닌 하나의 Context를 다룰 파일을 따로 만든 뒤 파일내에서 초기상태와 reducer함수를 만들어 useReducer로 state와 dispatch를 생성 후 reducer함수에서 사용 할 액션의 타입을 상수로 선언하고 dispatch함수를 호출할때 일일히 액션객체를 작성해주는것이 아닌 액션 크리에이터 함수를 만들어 재사용할 수 있는 패턴을 만들었다. 그 후 Context 생성, Context의 Provider의 vaule에 state와 dispatch를 담아주어 그것을 반환하는 랩핑 컴포넌트를 생성하고 그 value를 Provider내부가 아니더라도 사용할 수 있게끔 useContext나 HOC를 활용하여 Context의 값을 볼 수 있게끔 하는 실습을 진행했다.

## 내일 할 일
- 프로젝트 구상을 하며 어떤 기능이 필요한지 상의하며 요구사항 명세 작성해보기
- 간단한 TotoList 만들어보기
