## AWS EC2 기본설정

1. Java 설치
```
$ sudo apt-get install openjdk-8-jdk
$ sudo add-apt-repository ppa:webupd8team/java # 패키지가 없으니 패키지 서버를 추가하고 다시 설치(?)
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
$ java -version
# javac -version
```

2. Docker 설치  
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
2-1. Linux 시스템에 Docker Compose 설치
```
# 다음 명령을 실행하여 Docker Compose의 현재 안정적인 릴리스를 다운로드
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 바이너리에 실행 권한을 적용
$ sudo chmod +x /usr/local/bin/docker-compose
# 설치를 테스트
$ docker-compose --version
```

3.  Docker를 통한 MySQL 설치  
참고 : http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/  
1) MySQL Docker 이미지 다운로드
```
$ docker pull mysql:8.0.17
```
2) 도커 이미지 확인
```
$ docker images
```
3) MySQL Docker 컨테이너 생성 및 실행
``` 
$ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=ssafy --name surlock mysql:8.0.17 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

4. Docker를 통한 Nginx 설치
1) Nginx 최신버전 설치 명령어
```
$ sudo docker pull nginx:latest
```
```
$ docker run --name nginx-test -v /home/mint/share/nginx/html:/usr/share/nginx/html:ro -d -p 80:80 nginx
```

5. Docker Container로 Jenkins 사용하기
1) Jenkins Docker Container 실행
```
$ docker run -itd --name [CONTAINER_NAME] -p [HOST_PORT]:8080 jenkins/jenkins:lts
# docker run -itd --name jenkins-surlock -p 8080:8080 jenkins/jenkins:lts
```
2) Jenkins 컨테이너 정상적으로 실행 중인지 확인
```
$ docker ps
```
3) Jenkins 비밀번호 확인
```
$ docker logs [CONTAINER_NAME]
```