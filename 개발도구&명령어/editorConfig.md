# editorConfig
프로젝트의 컨벤션을 유지하기 위해서 사용되는 확장 프로그램인데 `.editorconfig`라는 파일에 작성된 설정들을 기반으로 작동된다. 이 파일이 프로젝트의 디렉토리의 root경로에 위치하게 되면 Editor나 IDE의 전역설정보다 우선하여 코드스타일을 유지, 관리할 수 있다. 즉, 개발환경을 배포할때 root경로에 함께 넣어 배포한다면 협업하는 프로젝트에서 간단하게 직접 팀원들의 프로젝트에 손을 대지 않고 컨벤션을 유지, 관리하며 개발을 진행할 수 있다.  

## VS Code에서의 사용법
1. 확장 프로그램 설치 탭에 들어가 검색창에 `EditorConfig for VS Code`를 검색하여 설치
2. VS Code내에서 사용하려는 프로젝트의 root경로에서 마우스 우측 클릭을 한 후 `Generate .editorconfig` 클릭
3. 생성된 `.editorconfig` 파일의 설정을 수정(수정해야 할 옵션이 있을경우)
`Generate .editorconfig` 명령으로 생성된 파일의 기본값
```json
root = true // 값이 false라면 현재 폴더로부터 root = true 옵션이 있는 .editconfig파일을
// 찾을때까지 상위 폴더로 계속 올라가며 .editconfig 파일들의 옵션값들을 가져와 덮어쓰기함
// 보통 프로젝트 폴더 단위로 root = true를 지정해주고 시작하는 경우가 일반적이다.

[*]
indent_style = space //space or tab
indent_size = 2 // number값으로 설정
charset = utf-8 // latin1, utf-8, utf-8-bom, utf-16be, utf-16le
trim_trailing_whitespace = false // 공백 문자 제거 여부
insert_final_newline = false // 파일 맨마지막에 newline을 추가할지 여부
```

추가로 아래의 옵션도 적용해줄 수 있다.
```json
end_of_line = lf // lf, cr, crlf 선택
```
옵션을 추가해 개행 옵션을 지정해주면 되는데 아마 window 환경을 가진 PC에 배포할때 lf를 지정해주어 crlf방식을 사용하지 않고 lf로 쓰게끔 하고싶을때 유용할것 같다.
  