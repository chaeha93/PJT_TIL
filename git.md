## git(깃)
  깃은 컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하기 위한 분산 버전 관리 시스템

### 프로젝트를 github에 올리기
```
git init
git add .
git commit -m "commit message"
git remote add origin [github remote repository 주소(대괄호 제외하고)]
git push origin master
```

### git 연결 끊기
- git 연결 끊기
```
git remote remove origin
```
- git init 취소
```
rm -r .git
```
- 현재 연결되어있는 저장소 경로 보기
```
git remote -v
```

### github master -> main
```
git checkout master
git branch main master -f
git checkout main
git push origin main -f
```  

### git push가 안되는 경우 (fatal:refusing to merge unrelated histories)
```
git pull origin 브런치명 --allow-unrelated-histories
```  

### git status에서 특정 파일 modfied(변경사항) 되돌리기
```
$ git checkout -- 파일 이름
# git checkout -- pom
```  

### git branch 'develop'을 Github에 올리기
```
$ git push -u origin develop
``` 


