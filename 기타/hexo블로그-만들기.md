# hexo블로그 만들기


hexo블로그를 만들기 전 먼저 [git](https://gitforwindows.org/) 과 [node.js](https://nodejs.org/en/)를 설치해주시고 [github](https://github.com/)에 가입까지 완료해야한다.
그리고 파일을 수정해 줄 에디터도 필요한데 여기선 VScode를 기준으로 사용하겠다.

## git bash를 통해 hexo설치


1. cd(change directory)를 통해 설치를 원하는 폴더로 이동
<pre>예) $ cd dev/</pre>

2. 설치
<pre>$ npm install -g hexo-cli</pre>

만약 설치가 안될경우 명령어 앞에 sudo를 붙혀야한다.

3. 블로그 폴더 생성
<pre>$ hexo init blog</pre>

4. ls를 입력하여 폴더가 잘 생성되었는지 확인한다.
<pre>$ ls</pre>

5. 확인이 되었다면 이번엔 노드 모듈을 설치해준다.
<pre>npm install</pre>

6. hexo server를 구동시켜 localhost:4000포트에서 생성이 잘 되었는지 확인한다.
<pre>$ hexo server</pre>
 
 http://localhost:4000 으로 들어가서 확인한다.
 이때 복사를 한다고 ctrl + c 를 하면 복사도 안되고 서버가 종료되니 조심해야한다.
 (처음엔 서버 꺼진줄 모르고 왜 404가 뜨는지 ^^...)

7. 새로운 포스트 생성(md파일) 
<pre>$ hexo new post "my new post"</pre>
""안의 내용은 문서의 제목을 의미하니 원하는 제목을 입력하고 enter를 눌러주면 된다.

파일이 잘 생성됐는지 cd와 ls를 통해 확인해준다.
파일은 blog/source/_posts 폴더안에 생성이 된다.

8. 내용 입력
<pre>~/Documents/dev/blog$ code source/_posts/.</pre>
VScode로 포스트파일이 있는 폴더를 열어서 새로 만들어준 게시글을 markdown 문법에 맞춰 작성해준다.

9. 블로그에 적용시켜보기
git bash에서 다시 blog폴더로 돌아가 블로그에 변경사항은 적용시켜준다.
<pre>~/Documents/dev/blog$ hexo clean && hexo generate</pre>

10. 적용된 파일을 확인
hexo server를 통해 localhost:4000으로 확인한다.
- 이제 기본적인 블로그의 틀을 갖췄으니 github를 통해 실제 블로그를 만들어보자


## github pages로 배포하기(실제 블로그 만들기)

1. github repository생성(이하 repo)

- github에서 repo의 이름을 "사용자명.github.io" 공개범위는 public으로 생성한다. (""는 제외)
<pre>예) HaJunRyu.github.io</pre>

2. git bash에서 pages설치

<pre>~/Documents/dev/blog$ npm install hexo-deplyer-git --save</pre>

3. _config.yml에서 deploy항목 변경해주기
- _config.yml 파일에서 가장 하단에 deploy 항목으로 가면 type과 repo가 있는데 아래처럼 변경해준다.
(띄워쓰기 및 오타 주의)
<pre>
  deploy: 
  type: git
  repo: 위에서 설치한 repository주소
  예) repo: https://github.com/HaJunRyu/HaJunRyu.github.io.git
</pre>

4. 블로그 생성
- git bash에서 hexo clean && hexo deploy 입력(블로그에 적용)
<pre>
  ~/Documents/dev/blog$ hexo clean && hexo deploy
</pre>

5. github사용자명.github.io - url로 접근

일정시간(1분 ~ 길면 5분) 경과 후 생성된 블로그 url확인
<pre>예) HaJunRyu.github.io 검색</pre>

아까 만들었던 포스트와 함께 github.io의 도메인으로 개인블로그가 만들어진걸 확인할 수 있다.
