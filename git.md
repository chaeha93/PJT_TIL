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

### git 강제 pull
```
git fetch --all
git reset --hard origin/<브랜치명>
git pull origin <브랜치명>
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

### git 'cannot lock ref' 에러
```
$ git gc --prune=now
$ git remote prune origin
```  

### Gitlab에서 Github로 commit log를 유지하여 Clone
[참고] https://xtring-dev.tistory.com/28
- 100MB가 넘는 크기의 파일을 지닌 저장소 미러링
1. 복사하고자 하는 저장소(Gitlab)의 clone을 생성
```
$ git clone --mirror https://gitlab.com/{주소}.git
```    
2. 커밋 히스토리 내에서 Large file을 찾아 tracking
```
$ git filter-branch --tree-filter 'git lfs track "*.{zip,jar}"' -- --all
```    
3. 새로운 저장소(Github)으로 mirror-push를 진행
```
$ git push --mirror https://github.com/{주소}.git
```  

### git reset
[참고] http://www.devpools.kr/2017/02/05/%EC%B4%88%EB%B3%B4%EC%9A%A9-git-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-reset-revert/
- 돌아가려는 커밋으로 재설정되고, 해당 커밋 이후의 이력은 사라진다.  
```
(1) hard: 돌아가려는 이력 이후의 모든 내용을 지운다.
$ git reset --hard <커밋 고유 번호>

(2) soft: 돌아가려한 이력으로 되돌아가지만, 이후의 내용은 지워지지 않는다.
$ git reset --soft <커밋 고유 번호>

(3) mixed(default): 이력은 되돌아가며 이후 변경된 내용에 대해서는 남아있지만, 인덱스는 초기화된다.
$ git reset --mixed <커밋 고유 번호>
```  
