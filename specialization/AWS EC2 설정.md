## AWS EC2 설정

1. Java 설치
```
$ sudo apt-get install openjdk-8-jdk
## $ sudo add-apt-repository ppa:webupd8team/java # 패키지가 없으니 패키지 서버를 추가하고 다시 설치(?)
$ java -version
```  

2. Maven 설치
```
$ sudo apt update
$ sudo apt install maven
$ mvn -version
```  

3. Docker 설치  
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

3-1. Linux 시스템에 Docker Compose 설치
```
# 다음 명령을 실행하여 Docker Compose의 현재 안정적인 릴리스를 다운로드
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 바이너리에 실행 권한을 적용
$ sudo chmod +x /usr/local/bin/docker-compose
# 설치를 테스트
$ docker-compose --version
```

4.  Docker를 통한 MySQL 5.7 버전 설치  
참고 : http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/  
(1) MySQL Docker 이미지 다운로드
```
$ docker pull mysql:5.7
```
(2) 도커 이미지 확인
```
$ docker images
```
(3) MySQL Docker 컨테이너 생성 및 실행
``` 
$ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=ssafy --name surlock mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

5. Docker를 통한 Nginx 설치 (하지 않음)
(1) Nginx 최신버전 설치 명령어
```
$ sudo docker pull nginx:latest
```
(2) 실행
```
$ docker run --name nginx-test -v /home/mint/share/nginx/html:/usr/share/nginx/html:ro -d -p 80:80 nginx
```

6. Docker를 통한 Jenkins 설치  
(1) Jenkins 도커 이미지 다운로드
```
$ docker pull jenkins/jenkins
```  
(2) Jenkins 컨테이너 실행
```
$ docker run -d -p 8000:8080 --restart=always -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -u root jenkins/jenkins
# -v /var/jenkins:/jenkins:/var/jenkins_home -> 리눅스의 /var/jenkins_home을 local의 /var/jenkins와 공유한다. (볼륨을 준다고 표현) 
```
(3) Jenkins 비밀번호 확인
```
$ docker logs [CONTAINER_NAME]
# docker logs jenkins
```

7. Jenkins와 Github의 webhook (자동 빌드) 설정 이후 docker로 자동 배포 과정  
(1) Dockerfile 생성
```
FROM openjdk:8-alpine
VOLUME /tmp
EXPOSE 8080
ARG JAR_FILE=target/surlock-0.0.1-SNAPSHOT.jar
ADD ${JAR_FILE} surlock.jar
ENTRYPOINT ["java", "-jar", "surlock.jar"]
```  
(2) dockerbuild.sh 작성
```
docker build -t surlock ./
docker ps -f name=surlock  -q | xargs --no-run-if-empty docker container stop
docker container ls -a -f name=surlock -q | xargs -r docker container rm
docker run -d -p 8080:8080 --name surlock surlock
```  
(3) Jenkins 프로젝트 내 Execute shell
```
cd backend
chmod 777 mvnw
./mvnw clean package
chmod 777 dockerbuild.sh
sh dockerbuild.sh
```  

8. EC2 서버 내 Nginx 설치 및 실행  
(0) Nginx 삭제
```
$ sudo apt-get remove nginx
```  
(1) Nginx 설치
```
$ sudo apt-get update
$ sudo apt-get install nginx
```  
(2) Nginx 세팅
```
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
# 다른 내용 반드시 주석하기!
```  
(3) Nginx 실행
```
$ sudo service nginx start
```
