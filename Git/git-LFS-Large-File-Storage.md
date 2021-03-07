# git LFS(Large File Storage)
기본적으로 github 원격저장소는 용량의 제한이 있는데 50Mb부터는 Warning이 표시되고 100Mb부터는 push를 할 때 Error가 발생하게 된다.
하지만 상황에 따라 디자인 시안을 원격저장소에 포함시켜야 되는등의 경우에 프로젝트 파일을 올릴때 사용할 수 있는것이 git LFS이다.

일반적인 상황에서는 크게 사용 할 일이 없어보이니 선택적으로 필요할때 사용하면 될 것 같다.

## 설치
  MacOS - `brew install git-lfs`
  Homebrew가 없거나 그 이외의 경우 홈페이지에서 [직접 다운로드](https://git-lfs.github.com/)

## 시작
  `git lfs install`

## 설정
  ```
  git lfs track "*.psd" // 예시로 모든 psd형식의 파일을 lfs관리 파일로 지정
  git add .gitattributes // .gitattributes를 스테이징
  ```

## 설정내용을 원격저장소에 add, commit, push
  ```
  git add file.psd 
  git commit -m "Add design file" 
  git push origin main // branch와 remote명은 본인의 저장소에 맞게끔 작성
  ```