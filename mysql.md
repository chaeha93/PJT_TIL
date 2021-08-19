## AWS EC2에 MySQL 설치하기
1. ubuntu 패키지 정보 업데이트
> sudo apt update
2. mysql 설치
> sudo apt install mysql-server
3. mysql 설치 확인
> dpkg -l | grep mysql-server
4. mysql 실행여부 확인
> sudo netstat -tap | grep mysql
5. mysql 접속
> mysql -u root -p
6. database 접속
> use ssafy_web_db;
