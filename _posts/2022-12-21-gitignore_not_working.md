---
published: True
title:  "[git] .gitignore 동작하지 않을 때"
categories: git
tag: [git, gitignore]
---

.gitignore가 인식이 안되어서 ignore 처리된 파일이 changes에 계속 나올 때 해결방법  

## .gitignore 파일이 제대로 작성되었는지 확인
먼저 .gitignore 파일이 맞게 작성되었는지 다시한번 확인한다.  
경로에 오타가 있거나 .gitignore 파일 이름에 오타가 있는경우가 있다.  
내 경우 .gitigonre 로 파일 이름을 잘못 써서 인식이 안되었던 적이 있었다.  

gitignore 자동 생성 사이트를 이용해도 좋다.  
<https://www.toptal.com/developers/gitignore>

## git의 캐시 초기화
파일명이나 경로명에 문제가 없다면 git의 캐시 문제가 있을 수 있다.  
아래 명령어로 캐시를 전부 삭제하고 다시 모든 파일을 커밋하면 된다.  
```
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```
### 참고
<https://jojoldu.tistory.com/307>