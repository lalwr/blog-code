# Git 명령어 통해서 저장소 옮기기

```shell
#기존 저장소명 변경
git remote rename origin origin_backup
#변경할 저장소 추가
git remote add origin https://github.com/mandiwise/awesome-new-repo.git
#저장소로 파일 업로드
git push origin master
#기존 저장소 삭제
git remote rm origin_backup
```

