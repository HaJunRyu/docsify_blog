# 2021년 07월 19일 월요일 TIL(Snackbar UI, 나만의 유튜브 강의실 2단계 완성)

## 오늘 한 일과 느낀점

### Snackbar UI

프론트엔드 개발을 하다보면 계속 새로운 UI 패턴들을 알게 되는것 같다. Toggle side nav, Accordion, Skeleton 등등... 계속적으로 새로운걸 알게되고 오늘 처음 적용해본 Snackbar UI는 처음에는 Toast UI와 비슷하게 생겨 이름이 따로 존재하는지 몰랐다. 근데 사실 그렇게 흥미가 가는 UI 패턴은 아닌것 같다. 그냥 심플하고 깔끔하게 정보를 제공할 수 있는? 알아두면 유용한 UI인것 같다!

### 나만의 유튜브 강의실 2단계 완성

다시 블랙커피 스터디 Lv3가 시작된지 2주정도 지났다. 지난주에 나만의 유튜브 강의실 1단계 미션을 끝내고 코드리뷰를 진행했고 이번주는 2단계 미션으로 라우팅 기능과 상태값을 제어하여 해당 라우팅에 적절한 렌더링을 해주면 되는 요구사항이 있었다. 사실 힘든건 1단계때 프로젝트의 구조와 아키텍쳐를 짜는 일이었고 2단계는... 그냥 그 아키텍쳐로 개발을 하는게 끝이라 쉬웠다. 고치고 싶은 부분이 있긴하다. 현재 diff알고리즘을 적용해 변경된 부분만 리렌더링 하게끔 구현이 되어있는데 list 렌더링 시 list내의 앞쪽에 item요소가 사라지거나 새로 생길때 그 뒤에 위치해있는 item들은 모두 다 리렌더링이 되어서 React에서 key props를 이용한것처럼 해결해보고 싶은데 아직 감도 안잡히고 사실 계속 이렇게 해도되나 싶다..... 요즘 React의 개념은 공부도 잘 안하고 특히 상태를 관리할때 비동기 처리가 들어갈 경우 사용해본것이 redux-thunk밖에 없어 saga도 공부해야하고 알고리즘도 꾸준히 해야하고... CS지식도 많이 부족하고 그냥 부족한거 천지다,,,, 어쩃든 본론으로 돌아와서 그것뿐만 아니라 JSX를 babel이 트랜스파일링 해주니 리액트를 쓰지않고도 JSX문법을 쓸 수 있는지에 대해서도 공부해보고 된다면 적용을 해보고싶긴하다 이런저런 생각으로 초큼 우울한 하루의 마무리이다...

## 내일 할 일

- redux-saga로 아주 간단한 데이터 fetch 시도해보기
- 테스트에 대한 개념 알아보기
- 블랙 커피 미션 saveVideoSection에 있는 이벤트 핸들러 리팩토링 하기
