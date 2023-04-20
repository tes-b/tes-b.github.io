---
published: True
title:  "[git] fatal: Branch rename failed"
categories: [git, github]
tag: [git, github]
---

로컬 폴더를 깃헙 레포지토리와 연결하려면 다음 명령어를 순서대로 실행하면 된다.  

```
git remote add origin <레포지토리 주소>
git branch -M main
git push -u origin main
```

그런데 경로에 먼저 만든 파일들이 있을 경우  
```git branch -M main``` 이 커멘드를 실행하면  
다음과 같은 에러가 나며 실패한다.  
```
fatal: Branch rename failed
```

이럴 때는 변경사항을 모두 커밋하고 다시 실행하면 된다.  
```
git add .
git commit -m 'Init'
```