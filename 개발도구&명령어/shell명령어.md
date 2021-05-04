# Shell 명령어 (CLI)

**명령어의 예시에서 쓰이는 대괄호`[]`는 그안에 값을 쓰고 대괄호는 지우고 사용해줘야한다.**

### CLI에서 scp명령어로 서버에 파일 전송하기
scp로 ~/.ssh/fds.pem ubuntu@[public-ip] 서버에 ~/Downloads/employees.sql 파일을 ~/ 경로에 넣어달라는 명령어  
```sh
scp -i ~/.ssh/fds.pem ~/Downloads/employees.sql ubuntu@[public-ip]:~/
```

#### aws ec2 리눅스(서버) 명령어
- mysql 접속
`mysql -u root -p`  명령어 입력 후 password 입력

- 로컬의 sql파일을 mysql로 읽기 (데이터베이스 생성 및 내용 추가)  
`mysql -u root -p[password] [데이터베이스 이름] < [sql파일 이름.확장자]`

- mysql서버 종료  
`sudo systemctl stop mysql`

- mysql서버 시작  
`sudo systemctl start mysql`

- 현재 메모리의 상태를 표시  
`free -h`

- 디스크 공간 정보를 표시
`df -h`

- 파일의 내용을 원하는 라인만큼만 출력
`head -n [숫자값] employees.sql`

예) 6번 라인의 끝에서 2라인만큼 출력해달라, 즉 5~6번라인을 출력해달라
`head -n 6 employees.sql | tail -n 2 employees.sql`