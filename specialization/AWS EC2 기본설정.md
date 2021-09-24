## AWS EC2 기본설정

1. Java 설치
```
$ sudo apt-get install openjdk-8-jdk
$ java -version
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
 
