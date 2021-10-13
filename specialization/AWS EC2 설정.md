## AWS EC2 설정

### Java 설치
```
$ sudo apt-get install openjdk-8-jdk
## $ sudo add-apt-repository ppa:webupd8team/java # 패키지가 없으니 패키지 서버를 추가하고 다시 설치(?)
$ java -version
```  

### Maven 설치
```
$ sudo apt update
$ sudo apt install maven
$ mvn -version
```  

### Docker 설치  
참고 : https://docs.docker.com/engine/install/ubuntu/
```
# 이전 버전 제거
$ sudo apt-get remove docker docker-engine docker.io containerd runc  

# HTTPS를 통해 리포지토리를 사용할 수 있도록 패키지 인덱스를 업데이트하고 패키지를 설치
 $ sudo apt-get update
 $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release  

# Docker의 공식 GPG 키 추가
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg  

# 안정적인 저장소 설정
$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  

# 도커 엔진 설치
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io  

# 도커 버전 확인
$ docker version
$ docker -v
```  

### Linux 시스템에 Docker Compose 설치
```
# 다음 명령을 실행하여 Docker Compose의 현재 안정적인 릴리스를 다운로드
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 바이너리에 실행 권한을 적용
$ sudo chmod +x /usr/local/bin/docker-compose
# 설치를 테스트
$ docker-compose --version
```

### Docker를 통한 MySQL 5.7 버전 설치  
참고 : http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/  
1. MySQL Docker 이미지 다운로드
```
$ docker pull mysql:5.7
```
2. 도커 이미지 확인
```
$ docker images
```
3. MySQL Docker 컨테이너 생성 및 실행
``` 
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=surlock -d -p 3306:3306 mysql:5.7
```
4.  도커 컨테이너 MySQL 접속
```
$ docker exec -it {컨테이너 이름} /bin/bash
#  mysql -u root -p
> create database if not exists surlock
``` 

### Docker를 통한 Nginx 설치 (하지 않음)
1. Nginx 최신버전 설치 명령어
```
$ sudo docker pull nginx:latest
```
2. 실행
```
$ docker run --name nginx-test -v /home/mint/share/nginx/html:/usr/share/nginx/html:ro -d -p 80:80 nginx
```
### Docker를 통한 Jenkins 설치  
1. Jenkins 도커 이미지 다운로드
```
$ docker pull jenkins/jenkins
```  
2. Jenkins 컨테이너 실행
```
$ docker run -d -p 8000:8080 --restart=always -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -u root jenkins/jenkins
# -v /var/jenkins:/jenkins:/var/jenkins_home -> 리눅스의 /var/jenkins_home을 local의 /var/jenkins와 공유한다. (볼륨을 준다고 표현) 
```
3. Jenkins 비밀번호 확인
```
$ docker logs [CONTAINER_NAME]
# docker logs jenkins
```  

### Jenkins-Github Webhook  (Github에 변동이 발생하면 자동 빌드)
##### GitHub  
1. Settings -> webhooks -> Addwebhook 클릭
2. Webhooks/Manage webhook Settings  
- Payload URL 작성
ex) http://j5a501.p.ssafy.io:8000/github-webhook/   
 __반드시 주소 뒤 '/github-webhook/'을 작성!!__
- Content Type : application/json
- Which events would you like to trigger this webhook? : 상황에 맞게 클릭
- Active 활성화
- Update webhook 클릭

##### Jenkins   
1. 확인한 비밀번호로 접속 및 계정 생성  
2. Jenkins 관리 -> 플러그인 관리 -> Docker Pipeline, Docker plugin, npm 설치  
3. Jenkins 관리 -> Global Tool Configuration -> NodeJS v14.17.6  
4. 새로운 item -> Freestyle project 생성  
5. 소스 코드 관리에서 Git 선택 후 Repository URL 작성과 Credentials Add 후 선택  
6. 빌드 유발 - GitHub hook trigger for GITScm polling 선택  
7. 빌드 환경 - Provide Node & npm bin /folder to PATH 선택 후 Node 버전 선택  
8. Build - Excute shell  
```
cd backend
chmod 777 mvnw
./mvnw clean package
chmod 777 dockerbuild.sh
sh dockerbuild.sh
```  

### Docker 자동 배포 (도커라이징)
1. backend 폴더 내 Dockerfile 생성
```
FROM openjdk:8-alpine
VOLUME /tmp
EXPOSE 8080
ARG JAR_FILE=target/surlock-0.0.1-SNAPSHOT.jar
ADD ${JAR_FILE} surlock.jar
ENTRYPOINT ["java", "-jar", "surlock.jar"]
```  
2. backend 폴더 내 dockerbuild.sh 작성
```
docker build -t surlock ./  

# 기존 컨테이너 정지 : docker ps -f name=[컨테이너이름] -q | xargs --no-run-if-empty docker container stop  
docker ps -f name=surlock  -q | xargs --no-run-if-empty docker container stop  

# 기존 컨테이너 삭제 : docker container ls -a -f name=[컨테이너이름] -q | xargs -r docker container rm  
docker container ls -a -f name=surlock -q | xargs -r docker container rm  

docker run -d -p 8080:8080 --name surlock surlock
```  

### EC2 서버 내 Nginx 설치 및 실행
1. Nginx 설치
```
$ sudo apt-get update
$ sudo apt-get install nginx
```  
2. Nginx 세팅
```
# 설정 파일의 위치 찾기
$ cd /etc/nginx/sites-available
$ sudo vi default
server {
  listen 80;
  location / {
    root /var/jenkins/workspace/Sur-Lock/frontend/build;
    index  index.html index.htm;
    try_files $uri /index.html;
  }
}
# 이 외 default 내용은 주석 처리 or 지워야함. 
```  
(3) Nginx 실행
```
$ sudo service nginx start
```
