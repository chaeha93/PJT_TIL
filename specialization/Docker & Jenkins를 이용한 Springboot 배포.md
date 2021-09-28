## Docker & Jenkins를 이용한 배포
###### Docker를 통한 Jenkins 설치  
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

2. Jenkins-Github Webhook  (Github에 변동이 발생하면 자동 빌드)
###### GitHub  
(1) Settings -> webhooks -> Addwebhook 클릭
(2) Webhooks/Manage webhook Settings  
- Payload URL 작성
ex) http://j5a501.p.ssafy.io:8000/github-webhook/ 
 * 반드시 주소 뒤 '/github-webhook/'을 작성!!
- Content Type : application/json
- Which events would you like to trigger this webhook? : 상황에 맞게 클릭
- Active 활성화
- Update webhook 클릭

###### Jenkins   
(1) 확인한 비밀번호로 접속 및 계정 생성  
(2) Jenkins 관리 -> 플러그인 관리  
> Docker Pipeline, Docker plugin, npm 설치  
(3) Jenkins 관리 -> Global Tool Configuration -> NodeJS v14.17.6  
(4) 새로운 item -> Freestyle project 생성  
(5) 소스 코드 관리에서 Git 선택 후 Repository URL 작성과 Credentials Add 후 선택  
(6) 빌드 유발 - GitHub hook trigger for GITScm polling 선택  
(7) 빌드 환경 - Provide Node & npm bin /folder to PATH 선택 후 Node 버전 선택  
(8) Build - Excute shell  
```
cd backend
chmod 777 mvnw
./mvnw clean package
chmod 777 dockerbuild.sh
sh dockerbuild.sh
```  

3. Docker 자동 배포 (도커라이징)
(1) backend 폴더 내 Dockerfile 생성
```
FROM openjdk:8-alpine
VOLUME /tmp
EXPOSE 8080
ARG JAR_FILE=target/surlock-0.0.1-SNAPSHOT.jar
ADD ${JAR_FILE} surlock.jar
ENTRYPOINT ["java", "-jar", "surlock.jar"]
```  
(2) backend 폴더 내 dockerbuild.sh 작성
```
docker build -t surlock ./
# 기존 컨테이너 정지
docker ps -f name=surlock  -q | xargs --no-run-if-empty docker container stop
# 기존 컨테이너 삭제
docker container ls -a -f name=surlock -q | xargs -r docker container rm
docker run -d -p 8080:8080 --name surlock surlock
``` 
