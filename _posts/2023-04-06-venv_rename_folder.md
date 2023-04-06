---
published: True
title:  "[python, venv] 폴더이름 바꾸기, venv 경로 재설정"
categories: python
tag: [python, venv, django, error]
---

## 문제

최근 진행하던 Django 프로젝트의 폴더이름을 바꾸고 싶었다.  
Django의 앱 이름을 바꾸려면 일이 귀찮아지는데 이 경우는 아니고  
단지 최상위 폴더의 이름만 바꾸고 싶었다.  
헌데 이름을 바꾸고 나니 설치된 라이브러리를 못찾는 문제가 생겼다.  
어떤 블로그를 보고 해결했는데 내 경우에는 추가 조치가 필요했어서  
이를 해결한 방법을 기록해두려고 한다.  

참고한 블로그는 하단에 기록해 두겠다.

## 해결 방법 1

라이브러리를 전부 프리즈 한뒤 가상환경을 지우고 새로 설치하는 것이다.  

블로그에서 추천하는 방법이었지만  
내 경우는 다른것들이 얽혀있는지 해결이 안됐다.  
하지만 프로젝트가 단순하고 설치된 라이브러리가 적다면 가장 간편한 해결책 일것 같다.  

1. 먼저 라이브러리 목록을 뽑아낸뒤
```
pip freeze > requirements.txt
```
2. 가상환경 폴더를 지운다.  
가상환경 이름이 ```venv```라면 폴더를 지우면 된다.  

3. 그다음 가상환경을 다시 만들고 라이브러리를 재설치한다.  
```
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```


## 해결 방법 2

내 경우 위 1번의 해결책이 해결이 안됐고  
설치된 가상환경의 스크립트를 수정해줬다.  

**쉘 스크립트 수정**  

```venv/bin``` 폴더에 ```activate```가 3개가 있는데  
여러가지 쉘에 대응하는 스크립트라고 한다.  
```
activate
activate.csh
activate.fish
```
나는 이렇게 세가지가 있었다.  

스크립트를 열어보면 ```VIRTUAL_ENV``` 항목이 있는데 이것을 바뀐 폴더이름으로 수정하면 된다.  
```
# unset irrelevant variables
deactivate nondestructive

VIRTUAL_ENV="/path/path/path/path/바뀐폴더이름/venv"
export VIRTUAL_ENV
```
사용하는 쉘만 수정해도 된다고 하는데 나는 그냥 3개 다 수정했다.  



**다른 스크립트들 수정**  

동일한 ```venv/bin``` 경로의 다른 스크립트 파일들을 수정해야 하는데  
설치된 라이브러리에 따라 다를것이다.  
내 경우 6개가 있었다.  

```
django-admin
pip
pip3
pip3.9
sqlformat
wheel
```
파일들을 열어보면 전부 동일하게 ```#!``` 로 시작하는 경로가 있다.  
역시 수정된 경로로 바꿔주면 된다.  

```
#!/path/path/path/path/바뀐폴더이름/venv/bin/python
```

**가상환경 재실행**  

전부 수정했다면 가상환경을 다시 실행하면 된다.  

### 참고
<https://seolin.tistory.com/96>