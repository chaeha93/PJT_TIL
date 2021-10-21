## Docker & Jenkins를 이용한 Spring Boot 배포  

### Docker를 통한 Jenkins 설치  
1. Jenkins 도커 이미지 다운로드
```
$ docker pull jenkins/jenkins
```  
2. Jenkins 컨테이너 실행
```
$ docker run -d -p 8000:8080 --restart=always -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker --name jenkins -u root jenkins/jenkins
# -v /var/jenkins:/jenkins:/var/jenkins_home -> 리눅스의 /var/jenkins_home을 local의 /var/jenkins와 공유한다. (볼륨을 준다고 표현) 
# -v $(which docker):/usr/bin/docker -> 도커빌드sh 안에 도커 명령어가 상대 경로이기 때문에, 절대 경로로 넣은 후 패스 설정 후 실행하는 방법
sh실행할때 실행하는 사용자(ubuntu) 프로파일을 가져오는게 아니라 젠킨스 프로파일을 가져오기 때문에 패스가 설정되어 있지 않아서 젠킨스에서 docker을 못 찾는 오류가 발생할 수 있음.
```
3. Jenkins 비밀번호 확인
```
$ docker logs [CONTAINER_NAME]
# docker logs jenkins
```  

### Jenkins-Gitlab Webhook  (Gitlab에 변동이 발생하면 자동 빌드)
###### Jenkins   
1. 확인한 비밀번호로 접속 및 계정 생성  
2. Jenkins 관리 -> 플러그인 관리 -> Docker Pipeline, Docker plugin 설치  
3. 새로운 item -> Freestyle project 생성  
4. 소스 코드 관리에서 Git 선택 후 Repository URL 작성과 Github 계정 Credentials Add 후 선택  
5. 빌드 유발 - GitHub hook trigger for GITScm polling 선택  
6. Build - Excute shell  
```
cd backend
chmod 777 mvnw
./mvnw clean package
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
