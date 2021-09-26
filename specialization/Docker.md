## Docker 명령어
- Docker container
```
# 동작중인 컨테이너 확인
$ docker ps
# 컨테이너 목록 출력
$ docker ps -a
# 개별 컨테이너 중지
$ docker container stop {컨테이너ID 앞 3자리}
# 개별 컨테이너 삭제
$ docker container rm {컨테이너ID 앞 3자리}
# 모든 컨테이너 중지
$ docker stop $(docker ps -a -q)
# 모든 컨테이너 삭제
$ docker rm $(docker ps -a -q)
```  
- Docker image
```
# 현재 이미지 확인
$ docker images
# 이미지 삭제
$ docker rmi {이미지 id}
```