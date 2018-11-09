# Git에서 Single Branch로 clone한 경우 새로운 Branch를 가지고 오고싶을때 명령어

```git
git clone -b {branch-name} --single-branch {github-url}
git remote set-branches --add origin {remote-branch}
git fetch origin {remote-branch}:{local-branch}

ex)
git clone -b lalwr --single-branch https://github.com/lalwr/develop
git remote set-branches --add origin origin/web
git fetch origin web:web
```

