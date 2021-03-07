# HTMLHint

ESLint과 비슷한 개념으로 HTML을 위한 정적 분석 도구이다. HTML문서는 문법상 틀린부분이 있어도 오류를 발생시키지 않고 개발자가 이것을 눈으로 발견하기도 쉽지 않다. 크롬 브라우저에서 Web Developer 확장 프로그램을 이용해 검사할 수 있겠지만 HTMLHint를 쓰면 개발단계에서 설정파일에 지정한 오류들을 사전에 발견하고 수정할 수 있다. 물론 다 문서를 다 작성한 후 크롬 브라우저에서 Web Developer 확장 프로그램으로 한번 더 검사하는것이 확실할 것 같다.

## 설치
1. VS Code 확장프로그램 설치 탭에 들어가 HTMLHint를 설치해준다.

2. npm install
전역 설치와 로컬 설치 중 선택
- 전역 `npm i -g htmlhint`
- 로컬 `npm i -D htmlhint`

## 설정
1. 프로젝트의 root 경로에 .htmlhintrc파일 생성
2. 설정파일 작성
**[설정파일 rules](https://htmlhint.com/docs/user-guide/list-rules)**
- 예시
```json
{
  "attr-value-not-empty": false
}
```

## 검사
프로젝트 폴더의 모든 html확장자를 검사
`npx htmlhint "**/*.html"`