---
published: True
title:  "venv 가상환경 사용방법"
categories: Etc
tag: [venv]
---

## 가상환경 생성

프로젝트 폴더 에서 가상환경을 생성하면 된다.  
모든 가상환경 이름은 하나로 하는것을 추천하는데  
어떤 프로젝트든 같은 키워드로 가상환경을 실행 할 수 있기 때문이다.  
```
python -m venv <가상환경이름>
```

## 가상환경 활성화
프로젝트 폴더에서 아래 명령어를 실행
```
source <가상환경이름>/bin/activate
```

## 가상환경 비활성화
```
deactivate
```
