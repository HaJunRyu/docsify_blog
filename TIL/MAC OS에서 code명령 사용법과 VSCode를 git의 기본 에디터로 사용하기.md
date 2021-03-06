---
title: MAC OS에서 code명령 사용법과 VSCode를 git의 기본 에디터로 사용하기
date: 2021-03-03 10:31:02
tags: VSCode
---

# MAC OS에서 code명령 사용법과 VSCode를 git의 기본 에디터로 사용하기

**해당글은 zsh을 사용하는 MAC OS를 기준으로 작성되었습니다.**

## 터미널에서 code 명령어 사용

### PATH에 code 명령 설치
VS Code를 켠 상태에서 (⌘ command + ⇧ Shift + P)를 눌러 명령 팔레트를 띄운 후 shall이라고 검색하면 아래와 같은 명령이 나온다.
![image](https://user-images.githubusercontent.com/71176945/109739280-ba2e9980-7c0c-11eb-9f0b-b062300e21e4.png)

위 명령을 눌러 실행하면 VS Code가 잘 설치되어 있는 경우 터미널에서 code 명령어를 사용할 수 있을것이다.

하지만 나의 경우엔 VS Code를 완전히 종료했다가 다시 켜거나 컴퓨터를 재부팅 했을 경우 터미널에서 code명령어를 사용해보면 code명령어를 찾을 수 없다는 메세지가 뜨고 다시 VS Code에 가서 PATH에 code 명령어를 설치해주는 행위를 반복해주어야하는 이슈가 있었다.

![image](https://user-images.githubusercontent.com/71176945/109741676-e21ffc00-7c10-11eb-9561-022d9cef6ea0.png)
<center>↑ 재부팅 후 다시 code명령어를 쳐보았을때의 결과 ↑</center>  

### zsh설정 파일 변경

만약 그냥 단순히 터미널에서 code 명령어를 사용하고 싶은것이라면 해당 이슈에 대해 2가지 방법이 있다.

2가지 방법은 공통적으로 zsh설정파일(터미널 설정파일)에 접근해 설정을 추가해주어야한다.

1. `vim ~/.zshrc` 명령을 터미널에 입력 후 i 버튼을 눌러 insert모드로 전환한다.  

  그리고 아래 1-1과 1-2의 방법 중 하나를 선택하자 (내부동작은 다를 수 있으나 단순히 code명령어를 사용하기 위함이라면 사용할때의 동작은 동일하다.)  

  1-1. 최하단에 아래의 명령어 추가
  (꼭 최하단이 아니어도 된다. 가독성을 위함이고 앞에 #표시가 없는것을 꼭 확인하자)
  ```
  code () { VSCODE_CWD="$PWD" open -n -b "com.microsoft.VSCode" --args $* ;}
  ```
  
  1.2 최하단에 아래의 명령어 추가
  (마찬가지로 꼭 최하단이 아니어도 됨)
  ```
  alias code="/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code"
  ```

2. esc버튼을 누룬뒤 `:wq` 명령어로 문서를 저장 후 빠져나온다.  

3. 터미널을 재시작 한 후 `cd`명령어를 통해 작업을 하고싶은 폴더에 접근하여 `code .` 명령어를 작성하면 VS Code가 정상적으로 잘 작동할것이다.

하지만 단순히 code 명령어를 쓰는게 아닌 git commit을 남길 editor를 VS Code로 사용할때 즉, .gitconfig파일에서 기본 editor설정을 code 명령어로 사용하고 있을때 다시 code 명령어를 찾지 못하는 이슈가 발생했다.

## VS Code를 git의 기본 editor로 설정

아마 위의 작업만 해도 잘 되는 PC가 있을것이다. 사실 처음에 VS Code설치를 잘했으면 정상적으로 일어나는 동작이다.

### .gitconfig파일의 core.editor설정
일단 터미널 명령을 통해 .gitconfig파일의 core.editor 설정을 변경해주자.
`git config --global core.editor "code --wait"`

이제 code 명령어는 사용할 수 있으니 code 명령어를 이용해 .gitconfig를 열어보자.

`code ~/.gitconfig`

위의 명령어가 잘 실행되었다면 .gitconfig파일을 열어보면 아래와 같이 설정이 되어 있을것이다.

![image](https://user-images.githubusercontent.com/71176945/109742913-3926d080-7c13-11eb-9c19-6e1785ebd6a8.png)

그리고 이제 code명령어가 초기화 됐던 상황과 똑같이 컴퓨터를 재부팅 해보자.

새로운 git 로컬저장소를 만든 후 아무 파일이나 만든 후 add한 뒤 터미널에서 `git commit` 명령어를 입력해보자.

나의 PC에서는 아래 사진과 같은 이슈가 발생하였다.

![image](https://user-images.githubusercontent.com/71176945/109742698-dfbea180-7c12-11eb-88ef-0999133ac701.png)

이 문제를 해결하기 위해 한참을 구글링을 하며 작업을 해보았고 가장 많이 나온 해결책은 수동으로 환경변수를 변경하라는것이었는데 오히려 환경변수가 잘못 설정되어 터미널 자체를 못쓰게 됐던분도 있었고 그것의 관련 포스팅으로 환경변수를 초기화하여 터미널을 살리는 포스팅까지 작성하셨던데ㅋㅋㅋ...

결론은 PATH경로에 접근하여 수동으로 환경변수도 바꿔줘봤지만 해당 이슈가 해결되진 않았다.

### Visual Studio Code.app 파일의 위치 변경

그래서 MS의 공식문서를 살펴보던 중 Visual Studio Code.app 파일의 경로가 Application(응용프로그램)폴더여야 한다는 내용을 발견했다. 사실 이건 한달 전 쯤에도 봤던 문서이고 그때 다운로드 폴더에 존재했던 Visual Studio Code.app파일을 터미널의 `mv` 명령어를 통해 Application폴더로 이동시켰던 기억이 있다.

근데 이번 계기를 통해 다시 확인을 해보니 Applicion이 각각의 다른 경로로 2개가 존재했다. 

1. 사용자 폴더 경로에 있는 Application 폴더(~이란 표시는 root/Users/사용자/ 경로라는 뜻이다.)
![image](https://user-images.githubusercontent.com/71176945/109744141-2ad9b400-7c15-11eb-8dad-eaa552aad2a7.png)

2. root(최상단) 경로에 있는 Application 폴더
![image](https://user-images.githubusercontent.com/71176945/109744089-1695b700-7c15-11eb-8343-d5fcbd71187e.png)

그래서 결국은 root/Users/hajun/Application/ 경로에 있는 Visual Studio Code.app 파일을 `mv` 명령어를 이용하여 root/Application/ 경로로 이동시켜주었고 다시 VS Code에서 명령 팔레트를 켜고 PATH에 code 명령어를 설치해주니 재부팅을 해도 git commit 메세지를 VS Code로 작성할 수 있게 되었다.

<center>⬇ commit 메세지를 작성하고 editor(commit message파일)가 종료 될때까지 대기 ⬇</center>

![image](https://user-images.githubusercontent.com/71176945/109744619-f5819600-7c15-11eb-86a3-86c01c04c5b0.png)

<center>⬇ VS Code에서 commit message 작성 ⬇</center>

![image](https://user-images.githubusercontent.com/71176945/109744763-34175080-7c16-11eb-8286-d6015b74a4fa.png)

<center>⬇ commit message를 다 작성한 후 editor(commit message파일)를 종료하면 아래와 같이 commit이 완료된다. ⬇</center>

![image](https://user-images.githubusercontent.com/71176945/109744816-42fe0300-7c16-11eb-85b8-2018a9ec2d0f.png)

## 결론

Mac OS를 사용한다면 아마 처음에 Visual Studio Code를 설치할때 [공식페이지](https://code.visualstudio.com/)에서 간단히 Download for Mac(또는 window)버튼을 눌러 설치할것이다.
Download 된 파일은 기본적으로 Download폴더에 위치하게 된다. 그리고 그 파일을 압축해제하면 Visual Studio Code.app이라는 파일이 생성될것이고 그대로 VS Code를 사용할 수 있게된다.
그래서 흔히들 Visual Studio Code.app 파일의 위치(경로)가 root/Users/사용자/Download인 채로 사용하게 되는데 나의 경우에는 이것을 인지하고 Application폴더로 이동해주었지만 그 폴더 또한 올바른 Application폴더가 아니었던것이다....  

추측이지만 code명령어는 root/Applications/Visual Studio Code.app/Contents/Resources/app/bin/ 경로에서 code 파일을 찾아 작동하는것 같다. 그래서 Visual Studio Code.app 파일이 올바른 경로에 있지 않을때 code 명령어가 정상적으로 작동하지 않는것 같다.

그래서 지금 이 단계까지 잘 왔다면 본문의 상단에서 작성한 .zshrc 파일에서 code명령어를 수동으로 설정했던 행위도 필요하지 않게 된다.

그냥 놔둬도 되지만 거슬려서 다시 .zshrc 파일에 접근하여 주석처리를 해주었다.

`vim ~/.zshrc` or `code ~/.zshrc`

<center>⬇ #을 붙혀주면 주석처리가 된다. ⬇</center>  

![image](https://user-images.githubusercontent.com/71176945/109746686-24e5d200-7c19-11eb-8e21-25a840c366a7.png)

그래서 진짜 결론은 응용프로그램을 설치할때는 처음부터 잘하자ㅎㅎ
